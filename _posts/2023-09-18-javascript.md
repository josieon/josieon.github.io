---
layout: post
title: javascript
---

# JavaScript

## 자바스크립트 개념 이해

### 1. 자바스크립트란?

`JavaScript`는 정적인 HTML 콘텐츠를 프로그램 구현을 통해 동적으로 변경하거나 사용자와의 상호작용을 담당하게 된다.  
HTML이나 CSS와 달리 자바스크립트는 C언어, 자바와 같은 일반 프로그램 언어와 비슷한 구조를 가지고 있다.  
자바스크립트는 객체(Object) 기반의 스크립트 언어로 기본적으로는 웹 브라우저에서 해석되는 인터프리터 언어이며 Node.js와 같은 프레임워크를 사용하면 서버 프로그래밍에도 사용할 수 있다.  
현재 컴퓨터나 스마트폰 등에 포함된 웹 브라우저에는 자바스크립트 인터프리터가 내장되어 있다.

#### 자바스크립트 특징

- 동적이며 타입을 명시할 필요가 없는 인터프리터 언어이다.
- 객체지향 프로그래밍과 함수형 프로그래밍을 모두 표현할 수 있다.
- HTML의 내용, 속성, 스타일을 변경할 수 있다.
- 이벤트를 처리하고 사용자와의 상호작용을 가능하게 한다.
- Ajax 기술을 이용해 서버와 실시간 통신기능 제공

#### 자바스크립트 버전

Vue, Angular, React와 같은 프론트엔드 프레임워크에서부터 Node.js를 기반으로 하는 서버 프로그래밍 영역까지 자바스크립트까지 널리 사용되고 있다.  
ECMAScript는 자바스크립트의 표준규격으로 볼 수 있으며 ES5부터 기존과 다른 새로운 구조가 많이 추가 되었다.

#### ES5(2009) 주요 특징

- 일반적으로 자바스크립트의 가장 기본이 되는 버전
- 배열에 forEach, map, filter, reduce, some, every 등의 메서드 지원
- Object에 대한 getter/setter 지원
- JSON 지원

#### ES6(2015) 주요 특징

- 2019년 기준 가장 보편적으로 적용되는 JS 버전
- 대부분의 브라우저에서 지원되는 사실상의 표준 규격
- let, const 키워드 추가
- arrow 문법 지원
- iterator/generator 추가
- module import/export 추가
- Promise : 자바스크립트 비동기 콜백 문제 해결

#### ES8(2017) 주요 특징

- async/await 비동기 관련 처리 추가
- Object, String 등에 기능 추가

### 2. Vue, Angular, React

#### 1) AngularJS

구글에서 만들었으며 MIT 라이센스로 누구나 무료로 사용 가능.

#### 주요 특징

- 기존보다 훨씬 적은 코드로 원하는 기능을 구현할 수 있음.
- 양방향 데이터 바인딩
- MVC, MVVM을 지원해 구성요소의 명확한 분리 가능

AngularJS2에서 TypeScript가 기본으로 사용되고 구성방식, 아키텍처, 도구 등의 변경으로 기존 개발자들이 적응하기 어렵게 되었으며 점점 거대화되는 구조와 학습곡선 그리고 새로운 프레임워크의 등장으로 사용자가 줄어들었다.

#### 2) React

페이스북에서 만들었으며 UI 라이브러리에 특화되어 있으며 재사용 가능한 UI 컴포넌트 생성을 지원한다. 또한 `가상돔(Virtual DOM)`이라는 개념을 사용해 보다 빠르게 웹 콘텐츠의 동적 처리를 가능하게 한다.

#### 주요 특징

- Virtual DOM을 사용
- JSX(JavaScript XML)를 사용한다.(Vue.js 등 여러곳에서 사용)
- Airbnb, Netflix, Dropbox, Twitter 등
- 프레임워크 보다는 View 라이브러리에 가까움

#### 3) Vue.js

`Vue`는 사용자 인터페이스를 만들기 위한 진보적인 프레임워크로 다른 단일형 프레임워크와 달리 점진적으로 채택할 수 있도록 설계되어있다. React, Angular의 장점을 통합해 빠른 속도로 성장하고 있는 가장 최신의 프레임워크

#### 주요 특징

- Virtual DOM을 사용
- 전반적인 문서화가 잘되어 있고 특히 한글문서가 잘 정리되어 있다.
- SSR이 React보다 훌륭하다.
- 부분적 도입에서부터 전체로의 적용까지 유연하게 적용 가능

### 3. Node.js

