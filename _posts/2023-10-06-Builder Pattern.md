---
layout: post
title: Builder Pattern
---

# Builder Pattern

빌더 패턴(Builder Pattern)은 복잡한 객체의 생성 과정과 표현 방법을 분리하여 다양한 구성의 인스턴스를 만드는 생성 패턴이다. 생성자에 들어갈 매개 변수를 메서드로 하나하나 받아들이고 마지막에 통합 빌드해서 객체를 생성하는 방식이다.

이해하기 쉬운 사례로 수제 햄버거를 들 수 있다. 수제 햄버거를 주문할때 빵이나 패티 등 속재료들은 주문하는 사람이 마음대로 결정된다. 어느 사람은 치즈를 빼달라고 할 수 있고 어느 사람은 토마토를 빼달라고 할 수 있다. 이처럼 선택적 속재료들을 보다 유연하게 받아 다양한 타입의 인스턴스를 생성할 수 있어, 클래스의 선택적 매개변수가 많은 상황에서 유용하게 사용된다.

## 탄생 배경

### 점층적 생성자 패턴

점층적 생성자 패턴(Telescoping Constructor Pattern)은 필수 매개변수와 함께 선택 매개변수를 0개, 1개, 2개, ... 받는 형태로, 우리가 매개변수를 입력받아 인스턴스를 생성하고 싶을떄 사용하던 생성자를 오버로딩 하는 방식이다.

```java
class Hamburger {
    // 필수 매개변수
    private int bun;
    private int patty;

    // 선택 매개변수
    private int cheese;
    private int lettuce;
    private int tomato;
    private int bacon;

    public Hamburger(int bun, int patty, int cheese, int lettuce, int tomato, int bacon) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
        this.lettuce = lettuce;
        this.tomato = tomato;
        this.bacon = bacon;
    }

    public Hamburger(int bun, int patty, int cheese, int lettuce, int tomato) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
        this.lettuce = lettuce;
        this.tomato = tomato;
    }
    

    public Hamburger(int bun, int patty, int cheese, int lettuce) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
        this.lettuce = lettuce;
    }

    public Hamburger(int bun, int patty, int cheese) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
    }

    ...
}
```

하지만 이러한 방식은 클래스 인스턴스 필드들이 많으면 많을수록 생성자에 들어갈 인자의 수가 늘어나 몇번째 인자가 어떤 필드였는지 헷갈릴 경우가 생기게 된다. 만일 여러 종류의 햄버거를 생성하기 위해 Hamburger 생성자의 몇번째 인수가 양상추의 갯수인지 토마토의 갯수인지 파악할 필요가 있다.

또한 매개변수 특성상 순서를 따라야 하기 때문에 위의 '빵과 베이컨만 있는 햄버거'를 원할경우 억지로 파라미터에 0을 전달해야 된다. 생성자로만으로는 필드를 선택적으로 생략할 수 있는 방법이 없기 때문이다.

그리고 무엇보다 타입이 다양할수록 생성자 메서드 수가 기하급수적으로 늘어나 가독성이나 유지보수 측면에서 좋지 않다.

### 자바 빈 패턴

이러한 단점을 보완하기 위해 Setter 메서드를 사용한 자바 빈 패턴이 고안되었다. 매개변수가 없는 생성자로 객체 생성 후 Setter 메서드를 이용해 클래스 필드의 초깃값을 설정하는 방식이다.

```java
class Hamburger {
    // 필수 매개변수
    private int bun;
    private int patty;

    // 선택 매개변수
    private int cheese;
    private int lettuce;
    private int tomato;
    private int bacon;
    
    public Hamburger() {}

    public void setBun(int bun) {
        this.bun = bun;
    }

    public void setPatty(int patty) {
        this.patty = patty;
    }

    public void setCheese(int cheese) {
        this.cheese = cheese;
    }

    public void setLettuce(int lettuce) {
        this.lettuce = lettuce;
    }

    public void setTomato(int tomato) {
        this.tomato = tomato;
    }

    public void setBacon(int bacon) {
        this.bacon = bacon;
    }
}
```

기존 생성자 오버로딩에서 나타났던 가독성 문제점이 사라지고 선택적인 파라미터에 대해 해당되는 Setter 메서드를 호출함으로써 유연적으로 객체 생성이 가능해졌다. 하지만 이러한 방식은 객체 생성 시점에 모든 값들을 주입하지 않아 일관성(consistency) 문제와 불변성(immutable)문제가 나타나게 된다.

1. 일관성 문제
    필수 매개변수란 객체가 초기화될때 반드시 설정되어야 하는 값이다. 하지만 개발자가 깜빡하고 `setBun()`이나 `setPatty()` 메서드를 호출하지 않았다면 이 객체는 일관성이 무너진 상태가 된다. 즉, 객체가 유효하지 않은 것이다. 만일 다른곳에서 햄버거 인스턴스를 사용하게 된다면 런타임 예외가 발생할 수도 있다.

    이는 객체를 생성하는 부분과 값을 설정하는 부분이 물리적으로 떨어져 있어서 발생하는 문제점이다. 물론 이는 어느정도 생성자와 결합하여 극복은 할 수 있다. 하지만 다음에 소개할 불변성의 문제 때문에 자바 빈즈 패턴은 지양해야 한다.

