---
layout: post
title: Static Factory Method Pattern
---

## Static Factory Method Pattern

정적 팩토리 메서드 패턴은 개발자가 구성한 Static Method를 통해 간접적으로 생성자를 호출하는 객체를 생성하는 디자인 패턴이다. 우리는 지금까지 객체를 인스턴스화 할때 직접적으로 생성자를 호출하여 생성하였는데, 별도의 객체 생성의 역할을 하는 클래스 메서드를 통해 간접저긍로 객체 생성을 유도한느 것이다. 그리고 이 정적 메서드를 통칭적으로 정적 팩토리 메서드 패턴이라고 부르는 것이다.

```java
class Book {
    private String title;

    // 생성자를  private화 하여 외부에서 생성자 호출 차단
    private Book(String title) { this.title = title; }

    // 정적 팩토리 메서드
    public static Book titleOf(String title) {
        return new Book(title); // 메서드에서 생성자를 호출하고 리턴
    }
}
```

```java
public static void main(String[] args) {
    // 정적 메서드 호출을 통해 인스턴스화된 객체를 얻음
    Book book1 = Book.titleOf("어린왕자");
}
```

정적 팩토리 메서드는 이름 취지에서도 알 수 있듯이 팩토리 메서드, 추상 팩토리 패턴의 팩토리 개념을 따와 심플하게 변형시킨 팩토리 변형 패턴 종류라고 보면 된다.

그렇다면 왜 멀쩡한 생성자를 냅두고 번거롭게 한단계 거쳐 정적 팩토리 메서드를 통해 객체를 생성하는지에 대한 실용성에 대해 의문을 가질것이다. 그런데 실제로 정적 팩토리 메서드는 단순히 생성자의 역할을 대신 이행하는 것 뿐만 아니라 개발자가 좀 더 가독성 좋은 코드를 작성하고 객체 지향적으로 프로그래밍 할 수 있게 도와주기 때문에 실무에서도 심심치 않게 사용되고 있다. 이것은 생성자의 본질적인 문제점을 극복하기 위해서이기도 한데, 지금부터 왜 정적 팩토리 메서들르 이용해야 되는지에 대한 이유를 알아보자.

## 생성자 대신 정적 팩토리 메서드를 고려하라

### 정적 팩토리 메서드 특징

#### 1. 생성 목적에 대한 이름 표현이 가능하다

지금까지 클래스를 설계할때 다양한 타입의 객체를 생성하기 위해, 생성 목적에 따라 생성자를 오버로딩하여 구분하여 사용해왔다. 하지만 문제는 이러한 객체를 new 키워드를 통해 생성자로 생성하려면, 개발자는 해당 생성자의 인자 순서와 내부 구조를 알고 있어야 목적에 맞게 객체를 생성할수가 있다는 번거로움이 있다.

예를들어, 다음과 같이 Car 클래스는 브랜드명과 자동차 색깔을 정의하는 멤버를 가지고 있다고 하자. 브랜드명은 반드시 생성자를 통해 외부로부터 입력을 받아야 되지만, 자동차 색깔은 기본값이 '검정'이며 선택적으로 입력받을 수 있다고 한다. 즉, 객체 생성에 있어 필수 속성과 선택 속성이 나뉘게 되는데, 이를 생성자를 통해 구현하면 두가지 형식의 생성자 오버로딩으로 처리하고, 이를 호출하는 쪽에서 생성자의 인자 갯수를 다르게 할당함으로써 구현해야 된다.

```java
class Car {
    private String brand;
    private String color = "black";

    public Car(String brand, String color) {
        this.brand = brand;
        this.color = color;
    }

    public Car(String brand) {
        this.brand = brand;
    }
}
```

```java
public static void main(String[] args) {
    // 검정색 현대 자동차
    Car hyundaiCar = new Car("Hyundai");

    // 빨간색 기아 자동차
    Car kiaCar = new Car("Kia", "Red");
}
```