`Node.js(노드)`는 자바스크립트를 백엔드 프로그램 개발에 사용할 수 있도록 만들어진 서버사이드 자바스크립트 런타임이다. 정확하게는 `Chrome V8 JavaScript Engine`에 기반한다.  
기본적으로 스레드를 사용하지 않도록 설계되었지만 필요하다면 다수의 CPU 코어에 로드밸런싱이 가능한 구조로 경량의 빠른 웹서버 개발에 적합하다.  
별도의 웹서버 소프트웨어 없이 자체적으로 웹서버 구동이 가능하고 자바스크립트 문법을 통해 서버 프로그램 구현이 가능한 구조다.  
기존의 대형 시스템들을 여러개의 독립된 소형 서비스로 분리하는 MSA의 확산으로 node.js의 활용이 급증하게 되었다.

#### 주요 특징

- 이벤트 기반, 논 블로킹 I/O모델 사용
- npm이라고 하는 패키지 매니저 사용(65만개 이상의 오픈소스 패키지)
- 빠른 처리속도와 뛰어난 확장성

#### Node.js 주요 적용 분야

- 입출력이 잦은 어플리케이션
- 데이터 스트리밍 어플리케이션
- 데이터를 실시간으로 다루는 어플리케이션
- 경량의 RestfulAPI 기반 어플리케이션
- 싱글페이지 어플리케이션

#### Express.js

Node.js기반의 웹 개발 프레임워크로 컨트롤러(Controller)와 뷰(View)를 처리하기 위한 미들웨어 구조를 제공한다고 볼 수 있다. URI에 따른 요청을 실제 구현과 연결해주고 필요한 경우 뷰 템플릿으로 전달하는 등의 프로그램 구조를 생성하고 관리할 수 있도록 한다.

Node.js에서 웹 개발을 할 때 꼭 필요한 패키지이다.

## 기본 문법

### 1. 소스코드 위치

자바스크립트는 기본적으로 HTML 문서의 `<head></head>`사이에 위치한다. 그러나 그 외 위치에 둘수도 있고 외부파일이나 다른 서버를 통해 참조하는 방식으로도 사용할 수 있다.

#### 1) 내부 자바스크립트

HTML 문서 내부에 자바스크립트 소스코드를 두는 유형.

```HTML
<script>
    alert('hello world');
</script>
```
- 현재 HTML 파일의 문서구조(DOM)에 쉽게 접근 가능
- 현재 화면에 동적인 요소를 부여하는 자바스크립트가 같은 소스파일에 위치하기 때문에 코드 관리와 이해가 쉬움
- 자바스크립트 소스가 복잡해진느 경우 관리가 어려움
- 공통된 기능을 만들기 어렵고 코드의 재활용이 어려움

#### 2) 외부 자바스크립트

HTML 문서 외부에 별도의 파일을 생성하고 HTML에서 불러와 사용하는 방식. 이때 자바스크립트 파일의 위치는 HTML과 동일한 서버 혹은 외부 서버일수도 있다.

```javascript
//external.js 파일
function printDate() {
    alert('hello world');
}
```
```HTML
<head>
    <script src="../external.js"></script>
    <!-- 외부 서버의 js파일 참고인 경우 다음과 같이 사용-->
    <script src="https://www.cdn.com/myjs/external.js"></script>
</head>
```

#### 3) 소스파일 위치 결정

브라우저는 HTML의 구조와 CSS 스타일을 렌더링하는 도중 자바스크립트를 만나게 되면 이에 대한 해석과 구현이 완료될때까지 브라우저 렌더링을 멈추게 됩니다. 즉, 자바스크립트의 삽입 위치에 따라 스크립트 실행순서와 브라우저 렌더링에 영향을 미치기 때문에 다음 사항을 고려해 적절한 위치선정이 필요합니다.

#### `<head></head>`

- 브라우저 렌더링에 방해가 될 수 있으며 무거운 스크립트가 실행되는 경우 오랫동안 화면이 보여지지 않을 수 있음
- 문서를 초기화하거나 설정하는 가벼운 스크립트들을 주로 사용
- 문서의 DOM(Document Object Model) 구조가 필요한 경우 HTML이 모두 로드된 이후 실행되어야 하므로 `window.onload`와 같은 로드 이벤트가 추가되어야함

#### `<body></body>`

- 태그 내 모든 위치에 둘 수 있음
- 웹페이지 로딩이 완료된 다음 실행하기 위해 일반적으로는 `</body>`바로 앞에 위치
- 이 경우 문서의 DOM 구조가 완료된 시점에 실행되기에 별다른 추가설정이 필요없음

#### `내부 자바스크립트 VS 외부 자바스크립트`

- 비교적 간단한 코드로 구성되며 현재 파일에만 적용되는 경우 내부 자바스크립트를 이용
- 공통기능 구현이나 소스가 길어지면 외부 자바스크립트로 관리함
- 공통 라이브러리로 개발된 경우 CDN(Content Delivery Network)을 통해 외부 서버로부터 참조해서 사용함

### 2. 변수와 자료형