2. 불변성 문제
    자바 빈즈 패턴의 Setter 메서드는 객체를 처음 생성할때 필드값을 설정하기 위해 존재하는 메서드이다. 하지만 객체를 생성했음에도 여전히 외부적으로 Seter 메서드를 노출하고 있으므로, 협업 과정에서 언제 어디서 누군가 Setter 메서드를 호출해 함부로 객체를 조작할 수 있게 된다. 이것을 불변함을 보장할 수 없다고 얘기한다.

    마치 완성된 햄버거에 중간에 치즈를 교체한다고 햄버거를 막 분리하는 것과 같은 이치다.

### 빌더 패턴

빌더 패턴은 이러한 문제들을 해결하기 위해 별도의 Builder 클래스를 만들어 메소드를 통해 step-by-step으로 값을 입력받은 후에 최종적으로 `build()` 메소드로 하나의 인스턴스를 생성하여 리턴하는 패턴이다.

빌더 패턴 사용법을 잠시 살펴보면, StudentBuilder 빌더 클래스의 메서드를 체이닝(Chaining) 형태로 호출함으로써 자연스럽게 인스턴스를 구성하고 마지막에 `build()` 메서드를 통해 최종적으로 객체를 생성하도록 되어있음을 볼 수 있다.

```java
public static void main(String[] args) {

    // 생성자 방식
    Hamburger hamburger = new Hamburger(2, 3, 0, 3, 0, 0);

    // 빌더 방식
    Hamburger hamburger = new Hamburger.Builder(10)
        .bun(2)
        .patty(3)
        .lettuce(3)
        .build();
}
```

빌더 패턴을 이용하면 더이상 생성자 오버로딩 열거를 하지 않아도 되며, 데이터의 순서에 상관없이 객체를 만들어내 생성자 인자 순서를 파악할 필요도 없고 잘못된 값을 넣는 실수도 하지 않게 된다. 점층적 생성자 패턴과 자바빈즈 패턴 두가지의 장점만을 취하였다고 볼 수 있다.

## 빌더 패턴 구조

빌더 패턴 구현 자체는 난이도가 어렵지 않으니 빠르고 쉽게 구성이 가능하다.

예를 들어 다음과 같은 Student 클래스에 대한 객체 생성만을 담당하는 별도의 빌더 클래스를 만들려고 한다.

```java
class Student {
    private int id;
    private String name = "아무개";
    private String grade = "freshman";
    private String phoneNumber = "010-0000-0000";

    public Student(int id, String name, String grade, String phoneNumber) {
        this.id = id;
        this.name = name;
        this.grade = grade;
        this.phoneNumber = phoneNumber;
    }
    
    @Override
    public String toString() {
        return "Student { " +
                "id='" + id + '\'' +
                ", name=" + name +
                ", grade=" + grade +
                ", phoneNumber=" + phoneNumber +
                " }";
    }
}
```

### 빌더 클래스 구현

먼저 Builder 클래스를 만들고 필드 멤버 구성을 만들고자 하는 Student 클래스 멤버 구성과 똑같이 구성한다.

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;
    
}
```

그리고 각 멤버에 대한 Setter 메서드를 구현해준다. 이때 가독성을 좋게 하면서도 기존 Setter와의 다른 특성을 가지고 있는 점을 알리기 위해서, set 단어는 빼주고 심플하게 멤버이름으로만 메서드명을 지어준다.

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;

    public StudentBuilder id(int id) {
        this.id = id;
        return this;
    }

    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
    }
}
```

여기서 주목할 부분은 각 Setter 함수 마지막 반환 구문인 `return this` 부분이다. 여기서 `this`란 StudentBuilder 객체 자신을 말한다. 즉, 빌더 객체 자신을 리턴함으로써 메서드 호출 후 연속적으로 빌더 메서드들을 체이닝(Chaining)하여 호출할 수 있게 된다. ex) `new StudentBuilder().id(값).name(값)`