지금까지는 아는게 이것밖에 없어 이런식으로 사용해왔겠지만 위의 방식은 문제점이 있다.

프로그래밍 할때 중요한 요소 중에 하나가 코드를 읽기 쉽도록 작성해야 한다는 점이다. 그런 의미에서 new 생성자 방법은 단지 매개변수의 유형과 개수를 제안할 뿐이지 어떠한 역할 표현이나 편의성을 제공하지 않는다. 즉, 생성자로 넘기는 매개변수 만으로는 반환될 객체의 특성을 제대로 표현하기가 어렵다는 것이다.

만일 나 자신이 클래스를 설계하고 오로지 나만 클래스를 이용한다고 가정한다면 이는 큰 문제가 되지 않는다. 하지만 패키지로 배포하여 남이 사용한다거나 협업을 해야된다면 말은 달라진다. 외부 사용자 입장에선 `new Car()`에 몇개를, 몇번째 인자에 어떤 타입을 할당해야 자신이 원하는 색깔의 브랜드 자동차를 생성할 수 있는지에 대한 정보를 얻기 위해선 결국은 Car 클래스 내부 구조를 뜯어봐야 한다. 만일 클래스가 바이트로 컴파일된 상태라면 이를 디컴파일 해야하고 번거롭다.

이러한 현상은 생성자 이름이 반드시 클래스 이름으로 고정되어 있기 때문에 일어나는 현상이다. 그냥 언어 설계상 어쩔수 없는 것이다.

따라서 정적 메서드를 통해 적절한 메서드 네이밍을 해준다면 반환될 객체의 특성을 한번에 유추할 수 있게 된다. 왜냐하면 네이밍을 통해 어떤 값을 이용해 자동차 객체를 만들려고 하는지 쉽게 설계 의도를 전달할 수 있기 때문이다.

```java
class Car {
    private String brand;
    private String color;

    // private 생성자
    private Car(String brand, String color) {
        this.brand = brand;
        this.color = color;
    }

    // 정적 팩토리 메서드 (매개변수 하나는 from 네이밍)
    public static Car brandBlackFrom(String brand) {
        return new Car(brand, "black");
    }

    // 정적 팩토리 메서드 (매개변수 여러개는 of 네이밍)
    public static Car brandColorOf(String brand, String color) {
        return new Car(brand, color);
    }
}
```

이처럼 생성자 대신 정적 팩토리 메서드를 호출함으로써 생성될 객체의 특성에 대해 쉽게 묘사할 수 있다는 장점이 있어 코드의 가독성을 높여주게 된다. 생성자로 만드는 것보다 의미를 가진 메소드를 이용하면 객체 생성의 의미를 훨씬 파악하기 쉽기 때문이다.

! 정적 팩토리 메서드를 구성하고자 한다면, 반드시 생성자에 private 접근 제어자를 두어 외부에서 new 키워드를 이용하여 객체를 생성하는 것을 막는 것을 잊지 말자!

#### 2. 인스턴스에 대해 통제 및 관리가 가능하다

메서드를 통해 한단계 거쳐 간접적으로 객체를 생성하기 때문에, 기본적으로 전반적인 객체 생성 및 통제 관리를 할 수 있게 된다. 즉, 필요에 따라 항상 새로운 객체를 생성해서 반환할 수도 있고, 아니면 객체 하나만 만들어두고 이를 공유하여 재사용하게 하여 불필요한 객체를 생성하는 것을 방지할 수 있는 것이다.

대표적인 예로 Singleton 디자인 패턴을 들 수 있는데, `getInstance()`라는 정적 팩토리 메서드를 사용해 오로지 하나의 객체만 반환하도록 하여 객체를 재사용해 메모리를 아끼도록 유도할 수 있다.