자바스크립트는 자료형이 고정되어 있지 않은 동적타입 언어이다. 따라서 변수를 선언할때 별도의 자료형을 명시하지 않아도 된다.

#### 1) 자료형

내부적으로는 Primitive(기본형)와 Object(객체형)이 있으며 각각 다음과 같다.

#### `Primitive`

- Boolean: true, false
- null: 빈 값
- undefined: 값을 할당하지 않은 변수
- Number: 숫자형으로 정수와 부동 소수점, 무한대 및 NaN 등
- String: 문자열

#### `Object`

- Reference 타입
- 클래스 뿐만 아니라, 배열과 함수, 사용자 정의 클래스도 모두 Object
- JSON(JavaScript Object Notation)의 기본 구조

#### 2) 변수 선언

- 변수이름은 대소문자를 구별
- 여러 변수를 한번에 선언할 수 있다
- 지역변수와 전역변수가 있다
- 기본적으로 소문자로 시작되는 Camel Case 사용

#### `var, let, const`

> ES6 이전에는 `var`만 존재했으며 function-scoped로 인해 다른 언어들과 다른 문제가 있었다

- `var`는 지역변수 개념으로 함수 범위에서 유효함
- `var`를 선언하지 않으면 자동으로 전역변수가 됨
- `let`과 `const`는 ES6에서 등장한 block-scoped 변수 선언
- `let`은 값의 재할당이 가능하고 `const`는 불가능(immutable)
- `const`로 선언된 배열이나 객체의 경우 새로운 객체로 재할당하는 것은 안되고, 배열값의 변경/추가, 객체의 필드 변경 등은 가능

```javascript
var foo = 'foo1';
console.log(foo); // foo1
 
if (true) {
  var foo = 'foo2';
  console.log(foo); // foo2
}

console.log(foo); // foo2
```
```javascript
let foo = 'foo1';
const bar = 'bar1';
console.log(foo); // foo1
 
if (true) {
  let foo = 'foo2';
  console.log(foo); // foo2
  console.log(bar); // bar1
}
 
console.log(foo); // foo1
bar = 'bar2'; // error
```

#### `hoisting`

> 호이스팅은 끌어올리기라는 사전적 의미를 가지고 있다. 자바스크립트에서 모든 변수 선언은 호이스트되고 함수의 경우 선언형식은 호이스팅되며 변수에 할당된 형태는 호이스팅 되지 않는다

자바스크립트 변수에 있어 변수의 선언이 위치와 상관없이 최상위 레벨로 끌어올려진다고 이해할 수 있다.

```javascript
if (true) {
    console.log(foo); // undefined
    var foo = 'foo1';
    console.log(foo); // foo1
}
```
- 일반적인 언어인 경우 `foo`변수는 첫번째 출력문에서 선언되기 전 상태로 에러가 발생해야 함
- 자바스크립트는 `var foo`가 호이스트되어 변수는 선언되고 값이 할당되지 않은 상태가 됨
```javascript
var myVar;

function myVar() {
    ...
}
console.log(typeof myVar); // function
```
- myVar라는 변수를 먼저 선언한 상태에서 동일 이름의 함수를 정의
- 함수 선언이 호이스팅되어 myVar 변수에 할당

> 경우에 따라 호이스팅은 사소한 문제를 일으키지 않아 유용할 수 있으나 복잡한 코드에서는 오류 가능성이 높으므로 변수 선언시에는 var, let, const 등을 명확히 구분해서 사용하는 것이 좋다.

#### `String 변수`

일반적인 프로그램 언어들은 문자열을 표현할때 `큰따옴표(" ")`를 사용하는데 자바스크립트는 `" ", ' '`를 모두 사용할 수 있다

### 3. 출력문

자바스크립트는 HTML 문서를 통해 웹브라우저에서 출력되므로 따로 출력문이 존재한다기 보다는 HTML 문서의 구성요소에 동적으로 출력하거나 웹브라우저의 경고창을 이용해 출력하는 형태가 출력문으로 볼 수 있다. 물론 웹 브라우저의 콘솔창을 통해 특정 메시지를 출력할수도 있다.

#### 1) HTML 문서에 출력

> HTML 문서에 출력하는 형태로 페이지가 모두 로딩된 다음에 실행하면 원래 있던 HTML 화면내용은 지워지게 된다

```HTML
<script>
    document.write(5 + 6);
</script>

<body>
    <h2>Hello World</h2>
</body>
```

#### 2) HTML 문서의 특정부분에 출력

> HTML 문서의 특정요소를 찾아 해당 콘텐츠를 대체해 출력한다. 자바스크립트에서 가장 보편적으로 HTML 문서를 동적으로 핸들링하는 방법

```HTML
<script>
    document.getElementById("result").innerHTML = 5 + 6;
</script>

<body>
    <h2>Hello World</h2>
    <div id="result">
    </div>
</body>
```

#### 3) Alert 창을 이용한 출력

