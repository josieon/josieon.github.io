---
layout: post
title: Singleton Pattern
---

# Singleton Pattern

싱글톤 패턴은 디자인 패턴들 중에서 가장 개념적으로 간단한 패턴이다.

하지만 간단한만큼 이 패턴에 대해 코드만 던져주고 끝내버리는 경우가 있어, 어디에 쓰이는지 어떠한 문제가 있는지 제대로 알지 못하고 얼렁뚱땅 넘어가버리는 것이 많아 보인다.

이번 시간에는 기술면접의 단골 질문이면서 간단하지만 결코 간단하지 않은 디자인 패턴의 싱글톤 패턴에 대해 꼼꼼하게 알아가 보는 시간을 가져보자

싱글톤 패턴이란 단 하나의 유일한 객체를 만들기 위한 코드 패턴이다.

쉽게 말하자면 메모리 절약을 위해, 인스턴스가 필요할 때 똑같은 인스턴스를 새로 만들지 않고 기존의 인스턴스를 가져와 활용하는 기법을 말한다.

우리가 전역 변수라는 걸 만들어 이용하는 이유는, 똑같은 데이터를 메서드마다 지역 변수로 선언해서 사용하면 무의미하기도 하고 낭비이기 때문에 전역에서 한번만 데이터를 선언하고 가져와 사용하면 효율적이기 때문이다.

이러한 개념을 그대로 클래스에 대입한 것이 싱글톤 패턴이라고 이해하면 된다.

따라서 보통 싱글톤 패턴이 적용된 객체가 필요한 경우는 그 객체가 리소스를 많이 차지하는 역할을 하는 무거운 클래스일때 적합하다.

대표적으로 데이터베이스 연결 모듈을 예로 들 수 있는데, 데이터베이스에 접속하는 작업(I/O 바운드)은 그 자체로 무거운 작업에 속하며 또한 한번만 객체를 생성하고 돌려쓰면 되지 굳이 여러번 생성할 필요가 없기 때문이다.

이밖에도 디스크 연결, 네트워크 통신, DBCP 커넥션풀, 스레드풀, 캐시, 로그 기록 객체 등에 이용된다.

이러한 객체들은 또 새로 만들어서 사용될 일도 없거니와 사용해도 리소스 낭비일 뿐이다. 따라서 어플리케이션에서 유일해야 하며 유일한 것이 좋은 것을 싱글톤 객체로 만들면 된다고 보면 된다.

! 실제로 안드로이드 스튜디오 자바 SDK에서 각 액티비티들이나, 클래스마다 주요 클래스들을 하나하나 전달하는게 번거롭기 때문에 싱글톤 클래스를 만들어 어디서든 접근하도록 설계되었다.

## 싱글톤 패턴 구현 원리

클래스에 싱글톤 패턴을 적용하는 것은 전혀 복잡하지 않다.

어렵게 생각할 필요없이 싱글톤으로 이용할 클래스를 외부에서 마구잡이로 `new` 생성자를 통해 인스턴스화 하는 것을 제한하기 위해 클래스 생성자 메서드에 `private` 키워드를 붙여주면 된다.

그리고 위 그림에서 볼 수 있듯이 `getInstance()`라는 메서드에 생성자 초기화를 해주어, 만일 클라이언트가 싱글톤 클래스를 생성해서 사용하려면 `getInstance()`라는 메서드 실행을 통해 `instance` 필드 변수가 null일경우 초기화를 진행하고 null이 아닐경우 이미 생성된 객체를 반환하는 식으로 구성하면 된다.

다음은 싱글톤으로 구성된 클래스를 외부에서 불러오는 예제이다.

정적 메서드로 `getInstance()`를 통해 객체를 불러와 변수에 저장하고 이를 출력해보면 똑같은 객체 주소를 가지고 있는 걸 볼 수 있다.

즉, 객체 하나만 생성하고 여러 변수에 불러와도 돌려쓰기를 한 것이다.