마지막으로 빌더의 목표였던 최종 Student 객체를 만들어주는 build 메서드를 구성해준다. 빌더 클래스의 필드들을 Student 생성자의 인자에 넣어줌으로써 멤버 구성이 완료된 Student 인스턴스를 얻게 되는 것이다.

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;

    public StudentBuilder id(int id) { ... }

    public StudentBuilder name(String name) { ... }

    public StudentBuilder grade(String grade) { ... }

    public StudentBuilder phoneNumber(String phoneNumber) { ... }

    public Student build() {
        return new Student(id, name, grade, phoneNumber); // Student 생성자 호출
    }
}
```

### 빌더 클래스 실행

```java
public static void main(String[] args) {

    Student student = new StudentBuilder()
                .id(2016120091)
                .name("임꺽정")
                .grade("Senior")
                .phoneNumber("010-5555-5555")
                .build();

    System.out.println(student);
}
```

# Builder 패턴 장단점 총정리

## 빌더 패턴 장점

### 1. 객체 생성 과정을 일관된 프로세스로 표현

생성자 방식으로 객체를 생성하는 경우는 매개변수가 많아질수록 가독성이 급격하게 떨어진다. 클래스 변수가 4개 이상만 되어도 각 인자 순서마다 이 값이 어떤 멤버에 해당되는지 바로 파악이 힘들다.

반면 다음과 같이 빌더 패턴을 적용하면 직관적으로 어떤 데이터에 어떤 값이 설정되는지 한눈에 파악할 수 있게 된다. 특히 연속된 동일 타입의 매개 변수를 많이 설정할 경우에 발생할 수 있는 설정 오류와 같은 실수를 방지할 수 있다.

다만 빌더 패턴이 탄생하게 된 옛 시대와 달리, 요즘에는 인텔리제이나 이클립스 같은 왠만한 IDE에선 아래와 같이 생성자 매개변수에 대한 미리보기 힌트 기능을 제공해주기 때문에 이 부분은 요즘 트렌드에는 맞지 않을 수도 있다.

### 2. 디폴트 매개변수 생략을 간접적으로 지원

본래 디폴트 매개변수라는 건 인자 값을 설정해줘도 되고 설정 안하고 생략해도 되는 것을 말한다. 그런데 파이썬이나 자바스크립트와 달리 자바 언어에선 기본적으로 메서드에 대한 디폴트 매개 변수를 지원하지 않는다.

따라서 디폴트 매개변수를 구현하기 위해선 클래스 필드 변수에 초깃값을 미리 세팅하고, 초깃값이 세팅된 필드 인자를 제외시킨 생성자를 따로 구현하는 식으로 설계해야 한다. 하지만 이는 결국 지나친 생성자 오버로딩 열거를 통한 본래의 문제점을 회귀한 꼴이 된다.

```java
class Student {
    private int id;
    private String name;
    private String grade = "freshman"; // 디폴트 매개변수 역할
    private String phoneNumber;

    public Student(int id, String name, String grade, String phoneNumber) {
        ...
    }
	
    // 디폴트 매개변수를 제외한 인자들을 받는 생성자 오버로딩
    public Student(int id, String name, String phoneNumber) {
        ...
    }

    @Override
    public String toString() {
        return "Student { " +
                "id='" + id + '\'' +
                ", name=" + name +
                ", grade=" + grade +
                ", phoneNumber=" + phoneNumber +
                " }";
    }
}
```

빌더 패턴에서도 디폴트 매개변수를 구현하는 방법은 똑같다. 다만 빌더라는 객체 생성 전용 클래스를 경유하여 이용함으로써 디폴트 매개변수가 설정된 필드를 설정하는 메서드를 호출하지 않는 방식으로 마치 디폴트 매개변수를 생략하고 호출하는 효과를 간접적으로 구현할 수 있게 된다.

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade = "freshman"; // 디폴트 매개변수 역할
    private String phoneNumber;

    ...
}

// 디폴트 필드인 grade를 제외하고 빌더 구성 및 인스턴스화
Student student1 = new StudentBuilder(2016120091)
        .name("홍길동")
        .phoneNumber("010-5555-5555")
        .build();

System.out.println(student1);
```

### 3. 필수 멤버와 선택적 멤버를 분리 가능

객체 인스턴스는 목적에 따라 초기화가 필수인 멤버 변수가 있고 선택적인 멤버 변수가 있을 수 있다.

만일 Student 클래스의 id 필드가 인스턴스화 할때 반드시 필수적으로 값을 지정해 주어야 하는 필수 멤버 변수라고 가정해보자. 이를 기존 생성자 방식으로 구현하려면 초기화가 필수인 멤버 변수만을 위한 생성자를 정의하고 선택적인 멤버 변수는 대응하는 생성자를 오버로딩을 통해 열거하거나, 혹은 전체 멤버를 인자로 받는 생성자만을 선언하고 매개변수에 null을 받는식으로 구성하여야 한다.

```java
class Student {
    // 초기화 필수 멤버
    private int id;

    // 초기화 선택적 멤버
    private String name;
    private String grade;
    private String phoneNumber;

    public Student(int id, String name, String grade, String phoneNumber) {
        this.id = id;
        this.name = name;
        this.grade = grade;
        this.phoneNumber = phoneNumber;
    }
}

Student student = new Student(2010234455, null, null, null);
```

이는 한눈에 봐도 별로 좋지 않은 방법임을 알 수 있을 것이다.