> 웹브라우저에서 오픈되는 조그만 경고창(alert)을 이용한 출력이다. 보통 프로그램에서 에러, 경고, 사용자 입력을 위해 많이 사용한다.

```HTML
<script>
    alert(5 + 6);
</script>
```

#### 4) 브라우저 콘솔창을 이용한 출력

> 자바스크립트 코드에서 진행상황을 출력하거나 개발을 위해 참고하기 위한 값들을 출력하기 위한 용도로 사용한다.

웹 브라우저의 콘솔창에 나타나게 된다.

```HTML
<script>
    console.log(5 + 6);
</script>
```

### 4. 연산자와 제어문

#### 1) 연산자

#### `비교 연산자`

|연산자|연산자 설명|
|:---:|:---:|
|==|값이 같은지 비교|
|===|값과 타입이 모두 같은지 비교|
|!=|같지 않음|
|!==|값이나 타입이 같지 않음|
|>|크다|
|<|작다|
|?|3항 연산자|

#### 2) 조건문

#### `if, else if`

> 특정 조건이 참인 경우 수행되는 코드 블럭을 정의합니다. else와 결합해 조건 범위나 조건을 세분화하는 것이 가능하며 AND, OR 연산을 함께 사용할 수 있습니다.

#### `switch`

> 입력값에 따라 처리를 다르게 하는 경우 사용한다.

#### 3) 반복문

#### `for`

> 기본 구조는 시작, 종료조건, 증감식을 가지는 형태

#### `while`

> 조건이 성립하는 경우 계속 반복하는 구문

#### `forEach`

> ES5에서 사용가능하게 된 문법으로 배열의 모든 원소에 대해 특정 코드블럭을 수행할 수 있는 방법

```javascript
const colors = ['red', 'blue', 'green'];
colors.forEach(function(value) {
    console.log(value);
});
```

`ES6`에서는 `arrow 연산자`를 통해 다음과 같이 간결하게 사용할수 있습니다

```javascript
const colors = ['red', 'blue', 'green'];
colors.forEach( value => console.log(value));
```

#### `for-in, for-of`

> 최근 다른 언어들에 도입된 -in 형태의 구문이다. 다만 -in의 경우 배열원소의 값에 접근할 수 없고 키 혹은 인덱스만 접근이 가능하므로 다른 언어에서의 -in과 같은 형태로 사용하려면 for-of를 사용해야 한다

```javascript
const colors = ['red', 'blue', 'green'];
for (var index in colors) {
    console.log(colors[index]);
}
for (var value of colors) {
    console.log(value);
}
```

`for-in`

- 객체의 모든 열거 가능한 속성에 대해 반복
- 즉, 배열 뿐만 아니라 일반적인 객체의 속성들을 모두 반복할 때도 사용가능
- 모든 객체의 key(배열의 경우 인덱스)에 접근할 수 있지만 value에 접근할 수는 없음

`for-of`

- 반복 가능한(Iterable)객체의 값을 순환. 배열 이외 문자열 데이터(유니코드 이모지 포함) 처리도 가능
- `ES6`에 새로 추가된 Map, Set에도 적용 가능
- Object를 대상으로 하지 않으며 객체의 속성을 순회하려면 `for-in`을 사용
- Object를 사용할 경우 object.keys()로 키 값을 구해서 순회하면서 출력 가능

### 5. 배열과 자료구조

#### 1) 배열

배열의 선언은 `[]`나 `new Array()`로 생성하고 크기의 제약이 없으며 하나의 배열에 서로 다른 타입의 변수가 들어갈 수 있다.

#### `배열의 주요 메서드`

`데이터 변경`

- `push()`: 배열의 끝에 값을 추가
- `pop()`: 배열 마지막 값을 제거
- `shift()`: 배열 데이터를 왼쪽으로 하나씩 밀어 맨앞 값을 제거
- `splice()`: 배열값을 추가하거나 제거해서 반환
- `reverse()`: 배열을 역순으로 재배치
- `sort()`: 배열 데이터를 정렬

`배열의 일부를 반환`

- `concat()`: 두개의 배열을 합침
- `join()`: 배열 데이터 사이에 원하는 문자열을 넣어 구분자로 사용
- `slice()`: 배열의 일부를 지정해서 가져옴

`데이터 순회`

- `map()`: 모든 배열 데이터마다 반복 처리가 필요한 경우 사용
- `filter()`: 특정 조건을 만족하는 데이터만 처리할 경우 사용
- `reduce()`: 모든 데이터를 순회하면서 누적 연산이 필요한 경우 사용

#### 2) Map, Set

> `ES6`에서 새롭게 추가된 자료구조

#### `Map`

> Map은 new Map()으로 선언하고 map.set()을 이용해 키와 값을 추가