```java
class Singleton {
    private static Singleton instance;

    private Singleton() {}

    // 정적 팩토리 메서드
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

또다른 예로는 인스턴스에 대한 캐싱 절차 구조를 정적 팩토리 메서드로 구현할 수 있다. 인스턴스에 대해 캐싱을 한다면 필요한 인스턴스만 뽑아 재사용하여 메모리를 절약할 수 있게 된다.

아래 코드를 살펴보면 요일 정보를 나타내는 Day 객체와, 이 Day 객체를 생성하는 DayFactory 팩토리 객체가 있다. 그중 팩토리 객체를 보면 내부적으로 HashMap 자료구조를 이용하여 key 값을 통해 인스턴스를 캐싱하여 관리하고 있는데, 정적 팩토리 메서드가 호출되면 먼저 캐시 저장소를 조회하여 기존 Day 객체가 있는지 검사하고, 있으면 그대로 반환하여 재사용을 유도하고, 없다면 새로 생성해서 캐싱해두게 된다.

이렇게 인스턴스를 통제하는 것은 인스턴스가 단 하나뿐임을 보장하는 것이고, Flyweight 디자인 패턴의 근간이 되게 된다.

```java
class Day {
    private String day;

    public Day(String day) { this.day = day; }

    public String getDay() { return day; }
}

// Day 객체를 생성하고 관리하는 Flyweight 팩토리 클래스
class DayFactory {

    // Day 객체를 저장하는 캐싱 저장소 역할
    private static final Map<String, Day> cache = new HashMap<>();

    // 자주 사용될것 같은 Day 객체 몇가지를 미리 등록
    static {
        cache.put("Monday", new Day("Monday"));
        cache.put("Tuesday", new Day("Tuesday"));
        cache.put("Wednesday", new Day("Wednesday"));
    }

    // 정적 팩토리 메서드 (인스턴스에 대해 철저한 관리)
    public static Day from(String day) {

        if(cache.containsKey(day)) {
            // 캐시 되어있으면 그대로 가져와 반환
            return cache.get(day);
        } else {
            // 캐시 되어 있지 않으면 새로 생성하고 캐싱하고 반환
            Day d = new Day(day);
            cache.put(day, d);
            return d;
        }
    }
}
```

이렇게 인스턴스의 생성에 관여하여, 생성되는 인스턴스의 수를 통제할 수 있는 클래스를 인스턴스 통제(instance-controlled) 클래스라고 한다.

#### 3. 하위 자료형 객체를 반환할 수 있다

클래스의 다형성의 특징을 응용한 정적 팩토리 메서드 특징이다. 메서드 호출을 통해 얻을 객체의 인스턴스를 자유롭게 선택할수 있는 유연성을 갖는 것이다.

```java
interface SmartPhone {}

class Galaxy implements SmartPhone {}
class Iphone implements SmartPhone {}
class Huawei implements SmartPhone {}