따라서 빌더 클래스를 통해 초기화가 필수인 멤버는 빌더의 생성자로 받게 하여 필수 멤버를 설정해주어야 빌더 객체가 생성되도록 유도하고, 선택적인 멤버는 빌더의 메서드로 받도록 하면, 사용자로 하여금 필수 멤버와 선택 멤버를 구분하여 객체 생성을 유도할 수 있다.

```java
class StudentBuilder {
    // 초기화 필수 멤버
    private int id;

    // 초기화 선택적 멤버
    private String name;
    private String grade;
    private String phoneNumber;

    // 필수 멤버는 빌더의 생성자를 통해 설정
    public StudentBuilder(int id) {
        this.id = id;
    }

    // 나머지 선택 멤버는 메서드로 설정
    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
    }

    public Student build() {
        return new Student(id, name, grade, phoneNumber);
    }
}

Student student1 = 
        new StudentBuilder(2016120091) // 필수 멤버
        .name("홍길동") // 선택 멤버
        .build();

Student student2 = 
        new StudentBuilder(2016120091) // 필수 멤버
        .name("임꺽정") // 선택 멤버
        .grade("freshman") // 선택 멤버
        .build();

Student student3 = 
        new StudentBuilder(2016120091) // 필수 멤버
        .name("주몽") // 선택 멤버
        .grade("Senior") // 선택 멤버
        .phoneNumber("010-5555-5555") // 선택 멤버
        .build();
```

### 4. 객체 생성 단계를 지연할 수 있음

객체 생성을 단계별로 구성하거나 구성 단계를 지연하거나 재귀적으로 생성을 처리할 수 있다. 즉, 빌더를 재사용 함으로써 객체 생성을 주도적으로 지연할 수 있는 것이다.

```java
// 1. 빌더 클래스 전용 리스트 생성
List<StudentBuilder> builders = new ArrayList<>();

// 2. 객체를 최종 생성 하지말고 초깃값만 세팅한 빌더만 생성
builders.add(
    new StudentBuilder(2016120091)
    .name("홍길동")
);

builders.add(
    new StudentBuilder(2016120092)
    .name("임꺽정")
    .grade("senior")
);

builders.add(
    new StudentBuilder(2016120093)
    .name("박혁거세")
    .grade("sophomore")
    .phoneNumber("010-5555-5555")
);

// 3. 나중에 빌더 리스트를 순회하여 최종 객체 생성을 주도
for(StudentBuilder b : builders) {
    Student student = b.build();
    System.out.println(student);
}
```

### 5. 초기화 검증 멤버별로 분리

만일 생성자로부터 멤버값을 받는 형태라면, 각 생성자 매개변수에 대한 검증 로직을 생성자 메소드마다 복잡하게 구현하여야 한다. 이는 생성자의 크기가 비대해지게 되는 결과를 낳게 된다.
```java
class Student {
	...

	// 각 매개변수에 대한 검증을 하나의 생성자 모두 처리하고 있다
    public Student(int id, String name, String grade, String phoneNumber) {
        if (!grade.equals("freshman") && !grade.equals("sophomore") && !grade.equals("junior") && !grade.equals("senior")) {
            throw new IllegalArgumentException(grade);
        }

        if (!phoneNumber.startsWith("010")) {
            throw new IllegalArgumentException(phoneNumber);
        }

        this.id = id;
        this.name = name;
        this.grade = grade;
        this.phoneNumber = phoneNumber;
    }
}
```

빌더를 이용하면 생성될 객체의 멤버 변수의 초기화와 검증을 각각의 멤버별로 분리해서 작성할 수 있다. 빌더의 각각의 멤버 설정 메서드에서 검증 과정을 분담함으로써 유지 보수를 용이하게 하는 것이다.

```java
class StudentBuilder {
	...

    public StudentBuilder(int id) {
        this.id = id;
    }

    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        if (!grade.equals("freshman") && !grade.equals("sophomore") && !grade.equals("junior") && !grade.equals("senior")) {
            throw new IllegalArgumentException(grade);
        }
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        if (!phoneNumber.startsWith("010")) {
            throw new IllegalArgumentException(phoneNumber);
        }
        this.phoneNumber = phoneNumber;
        return this;
    }

    public Student build() {
        return new Student(id, name, grade, phoneNumber);
    }
}
```

물론 이러한 형식은 흔히 Getter & Setter 형식에서도 많이 이용되는 패턴이기도 하다. 하지만 어느 클래스에 Setter 메서드를 구현한다는 말은 객체 멤버의 변경 가능성을 열어둔 것과 같아 불변성 문제가 터지게 된다.

### 6. 멤버에 대한 변경 가능성 최소화를 추구