자바스크립트의 Map은 `Key:Value`쌍으로 구성된 `Object`와 비슷한 구조로 볼 수 있지만 다음과 같은 차이가 있다

- Object의 키는 `문자열`이며, Map의 키는 `모든 값`을 가질 수 있다
- Object의 크기를 `수동으로 추적`해야하지만, Map은 크기를 쉽게 얻을 수 있다
- Map은 삽입된 순서대로 반복된다.
- 객체(Object)에는 prototype이 있어 Map에 기본 키들이 있다

> `언제 Object 혹은 Map을 사용할까?`  
> 실행 시점까지 키를 알 수 없고, 모든 키가 동일한 Type이며 모든 값들이 동일한 Type일 경우에는 Object를 대신해서 Map을 사용하는 것이 좋다. 만일 각 개별 요소에 대해 적용해야 하는 로직이 있다면 Object를 사용하길 권장한다.

#### `Set`

> Set은 중복된 값을 허용하지 않고 입력된 순서에 따라 데이터를 저장하는 자료구조이다. set.add()를 이용해 데이터를 추가할 수 있고 데이터 관리를 위한 메서드가 제공된다.

배열과 유사하지만 다음과 같은 차이가 있다.

- `indexOf()`를 사용하여 배열내에 특정 요소가 존재하는지 확인하는 것은 느리다
- 배열에선 해당 요소를 배열에서 잘라내야 하는 반면 Set은 삭제 기능을 제공한다
- NaN은 배열에서 indexOf메서드로 찾을 수 없다

### 6. 함수

함수의 개념은 대부분의 프로그램 언어에서 존재하지만 특히 자바스크립트는 함수를 변수에 할당할 수 있으며 인자로 함수를 전달하거나 콜백함수를 구현하는 형태 등 다양한 함수 활용을 보여주고 있다.  
이러한 특징들과 함께 최근의 함수형 프로그래밍 개념도 자바스크립에서 지원되고 있다.

#### `함수선언`

> 기본적으로 function 키워드로 선언하며 인자를 가질 수 있다. 자바스크립트는 타입을 명시하지 않기 때문에 리턴값의 유무와 상관없이 함수 선언부에 리턴 데이터에 대한 선언은 없다. 필요시 함수 바디에서 return문을 사용해 값을 반환하면 된다.

- 인자의 매개변수 역시 타입없이 갯수에 따라 변수명을 나열
- 함수안에 사용되는 변수는 block-scoped인 let이나 const 사용을 권장
- 함수안에 또다른 함수를 정의할 수 있다

#### `First Class Object`

> 자바스크립트에서 함수는 일급객체라고도 하며 다음과 같은 조건을 만족하는 객체를 말한다

- 변수나 데이터 구조안에 담을 수 있다
- 파라미터로 전달할 수 있다
- 반환값(return value)으로 사용할 수 있다
- 할당에 사용된 이름과 관계없이 고유한 구별이 가능하다
- 동적으로 속성 할당이 가능하다

#### `익명함수`

> 익명함수는 이름이 없는 함수로 변수에 함수를 구현하거나 함수를 리턴하는 형태가 가능하며 이를 사용하면 소위 콜백 함수의 구현이 가능해진다

#### `콜백함수`

> 자바스크립트에서 콜백함수는 특히 이벤트 처리에 많이 사용된다. 특정 UI에서 이벤트가 발생되었을때 처리될 코드를 익명함수에 넣고 콜백되어 처리될 수 있도록 하는 형식이다.

```javascript
function square(x, callback) {
    setTimeout(callback, 200, x*x);
}

square(2, function(number) {
    console.log(number);
})
```

#### `자료구조의 값에 함수 사용`

> 앞에서 언급한것처럼 함수를 객체의 속성으로 사용할 수 있으며 Map의 Value 혹은 배열의 원소로도 사용할 수 있다.

### 7. 객체와 클래스

#### 1) 객체(Object)

> 객체지향에서 객체는 우리 눈에 보이는 구체화된 대상을 의미한다. 객체는 속성과 행위로 정의할 수 있으며 객체를 정의할 수 있는 틀을 클래스라고 한다. 클래스는 멤버변수와 메서드로 정의하게 되고 클래스로부터 객체를 생성(인스턴스)해서 사용하는 개념이다.

- `ES6`에서 클래스가 나오기 이전에는 `Object`를 주로 사용해서 객체를 정의했다
- Primitive 타입과 달리 참조형이며 배열도 객체이다
- 객체는 속성의 모음이며 속성은 `Key:Value` 구조를 가진다
- 속성값이 함수인 경우 `Method`라고 표현한다

#### `객체 리터럴을 이용한 객체 생성(JSON)`

> 최근 프로그램 개발에 많이 쓰이는 JSON은 포맷은 JavaScript Object Notation으로 자바스크립트에서 사용하는 객체 표기법을 의미한다.