class SmartPhones {
    public static SmartPhone getSamsungPhone() {
        return new Galaxy();
    }
    public static SmartPhone getApplePhone() {
        return new Iphone();
    }
    public static SmartPhone getChinesePhone() {
        return new Huawei();
    }
}
```

이러한 아이디어는 인터페이스를 정적 팩토리 메서드의 반환 타입으로 사용하는 인터페이스 기반 프레임워크를 만드는 핵심이 된다. 대표적으로 자바의 컬렉션 프레임워크인 java.util.Collections 클래스를 들 수 있는데, 이 클래스는 Collection 인터페이스를 반환하는 여러 정적 팩토리 메서드를 가지고 있다.

Collections 클래스와 Collection 인터페이스의 이름을 보면 `'s'`를 붙인걸 볼 수 있는데, 이것을 Collection 인터페이스의 __동반 클래스(Companion Class)__라고 부른다. 위의 SmartPhones 예제 코드에도 컬렉션을 그대로 응용한 것이다.

하지만 java8 버전부터는 인터페이스가 정적 메서드를 가질수 있게 되어 동반 클래스 개념은 더이상 필요없어졌다. 즉, 인터페이스에 그냥 정적 팩토리 메서드를 선언하면 된다.

```java
interface SmartPhone {
    public static SmartPhone getSamsungPhone() {
        return new Galaxy();
    }
    public static SmartPhone getApplePhone() {
        return new Iphone();
    }
    public static SmartPhone getChinesePhone() {
        return new Huawei();
    }
}
```

#### 4. 인자에 따라 다른 객체를 반환하도록 분기할 수 있다

메서드이니 매개변수를 받을 수 있을테고, 메서드 블록 내에서 분기문을 통해 여러 자식 타입의 인스턴스를 반환하도록 응용 구성이 가능하다. 위의 3번에 대한 확장 예제라고 보면 된다.

```java
interface SmarPhone {
    public static SmarPhone getPhone(int price) {
        if(price > 100000) {
            return new IPhone();
        }

        if(price > 50000) {
            return new Galaxy();
        }

        return new Huawei();
    }
}
```

#### 5. 객체 생성을 캡슐화 할 수 있다

생성자를 사용하는 경우 외부에 내부 구현을 드러내야 하는데, 정적 팩토리 메서드는 구현부를 외부로부터 숨길 수 있어 캡슐화 및 정보 은닉을 할 수 있다는 특징이 있다. 또한 노출하지 않는다는 특징은 정보 은닉성을 가지기도 하지만 동시에 사용하고 있는 구현체를 숨겨 의존성을 제거해주는 장점도 지니고 있다. 이 예제 역시 위의 3번의 확장 예이다.

아래 예제 코드를 보면, 메인 메서드에서 오로지 GradeCalculator의 정적 팩토리 메서드 `of()`를 호출하여 Grade 인터페이스 타입의 객체를 반환할 뿐이지, Grade 인터페이스의 구현체인 A ~ F 객체 존재에 대해서는 모르게 된다. 즉, 구현체를 생성해서 반환할 책임은 정적 팩토리 메서드를 가진 GradeCalculator이고, 클라이언트는 구현체를 신경쓸 필요없이 제공되는 메서드를 호출만 하면 되어 편리하게 사용이 가능해진다.

```java
interface Grade {
    String toText();
}

class A implements Grade {
    @Override
    public String toText() {return "A";}
}

class B implements Grade {
    @Override
    public String toText() {return "B";}
}

class C implements Grade {
    @Override
    public String toText() {return "C";}
}

class D implements Grade {
    @Override
    public String toText() {return "D";}
}

class F implements Grade {
    @Override
    public String toText() {return "F";}
}