많은 개발자들이 자바 프로그래밍을 하면서 멤버에 값을 할당할때 흔히 사용하는 것이 Setter 메서드인데, 그 중 클래스 멤버 초기화를 Setter를 통해 구성하는 것은 매우 좋지 않은 방법이다. 즉, 이 부분은 위의 빌더 패턴 탄생 배경에서 다루었던 Java Beans Pattern인 Setter 메서드를 통해 멤버 초기화를 하지 말아야 하는 이유에 대한 좀 더 고수준적인 내용이다.

일반적으로 프로그램을 개발하는데 있어 다른 사람과 협업할 때 가장 중요시되는 점 중 하나가 바로 불변(immutable)객체이다. 불변 객체란 객체 생성 이후 내부의 상태가 변하지 않는 객체이다. 불변 객체는 오로지 읽기(get) 메소드만을 제공하며 쓰기(set)는 제공하지 않는다. 대표적으로 자바에서 final 키워드를 붙인 변수가 바로 불변이다.

현업에서 불변 객체를 이용해 개발해야 하는 이유로는 다음과 같다.
- 불변 객체는 Thread-Safe하여 동기화를 고려하지 않아도 된다
- 만일 가변 객체를 통해 작업을 하는 도중 예외(Exception)가 발생하면 해당 객체가 불안정한 상태에 빠질 수 있어 또 다른 에러를 유발할 수 있는 위험성이 있기 때문이다.
- 불변 객체로 구성하면 다른 사람이 개발한 함수를 위험없이 이용을 보장할 수 있어 협업에도 유지보수에도 유용하다.

따라서 클래스들은 가변적이여야 하는 매우 타당한 이유가 있지 않는 한 반드시 불변으로 만들어야 한다. 만약 클래스를 불변으로 만드는 것이 불가능하다면 가능한 변경 가능성을 최소화해야 한다.

예를 들어 경우에 따라 변수에 final 키워드를 붙일 수 없는 상황이 생길 수도 있다. 이때는 Setter 메서드 자체를 구현하지 않음으로써 불변 객체를 간접적으로 구성이 가능하다. 그러면 결국은 돌고 돌아 생성자를 이용하라는 것인데 역시나 지나친 생성자 오버로딩 문제가 발생하게 된다. 그래서 연구된것이 빌더 클래스이다.

즉, 최종 정리하자면 빌더 패턴은 생성자 없이 어느 객체에 대해 '변경 가능성을 최소화'를 추구하여 불변성을 갖게 해주게 되는 것이다.

## 빌더 패턴 단점

### 1. 코드 복잡성 증가

우선 빌더 패턴을 적용하려면 N개의 클래스에 대해 N개의 새로운 빌더 클래스를 만들어야 해서, 클래스 수가 기하급수적으로 늘어나 관리해야 할 클래스가 많아지고 구조가 복잡해질 수 있따. 또한 선택적 매개변수를 많이 받는 객체를 생성하기 위해서는 먼저 빌더 클래스부터 정의해야한다. 다만 이부분은 여느 디자인 패턴이 가지는 단점이기도 하다.

### 2. 생성자보다는 성능은 떨어진다

매번 메서드를 호출하여 빌더를 거쳐 인스턴스화 하기 때문에 어쩌면 당연한 말일지도 모른다. 비록 생성 비용 자체는 크지는 않지만, 어플리케이션의 성능을 극으로 중요시되는 상황이라면 문제가 될 수 있다.

### 3. 지나친 빌더 남용은 금지

클래스의 필드의 개수가 4개보다 적고, 필드의 변경 가능성이 없는 경우라면 차라리 생성자나 정적 팩토리 메서드를 이용하는 것이 더 좋을 수 있다. 빌더 패턴의 코드가 다소 장황하기 때문이다. 따라서 클래스 필드의 갯수와 필드 변경 가능성을 중점으로 보고 패턴 적용 유무를 가려야한다.

다만 API는 시간이 지날수록 많은 매개변수를 갖는 경향이 있기 때문에 애초에 빌더 패턴으로 시작하는 편이 나을 때가 많다고 말한다.

# Builder 디자인 패턴 종류

빌더 패턴에는 여타 디자인 패턴과는 다르게 두가지 디자인 종류가 존재한다. GOF의 디자인 패턴에서 소개하는 빌더 패턴과 이펙티브 자바 책에서 소개하는 빌더 패턴 구조가 서로 다르기 때문이다.

- 이펙티브 자바의 빌더 패턴 : 생성시 지정해야 할 인자가 많을때 사용. 객체의 일관성 불변성이 목적.
- GoF의 빌더 패턴 : 객체의 생성 단계 순서를 결정해두고 각 단계를 다양하게 구현하고 싶을 때 사용.

## 심플 빌더 패턴

보통 개발자들이 빌더 패턴을 말할 때 정의되는 것이 이펙티브 자바에서 소개한 빌더 패턴이다. GOF 빌더 패턴과 구분하기 위해 심플 빌더 패턴이라고도 불리운다.