자바스크립트 객체를 단순화해서 표현하면 `Key:Value`형태의 일종의 맵 혹은 딕셔너리 형태라고 할 수 있다. 앞에서 언급한 것처럼 객체의 값으로 함수를 사용할 수 있기 때문에 클래스와 유사한 형태가 되는 것이다. 객체 이니셜라이저(Object Initializer)라고도 한다

```javascript
let student = {
  'id': 2019101,
  'name': '홍길동',
  'scores': [95,80,91,85],
  'getTotalScore': function() {
    return this.scores.reduce((prev, curr) => prev + curr);
  }
}

console.log(student.getTotalScore());
```

#### `생성자를 이용한 객체 생성`

만일 클래스와 같이 구체적인 값을 가지지 않는 형태를 미리 선언하고 객체를 생성하려면 다음과 같이 생성자 함수를 정의하고 사용할 수 있다

```javascript
function Product(name, price) {
  this.name = name;
  this.price = price;
  this.getInfo = function() {
    return this.name + " , " + this.price;
  }
}

let p1 = new Product("애플 아이폰",1000000);
let p2 = new Product();
p2.name = "삼성 갤럭시"
p2.price = 1100000;

console.log(p1.getInfo());
console.log(p2.getInfo());
```

#### 2) 클래스(Class)

`ES6`에서부터 지원되기 시작했으며 좀 더 객체지향 개념에 가까운 객체 사용을 가능하게 한다. 앞에서 생성자 기반의 객체 선언을 살펴보았는데 객체의 함수를 `this.getInfo`와 같이 정의하는 방법은 사실 좋은 방법이 아니다.  
이유는 객체를 생성할때마다 동일한 함수도 매번 생성되기 때문에 이를 효과적으로 처리하려면 함수의 경우 `Product.prototype.getInfo = function() {}`처럼 정의해 주어야 합니다만 이경우 객체 선언부와 함수 선언부가 분리되어 코드 관리가 불편하게 된다.  
당연히 클래스를 사용하면 이런 문제점들을 해결할 수 있다.

#### `클래스 구조`

> 새로운 class 키워드로 선언한다. 클래스 이름은 보통 대문자로 시작한다.

```javascript
class 클래스명 {
  constructor(인자..) {
    this.var = 인자;
    ...
  }

  메서드() {
  }

  get 메서드() {
    return ..
  }

  set 메서드() {

  }
}
```
- 함수와 마찬가지로 클래스를 익명으로 선언할수도 있고 변수에 할당할수도 있다
- new 연산을 통해 호출되는 것은 클래스 이름이 아니라 생성자 메서드이다
- 클래스 바디에는 메서드 선언만 가능하고 다른 언어와 같이 필드(멤버) 선언은 불가능하다(추후 지원예정)
- 클래스 내부에서 선언한 속성은 클래스 인스턴스를 가리키는 this에 바인딩한다

#### `getter, setter`

> 보통 클래스 필드에 접근할때 직접 변수에 접근하는 형태가 아닌 getter, setter 메서드를 통한 접근방법을 사용한다. 클래스 입장에서는 외부에 값을 전달하거나 외부로부터 가지고 올때 추가적인 조작이 가능하기 때문이고 내부 구조를 캡슐화 하는데에도 유리하기 때문이다

자바스크립트 클래스에서는 get, set 메서드를 정의할 수 있고 해당 메서드는 함수호출이 아닌 클래스의 필드개념으로 사용하게 된다.

```javascript
class Product {
  constructor(name, price)  {
    this.name = name;
    this.price = price;
  }

  get printName() {
    return "제품명:"+this.name;
  }

  set downPrice(price) {
    this.price = price;
  } 
}

let p = new Product("애플아이폰",1000000);
console.log(p.printName);

p.downProce = 500000;
console.log(p.price);
```

## 이벤트와 DOM

### 1. 이벤트

일반적으로 프로그래밍 언어에서 이벤트라고 하면 사용자의 동작 혹은 프로그램에서 발생하는 특정한 상황을 의미한다. 이벤트가 발생하면 보통 사전에 정의된 특정 코드가 실행되고 그에 따라 기능이 동작하거나 화면이 변경되는 등의 변화가 발생한다.

- 웹 브라우저가 알려주는 HTML요소에 대한 사건의 발생을 의미
- 자바스크립트는 이벤트에 반응하여 특정 동작을 수행할 수 있다
- 입력양식으로부터 사용자의 입력값을 가져올 수 있다
- HTML 이벤트 속성은 자바스크립트 구문을 직접 실행하거나 함수를 호출할 수 있다

#### 1) HTML 이벤트

HTML에서 발생하는 이벤트는 주로 마우스 및 입력 양식에서 발생하며 문서의 로딩과 관련한 특별한 이벤트를 포함한다.