class GradeCalculator {
	// 정적 팩토리 메서드
    public static Grade of(int score) {
        if (score >= 90) {
            return new A();
        } else if (score >= 80) {
            return new B();
        } else if (score >= 70) {
            return new C();
        } else if (score >= 60) {
            return new D();
        } else {
            return new F();
        }
    }
}
```

## 정적 팩토리 메서드 네이밍 규칙

정적 팩토리 메서드와 다른 정적 메서드와 역할을 구분짓기 위해 독자적인 네이밍 컨벤션(Convention)이 존재한다. 단, 정적 팩토리 메서드에서의 네이밍은 단순히 선호도 의미를 넘어서 거의 법칙 정도로 사용되는 것이니, 각 네이밍의 역할에 대해 알아두는 것은 개념을 아는 것만큼 중요하다.

정적 팩토리 메서드에서 사용되는 네이밍 단어 종류는 다음과 같다.
- from : 하나의 매개 변수를 받아서 객체를 생성
- of : 여러개의 매개 변수를 받아서 객체를 생성
- getInstance : 인스턴스를 생성. 이전에 반환했던 것과 같을 수 있음
- newInstance : 항상 새로운 인스턴스를 생성

## 실무에서 찾아보는 정적 팩토리 메서드

사실 우리는 자바에서 제공하는 여러 패키지의 클래스들을 가져와 사용하면서 우리들은 알기도 모르게 정적 팩토리 메서드를 사용해 왔다. 그저 지칭하는 단어만 모르고 있을 뿐 그에 대한 톡톡한 효과를 누리고 있었던 것이다.

### Optional의 of()

자바 8버전부터 지원하는 Optional 객체를 얻기 위해선 아래와 같이 new 키워드 대신 `of()` 메서드를 이용해 객체를 얻도록 설계되어 있다. 위에서 배운 정적 팩토리 메서드 네이밍 컨벤션을 잘 따른 예이기도 하다.

### List의 of()

이 부분은 하위 자식 객체 반환 및 캡슐화에 대한 실전 구현체 예이다.

자바 8버전부터 지원하는 List 인터페이스의 `of()`메서드는 인터페이스를 반환하는 정적 팩토리 메서드이다. 사용자 입장에선 반환되는 클래스가 어떤 타입인지 알 필요 없이, 그냥 `of()` 메서드의 기능이 무엇인지만 알고 호출만 하면 되는 것이다.

### Integer의 valueOf()

이부분은 정적 팩토리 메서드의 '인스턴스를 캐싱하여 관리가 가능하다'는 실제 자바 구현체 예이다.

실제로 primitive 타입의 Wrapper 클래스들은 값을 캐싱하여 자바 프로그램을 최적화하도록 설계되어있다. 예를 들어 Integer 클래스의 `valueOf()` 메서드는 입력값이 -128 ~ 127 범위의 정수라면 캐싱을 이용하도록 설계되어있다.

## Lombok으로 정적 팩토리 메서드 만들기

```java
@RequiredArgsConstructor(staticName = "of")
class Product {
    private Long id;
    private String name;
}

public class Main {
    public static void main(String[] args) {
        Product p = Product.of();
    }
}
```

## 정적 팩토리 메서드 문제점

생성자보단 정적 팩토리 메서드를 고려해야 하지만, 정적 팩토리 메서드도 단점이 존재하니 이를 잘 파악하고 사용하여야 한다.

### 1. private 생성자일 경우 상속 불가능

정적 팩토리 메서드로 클래스를 설계를 하면 생성자를 private 접근 제어자로 설정하게 된다. 따라서 정적 팩토리 메서드를 적용하는 경우에는 상속을 이용한 확장이 불가능해진다.

다만, 이 부분은 단점이라기 보단 스펙에 가깝다고 얘기할 수 있다.

대표적인 예로 자바의 Collections 클래스를 보면 생성자가 private로 되어 있다. 이는 일부로 상속을 하게 하지 않기 위한 설계된건데, Collections는 자바의 컬렉션에 대한 헬퍼 제공 용도이지 상속을 통해 무언가를 확장하게 하기 위한 용도가 아니지 때문이다. 따라서 이러한 제약은 상속(Inheritance)보단 합성(Composition)을 사용하도록 유도하게 하거나, 불변(Immutable)객체로 만들고 싶을때 사용되는 하나의 코드 패턴이라고 보면 된다.

### API 문서에서의 불편함

생성자는 하나의 자바 프로그래밍 언어의 스펙이기 때문에 JavaDoc 같은 문서에서 상단에 정의되어 있기 때문에 빠르게 그에 대한 스펙 검색을 할 수가 있다. 반면에 정적 팩토리 메서드는 개발자가 임의로 만든 메서드이기 때문에, 안그래도 그 많은 메서드들 중에서 정적 팩토리 역할을 하는 메서드를 뒤져서 찾아 이해해야 한다.

따라서 클래스 설계자는 API 문서를 깔끔하게 작성할 필요가 있으며, 정적 팩토리 메서드를 작성할때 네이밍 컨벤션을 지킴으로써 문제점을 극복하기도 한다.