심플 빌더 패턴은 생성자가 많을 경우 또는 변경 불가능한 불변 객체가 필요한 경우 코드의 가독성과 일관성, 불변성을 유지하는 것에 중점을 둔다. 심플 빌더 패턴은 위에서 우리가 배운 빌더 패턴과 거의 차이가 없다. 다만 빌더 클래스가 구현할 클래스의 정적 내부 클래스로 구현된다는 점이 다르다.

빌더 클래스가 static inner class로 구현되는 이유로는 다음과 같다.

1. 하나의 빌더 클래스는 하나의 대상 객체 생성만을 위해 사용된다. 그래서 두 클래스를 물리적으로 그룹핑함으로써 두 클래스간의 관계에 대한 파악을 쉽게 할 수 있다.

2. 대상 객체는 오로지 빌더 객체에 의해 초기화된다. 즉, 생성자를 외부에 노출시키면 안되기 때문에 생성자를 private로 하고, 내부 빌더 클래스에서 private 생성자를 호출함으로써 오로지 빌더 객체에 의해 초기화되도록 설계 할 수 있다.

3. inner class를 쓰면 좋은건 알겠는데 왜 하필 static으로 선언해주어야 하냐면, 정적 내부 클래스는 외부 클래스의 인스턴스 없이도 생성할 수 있는데, 만일 일반 내부 클래스로 구성한다면 내부 클래스를 생성하기도 전에 외부 클래스를 인스턴스화 해야 한다. 빌더가 최종적으로 생성할 클래스의 인스턴스를 먼저 생성해야 한다면 모순이 생기기 때문이다.

4. 메모리 누수 문제 때문에 static으로 내부 클래스를 정의해주어야 한다. inner 클래스는 inner static 클래스보다 메모리를 더 사용하고, 느리고, outer class가 GC 대상에서 빠져버리기 때문이다.

### 심플 빌더 패턴 구현

1. 빌더 클래스를 static nested class로 정의한다.
2. 빌더를 통해 인스턴스화 하기 때문에 대상 객체 생성자는 private로 정의한다.
3. 빌더 클래스의 생성자는 public으로 하며, 필수 파라미터에 대해 생성자의 파라미터로 받는다.
4. 선택적 파라미터에 대해서는 메소드로 제공한다. 이때 메소드의 반환값은 빌더 객체 자신(this)이어야 한다.
5. 마지막 단계로 최종 객체를 생성하는 `build()` 메소드를 정의하여 클라이언트에게 최종 생성된 결과물을 제공한다.
6. 이때 생성자의 인수로 빌더 인스턴스 자기자신을 전달하고, 대상 객체 생성자에서 빌더 인스턴스의 필드를 각각 대입하여 최종 완성본이 나오게 된다.

```java
class Person {
    // final 키워드로 필드들을 불변 객체로 만든다.
    private final String name;
    private final String age;
    private final String gender;
    private final String job;
    private final String birthday;
    private final String address;

    // 정적 내부 빌더 클래스
    public static class Builder {

        // 필수 파라미터
        private final String name;
        private final String age;

        // 선택 파라미터
        private String gender;
        private String job;
        private String birthday;
        private String address;

        // 필수 파라미터는 빌더 생성자로 받게 한다
        public Builder(String name, String age) {
            this.name = name;
            this.age = age;
        }

        // 선택 파라미터는 각 메서드를 통해 정의한다
        public Builder gender(String gender) {
            this.gender = gender;
            return this;
        }

        public Builder job(String job) {
            this.job = job;
            return this;
        }

        public Builder birthday(String birthday) {
            this.birthday = birthday;
            return this;
        }

        public Builder address(String address) {
            this.address = address;
            return this;
        }

        // 대상 객체의 private 생성자를 호출하여 최종 인스턴스화 한다
        public Person build() {
            return new Person(this); // 빌더 객체 자신을 넘긴다.
        }
    }

    // private 생성자 - 생성자는 외부에서 호출되는것이 아닌 빌더 클래스에서만 호출되기 때문에
    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.gender = builder.gender;
        this.job = builder.gender;
        this.birthday = builder.birthday;
        this.address = builder.address;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age='" + age + '\'' +
                ", gender='" + gender + '\'' +
                ", job='" + job + '\'' +
                ", birthday='" + birthday + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}
```

## 디렉터 빌더 패턴 (GOF)

GOF에서 정의하고 있는 디자인 패턴은 복잡한 객체의 생성 알고리즘과 조립 방법을 분리하여 빌드 공정을 구축하는것이 목적이다. 빌더를 받아 조립 방법을 정의한 클래스를 디렉터라고 부른다.

심플 빌더 패턴은 하나의 대상 객체에 대한 생성만을 목적에 두지만, 디렉터 빌더 패턴은 여러가지의 빌드 형식을 유연하게 처리하는 것에 목적을 둔다. 어찌보면 일반적인 빌더 패턴을 고도화시킨 패턴이라고 볼 수도 있다.