|이벤트명|설명|
|:---:|:---:|
|click|클릭시 발생|
|change|변동이 있을시 발생|
|focus|포커스를 얻었을때 발생|
|keydown|키를 눌렀을때 발생|
|keyup|키에서 손을 뗐을때 발생|
|load|문서의 로드가 완료되었을때 발생|
|unload|문서가 언로드되었을때 발생|
|resize|윈도우 크기가 변경될 경우 발생|
|mouseover|마우스가 특정 객체 위로 올려졌을 시에 발생|
|mousedown|마우스를 클릭했을때 발생|
|mouseout|마우스가 특정 객체 밖으로 나갔을 때 발생|
|mousemove|마우스가 움직였을 때 발생|
|mouseup|마우스에서 손을 떼었을때 발생|
|select|option 태그 등에서 선택을 했을 때 발생|
|submit|입력양식이 제출 요청될 때 발생|

#### 2) 이벤트 핸들러

이벤트 핸들러는 이벤트 발생을 감지하고 처리할 코드를 수행하는 역할을 담당한다. HTML태그의 속성으로 지정하거나 자바스크립트에서 DOM 엘리먼트의 속성에 콜백 함수를 정의하는 형식으로 사용할 수 있다. 이벤트 핸들러는 이벤트 이름의 앞에 on을 붙여 사용한다.

#### `HTML 속성으로 정의`

> 이벤트를 감지하기 위한 HTML 태그의 속성에 이벤트 핸들러를 기술하는 방식이다. 보통 입력양식에 작성하게 된다.

```HTML
<input type="button" value="Button" onclick="alert('버튼 클릭됨!')"></input>
```

- onclick 속성에 자바스크립트 코드를 직접 작성하거나 이벤트 처리를 위해 구현한 함수를 호출한다
- onXXX를 이용해 여러 이벤트 핸들러 속성을 부여하는 것도 가능하다

#### `자바스크립트에서 정의`

> 자바스크립트 코드를 사용해 DOM 엘리먼트 즉, 특정 HTML 태그를 선택해 속성으로 이벤트 발생시 호출될 콜백 메서드를 정의하는 형식

```HTML
<script>
    document.getElementById("b1").onclick = function() {alert("버튼 클릭됨!")}
</script>

<body>
    <input type="button" id="b1">
</body>
```

- document.getElementById() 메서드는 HTML문서에서 지정한 id 속성을 찾아 바인딩한다
- 이벤트 발생시 할당된 함수부가 호출되어 콜백 함수라고 한다

### 2. DOM(Document Object Model)

#### 1) DOM 개요