```java
public class Main {
    public static void main(String[] args) {

        // Singleton.getInstance() 를 통해 싱글톤 객체를 각기 변수마다 받아와도 똑같은 객체 주소를 가리킴
        Singleton i1 = Singleton.getInstance();
        Singleton i2 = Singleton.getInstance();
        Singleton i3 = Singleton.getInstance();

        System.out.println(i1.toString()); // Singleton@1b6d3586
        System.out.println(i2.toString()); // Singleton@1b6d3586
        System.out.println(i3.toString()); // Singleton@1b6d3586

        System.out.println(i1 == i2); // true
    }
}
```

! 그렇다면 그냥 평범한 클래스 만들고 한번만 인스턴스화 한 뒤 사용안하면 싱글톤 패턴과 무슨 차이가 있겠나 싶겠지만, 본래 개발자는 사람이고 사람은 항상 실수를 반복하는 생물이라 이러한 문법적인 법률을 통해 구조적으로 제한하는 것으로 이해하면 된다.

# 싱글톤 패턴 구현 기법 종류

지금부터 자바 코드를 예제로 들어 싱글톤 패턴에 대해 알아보는 시간을 가져볼 것이다.

어떠한 목적을 구현하기 위해 코드 패턴이라는 것이 꼭 한가지만 있는 것은 아니다. 여러가지 코드 기법들이 존재하여 이들 중 가장 최적화된 패턴을 상황에 맞게 사용하는 것이 핵심이다.

다음은 싱글톤 패턴을 구현하는 코드 기법들이다.

총 7가지가 있으며 이들은 모두 싱글톤을 지향한다는 점에서는 같지만 각기 코드 패턴마다 장단점이 존재한다. 각 순서마다 1번부터 조금씩 단점을 보완하는 식으로 흘러가는 것으로 보면 된다.

1. Eager Initialization
2. Static block Initialization
3. Lazy Initialization
4. Thread safe Initialization
5. Double-Checked Initialization
6. Bill Pugh Solution
7. Enum 이용

만일 지금 당장 사용할 검증된 싱글톤 코드 패턴을 봐야한다면, Bill Pugh Solution과 Enum 이용 예제만 보면 된다.

다만 우리가 전공에서 컴퓨터의 역사부터 차근차근 배우는 이유와 같이, 하나의 목적을 이루고자 탄생한 다양한 자바의 코드 패턴들을 보며 순서대로 코드들이 최적화되고 발전되어져 가는 과정을 차근차근 학습한다면, 나중에 코드를 설계할때 이러한 경험들이 분명 도움이 될 것이다.

또한 자바 개발자라면, 싱글톤을 구현하는 7가지 방법을 학습하면서 나도 몰랐던 여러 기법들을 배워가는 점이 쏠쏠하기 때문에 모두 학습하는걸 권장한다.

## Eager Initialization

- 한번만 미리 만들어두는, 가장 직관적이면서도 심플한 기법
- `static final`이라 멀티 쓰레드 환경에서도 안전함
- 그러나 static 멤버는 당장 객체를 사용하지 않더라도 메모리에 적재하기 때문에 만일 리소스가 큰 객체일 경우, 공간 자원 낭비가 발생함
- 예외 처리를 할 수 없음

! 만일 싱글톤을 적용한 객체가 그리 크지 않은 객체라면 이 기법으로 적용해도 무리는 없다.

```java
class Singleton {
    // 싱글톤 클래스 객체를 담을 인스턴스 변수
    private static final Singleton INSTANCE = new Singleton();

    // 생성자를 private로 선언 (외부에서 new 사용 X)
    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

(https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

## Static block initialization

## Lazy Initialization

### 멀티 쓰레드 환경에서의 치명적인 문제점

## Thread safe initialization

## Double-Checked Locking

## Bill Pugh Solution(LazyHolder)

## Enum 이용

# 싱글톤 패턴은 안티 패턴?

## 싱글톤의 문제점

### 1. 모듈간 의존성이 높아진다

### 2. SOLID 원칙에 위배되는 사례가 많다

### 3. TDD 단위 테스트에 애로사항이 있다