### 디렉터 빌더 패턴 구조

- Builder : 빌더 추상 클래스
- Director : Builder에서 제공하는 메서드들을 사용해 정해진 순서대로 Product 생성하는 프로세스를 정의
- ConcreteBuilder : Builder의 구현체. Product 생성을 담당.
- Product : Director가 Builder로 만들어낸 결과물

이러한 구조는 클라이언트가 직접 빌더의 모든 API를 사용하는 게 아닌, Director를 통해서 간단하게 인스턴스를 얻어올 수 있고 코드를 재사용할 수 있도록 한다.

### 디렉터 빌더 패턴 구현

다음 예제는 일반적인 자바 데이터를 저장하고 있는 Data 객체를 Builder 인터페이스를 통해 적절한 문자열 포맷으로 변환하는 예제이다.

- PlainTextBuilder : Data 인스턴스의 데이터들을 평이한 텍스트 형태로 만드는 API
- JSONBuilder : Data 인스턴스의 데이터들을 JSON 형태로 만드는 API
- XMLBuilder : Data 인스턴스의 데이터들을 XML 형태로 만드는 API

이때 각 문자열 포맷으로 변환하는 프로세스를 Director 객체가 담당하게 된다. 클라이언트는 Director를 이용하여 간단한 메서드 호출만으로 복잡한 빌드 공정 과정을 알 필요없이 결과물을 얻을 수 있다.

```java
class Data {
    private String name;
    private int age;

    public Data(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}

abstract class Builder {
    // 상속한 자식 클래스에서 사용하도록 protected 접근제어자 지정
    protected Data data;

    public Builder(Data data) {
        this.data = data;
    }

    // Data 객체의 데이터들을 원하는 형태의 문자열 포맷을 해주는 메서드들 (머리 - 중간 - 끝 형식)
    public abstract String head();
    public abstract String body();
    public abstract String foot();

}

// Data 데이터들을 평범한 문자열로 변환해주는 빌더
class PlainTextBuilder extends Builder {

    public PlainTextBuilder(Data data) {
        super(data);
    }

    @Override
    public String head() {
        return "";
    }

    @Override
    public String body() {
        StringBuilder sb = new StringBuilder();

        sb.append("Name: ");
        sb.append(data.getName());
        sb.append(", Age: ");
        sb.append(data.getAge());

        return sb.toString();
    }

    @Override
    public String foot() {
        return "";
    }
}

// Data 데이터들을 JSON 형태의 문자열로 변환해주는 빌더
class JSONBuilder extends Builder {

    public JSONBuilder(Data data) {
        super(data);
    }

    @Override
    public String head() {
        return "{\n";
    }

    @Override
    public String body() {
        StringBuilder sb = new StringBuilder();

        sb.append("\t\"Name\" : ");
        sb.append("\"" + data.getName() + "\",\n");
        sb.append("\t\"Age\" : ");
        sb.append(data.getAge());

        return sb.toString();
    }

    @Override
    public String foot() {
        return "\n}";
    }
}

// Data 데이터들을 XML 형태의 문자열로 변환해주는 빌더
class XMLBuilder extends Builder {
    public XMLBuilder(Data data) {
        super(data);
    }

    @Override
    public String head() {
        StringBuilder sb = new StringBuilder();

        sb.append("<?xml version=\"1.0\" encoding=\"UTF-8\" ?>\n");
        sb.append("<DATA>\n");

        return sb.toString();
    }

    @Override
    public String body() {
        StringBuilder sb = new StringBuilder();

        sb.append("\t<NAME>");
        sb.append(data.getName());
        sb.append("<NAME>");
        sb.append("\n\t<AGE>");
        sb.append(data.getAge());
        sb.append("<AGE>");

        return sb.toString();
    }

    @Override
    public String foot() {
        return "\n</DATA>";
    }
}

// 각 문자열 포맷 빌드 과정을 템플릿화 시킨 디렉터
class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }

    // 일종의 빌드 템플릿 메서드라 보면 된다
    public String build() {
        StringBuilder sb = new StringBuilder();

		// 빌더 구현체에서 정의한 생성 알고리즘이 실행됨
        sb.append(builder.head());
        sb.append(builder.body());
        sb.append(builder.foot());

        return sb.toString();
    }
}

public static void main(String[] args) {
    // 1. 포맷할 자바 데이터 생성
    Data data = new Data("홍길동", 44);

    // 2. 일반 텍스트로 포맷하여 출력하기
    Builder builder1 = new PlainTextBuilder(data);
    Director director1 = new Director(builder1);
    String result1 = director1.build();
    System.out.println(result1);

    // 3. JSON 형식으로 포맷하여 출력하기
    Builder builder2 = new JSONBuilder(data);
    Director director2 = new Director(builder2);
    String result2 = director2.build();
    System.out.println(result2);

    // 4. XML 형식으로 포맷하여 출력하기
    Builder builder3 = new XMLBuilder(data);
    Director director3 = new Director(builder3);
    String result3 = director3.build();
    System.out.println(result3);
}
```