문서객체모델이라고 하며 HTML 혹은 XML 문서의 구조화된 표현을 제공하는 표준이다. HTML에서는 자바스크립트가 DOM 구조에 접근할 수 있는 방법이 제공되어 이를 통해 문서 구조, 스타일, 내용 등을 변경할 수 있다.  
자바스크립트는 DOM을 이용하여 HTML의 태그, 속성, 스타일 등을 추가/삭제 하거나 변경할 수 있다.
![](https://dinfree.com/assets/img/js_3-1.png)

#### `Document Node`

> document 객체를 말하며 DOM 트리에 접근하기 위한 최상위 노드로 모든 DOM 트리에 접근하기 위한 시작점이 된다.

#### `Element Node`

> HTML 구성 요소 즉 대표적으로 태그를 의미한다. 문서 내 태그들은 모든 Element Node들이다. 각각의 Element Node는 다시 Attribute와 Text Node를 가진다.

#### `Attribute Node`

> 태그들의 속성을 객체화한 노드를 말한다. 만일 특정 태그의 속성에 접근하려면 Attribute Node를 사용해야 한다

#### `Text Node`

> 태그의 텍스트를 객체화한 노드이다. 자식 노드를 가질 수 없으며 DOM 트리구조의 최종단 노드가 된다.

#### 2) HTML 요소 핸들링

자바스크립트에서 HTML DOM 요소에 접근하는 방법은 태그이름, 아이디, 클래스, 이름 등을 이용해 특정 노드 객체를 선택하는 것이다. CSS의 셀렉터와 유사하다고 볼 수 있다.

#### `HTML 요소선택`

> getElement(s)ByXXX()를 이용해 특정 요소(태그) 노드를 가지고 온다

```javascript
document.getElementsByTagName("tag_name")
document.getElementById("id_name")
document.getElementsByClassName("class_name")
document.getElementsByName("name_attribute")
```

- 모든 `getElementsByXXX()`함수의 경우 동일 조건의 모든 노드를 목록으로 가져옴
- `getElementByName()`의 경우 태그의 name 속성으로 노드를 찾음

#### `쿼리 셀렉터`

> getElementByXXX()와 달리 단일 메서드로 원하는 요소를 찾을 수 있다. 단 유효한 CSS 셀렉터를 사용해야 하며 그렇지 않으면 예외가 발생한다.

```javascript
document.querySelector("#main, #title, #footer");
document.querySelectorAll("p.note, p.tip")
```

- 콤마로 구분된 여러 셀렉터를 나열할 수 있으며 해당 조건에 맞는 첫번째 노드만 가져옴
- 해당 조건의 모든 노드를 가지고 오려면 querySelectorAll()을 사용

#### `HTML 요소 생성, 제거, 추가`

> HTML 요소를 선택하는 것 이외에도 특정 요소를 생성, 제거하거나 자식 노드를 추가하는 것도 가능하다

```javascript
document.createElement(element)        
document.removeChild(element)          
document.appendChild(element)          
document.replaceChild(element) 
```

#### 3) HTML 요소의 속성 핸들링

HTML 요소를 선택한 다음에는 해당 요소의 속성과 텍스트 노드에 접근할 수 있다. 물론 해당 요소내에 다른 요소들을 포함하고 있다면 마찬가지로 `getElement(s)XXX` 함수를 사용해 자식노드들을 가지고 올 수 있다.

#### `HTML 요소의 속성값 변경`

> 해당 태그의 속성값을 변경할 수 있다. 이 경우 원래 HTML 소스가 바뀌는 것이 아니라 동적으로 내용만 변경되고 브라우저에 반영된다.

```javascript
element.setAttribute(attribute, value)
element.getAttribute(attribute, value)
element.removeAttribute(attribute)

const el = document.getElementById("title-img");
el.setAttribute('src','b.jpg');
```

#### `HTML 이벤트핸들러 추가`

> 해당 요소의 이벤트를 처리하는 방법으로 이벤트에서 살펴본 것처럼 이벤트 속성에 콜백함수를 할당하는 형식으로 이벤트 핸들러를 추가할 수 있다.

```javascript
document.getElementById("id_name").onclick = function() {};
```

#### `addEventHandler()를 이용한 이벤트 핸들러 추가`

> 이벤트 요소를 추가하기 위한 가장 구조적인 방법으로 onclick 속성에 콜백 함수를 구현하는 것보다 좋은 방법이다.

```javascript
document.getElementById("id_name").addEventHandler("click", function() {});
```

#### `텍스트 노드 변경`

```javascript
element.innerHTML = '<tag>text</tag>';
element.innerText = 'text';
```

- innerHTML은 태그 바디에 따른 HTML태그와 자식노드를 포함한 텍스트를 처리할 때 사용
- innerText는 단순한 텍스트만을 처리할 때 사용
- innerText는 CSS에 종속적으로 CSS에 의해 hidden 처리가 되어있다면 텍스트노드를 읽을 수 없다.
- 따라서 가능하면 innerHTML사용 권장

### 3. 디자인 요소 변경

자바스크립트에서의 이벤트 처리는 대부분 화면을 동적으로 변경하는 것과 관련이 있다.

#### 1) 디자인 요소 변경

#### `style 속성변경`

> 모든 HTML 태그는 style 속성을 가질 수 있으므로 다음과 같이 setAttribute르 style 속성을 변경할 수 있다

```javascript
document.getElementById('box1').setAttribute('style', 'background-color:yellow');
```

- 이 방법은 인라인 스타일시트와 같은 방법이며 구조적이지 않아 권장되지 않는다

#### `style 객체 변경`

> style 속성 대신 요소의 style 객체의 속성값들을 변경하는 형식

```javascript
document.getElementById('box').style.backgroundColor = 'yellow';
```

- 이 방법은 좀 더 구조적인 방법이며 프로그램 친화적인 형태이다
- CSS 디자인 속성 이름은 `-`로 연결되지만 이 경우 Camel Case로 연결해야 한다
- style 속성 변경보다는 좋은 방법이지만 많은 디자인 속성을 변경할때는 바람직하지 않다

#### `클래스 지정`

> 별도의 디자인 클래스를 지정해 놓고 해당 요소의 class 속성을 지정하는 형태

```javascript
document.getElementById('box1').className = 'bgyellow';
```

- 이 경우 별도의 디자인 클래스가 정의되어 있어야 한다
- 가장 권장되는 방법

#### 2) 화면 숨기기

화면 구성요소를 상황에 따라 숨기거나 보여준느 방법은 UI 측면에서 매우 유용하다

#### `display:none`

> 요소를 숨기며 요소가 차지하는 공간도 함께 사라진다. 다시 보이게 하려면 속성을 block으로 변경하면 된다

```javascript
document.getElementById('box1').style.display = 'none';
```

#### `visibility:hidden`

> 요소를 숨기며 차지하는 공간도 유지하는 속성이다. width, height가 지정되어 있으면 내용은 보이지 않아도 공간은 존재하게 된다. 다시 보이게 하려면 visible이라고 하면 된다

```javascript
document.getElementById('box1').style.visibility = 'hidden';
```