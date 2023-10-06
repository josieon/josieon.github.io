---
layout: post
title: RestTemplate Error Handling
---

## 상황

- RestTemplate을 이용해 통신을 하는 상황
- 반복문을 통해 여러 건의 통신을 진행하고, 통신건마다 response에 API 명세를 통한 에러 내용이 담김 - response code는 200
- 통신의 결과를 담아 최종적으로 결과를 출력 - 성공/실패 X건, 실패 원인 : XXX
- 하지만, 통신 자체에서 에러가 발생하면(4XX, 5XX) 반복문이 종료되어 결과 출력이 되지 않음
- 에러가 발생하기 전까지 통신되었던 결과도 사라짐
- RestTemplate 클래스를 까보니 DefaultResponseErrorHandler에서 Error를 Handling하는데 Exception만 던지고 끝.
- 따라서, Exception이 발생해서 반복문이 종료되고, method가 exit된 것 같다.

## 방법 1 : catch문

- 유저 서버로부터 4XX, 5XX 등의 status code가 오면 DefaultResponseErrorHandler에서 ErrorException을 던지고 해당 catch 문으로 들어가게 된다.
- status code에 따라 적절하게 예외를 처리해주면 된다.

```java
public User getUser() {
    try {
        ResponseEntity<User> response = restTemplate.exchange(
            "https://[API 서버]",
            HttpMethod.GET,
            null,
            User.class
        );
        return Objects.requireNonNull(response.geet)
    }
}
```

- DefaultResponseErrorHandler의 HTTP status별 exception
    - HttpClientErrorException : in the case of HTTP status 4XX
    - HttpServerErrorException : in the case of HTTP status 5XX
    - UnknownHttpStatusCodeException : in the case of an unknown HTTP status

## 방법 2 : ResponseErrorHandler

- DefaultResponseErrorHandler를 extends하고 다음과 같이 handlerError를 override하여 정의하면 된다.
- DefaultResponseErrorHandler 대신 ResponseErrorHandler를 implements해서 사용해도 된다.
    - 이 경우 hasError()와 handlerError를 모두 정의해야 한다
- 그리고 정의한 ResponseErrorHandler를 RestTemplate에 set해주면 된다.

```java
public class RestTemplateResponseErrorHandler extends DefaultResponseErrorHandler {

    @Override
    public void handleError(ClientHttpResponse response) throws IOException {
        if (response.getStatusCode() == HttpStatus.PRECONDITION_FAILED) {
            throw new CUSTOM_EXCEPTION(msg);
        }
    }
}
```

```java
public class RestApi {

    private static RestTemplate restTemplate = new RestTemplate();

    static {
        restTemplate.setErrorHandler(new RestTemplateResponseErrorHandler());
    }

    public User getUser() {
        try {
            ResponseEntity<User> response = restTemplate.exchange(
                "Https://[API 서버]",
                HttpMethod.GET,
                null,
                User.class
            );
            return Objects.requireNonNull(response.getBody());
        } catch (HttpClientErrorException e) {
            throw e;
        }
    }
}
```