이처럼 Director는 템플릿화 한 메서드를 통해 일관된 프로세스로 인스턴스를 만드는 빌드 과정을 단순화하고, 클라이언트 쪽에선 Director가 제공하는 메서드를 호출함으로써 코드를 재사용할 수 있게 된다.

즉, Builder는 부품을 만들고, Director는 Builder가 만든 부품을 조합해 제품을 만든다고 할 수 있다.

### GOF의 빌더 패턴은 여러 디자인 패턴의 짬뽕

지금까지 구현해본 GOF의 디렉터 빌더 패턴은 어찌 보면 빌드 과정을 추상화 및 단순화한 Facade 패턴과 빌드 과정 코드를 템플릿화한 Template Method 패턴 그리고 원하는 문자열 형식의 각 빌드 전략 알고리즘을 정의한 Stratage 패턴을 짬뽕시킨 디자인 패턴이라고 볼 수도 있다.

## Lombok의 @Builder

개발자가 좀더 편하게 빌더 패턴을 이용하기 위해 Lombok에서는 별도의 어노테이션을 지원한다. 클래스에 @Builder 어노테이션만 붙여주면 클래스를 컴파일 할 때 자동으로 클래스 내부에 빌더 API가 만들어진다.

단, Lombok의 @Builder는 GOF의 디렉터 빌더가 아닌 심플 빌더 패턴을 다룬다는 점은 유의하자.

### 빌더 어노테이션 구현

- @Builder : PersonBuilder 빌더 클래스와 이를 반환하는 builder() 메서드 생성
- @AllArgsConstructor(access = AccessLevel.PRIVATE) : @Builder 어노테이션을 선언하면 전체 인자를 갖는 생성자를 자동으로 만드는데, 이를 private 생성자로 설정
- @ToString : toString() 메서드 자동 생성

```java
import lombok.AccessLevel;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.ToString;

@Builder
@AllArgsConstructor(access = AccessLevel.PRIVATE)
@ToString
class Person {
    private final String name;
    private final String age;
    private final String gender;
    private final String job;
    private final String birthday;
    private final String address;
}
```

### 필수 파라미터 빌더 구현

@Builder 어노테이션으로 빌더 패턴을 구현하면 필수 파라미터 적용을 지정해줄수가 없다. 따라서 대상 객체 안에 별도의 `builder()` 정적 메서드를 구현함으로써, 빌더 객체를 생성하기 전에 필수 파라미터를 설정하도록 유도할 수 있고, 또한 파라미터 검증 로직도 추가해줄 수 있다.

```java
@Builder
@AllArgsConstructor(access = AccessLevel.PRIVATE)
@ToString
class Person {
    private final String name;
    private final String age;
    private final String gender;
    private final String job;
    private final String birthday;
    private final String address;

    // 필수 파라미터 빌더 메서드 구현
    public static PersonBuilder builder(String name, String age) {
        // 빌더의 파라미터 검증
        if(name == null || age == null)
            throw new IllegalArgumentException("필수 파라미터 누락");

        // 필수 파라미터를 미리 빌드한 빌더 객체를 반환 (지연 빌더 원리)
        return new PersonBuilder().name(name).age(age);
    }
}
```

# 실무에서 찾아보는 Builder 패턴

## Java

### StringBuilder

자바의 문자열 클래스하면 배우는 StringBuilder가 이름에서 보듯이 그 예이다.

빌더에 해당하는 StringBulder를 생성하고, 비더가 제공하는 append 메서드로 파라미터를 구성하고, 최종적으로 toString을 호출해서 String 객체를 생성하는 일련의 과정이 빌더 패턴이기 때문이다.

```java
public static void main(String[] args) {
    String result = new StringBuilder()
            .append("hello ")
            .append("world!")
            .toString(); // build()

    System.out.println(result);
}
```

### StreamBuilder

Stream API가 제공하는 StreamBuilder도 마찬가지다. StreamBuilder를 사용하면 Stream에 들어갈 요소를 add할 수 있고, 최종적으로 build를 호출해서 stream객체를 생성한다.

```java
public static void main(String[] args) {
    Stream.Builder<String> stringBuilder = Stream.builder();
    Stream<String> stream = stringBuilder.add("hello").add("world!").add("bye..").build();

    stream.forEach(System.out::println);
}
```

## Spring Framework

스프링 프레임워크에선 UriComponents 인스턴스를 Uricomponentsbuilder를 통해서 만들 수 있다. uri를 하드코딩해서 만든느 것보다 안전하게 만들 수 있다.

```java
public static void main(String[] args) {
    UriComponents uriComponents = UriComponentsBuilder.newInstance()
            .scheme("https")
            .host("kangworld.tistory.com")
            .build();

    System.out.println(uriComponents);
}
```