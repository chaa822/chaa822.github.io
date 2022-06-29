---
title: 더 자바, Java8 - 함수형 인터페이스와 람다 표현식
date: 2022-02-10
tags:
- TheJava-Java8
keywords:
- 더 자바
- Java8
---

인터페이스 생성
다음과 같이 추상 메서드가 딱 하나만 존재하면, 함수형 인터페이스라고 정의한다.
```Java
public interface RunSomething {
    
    /* public, abstract 키워드는 생략 가능. */
    void doIt();
}
```

다음과 같이 추상 메서드가 2개 이상일 경우, 함수형 인터페이스가 아니다.
```Java
public interface RunSomething {
    
    /* public, abstract 키워드는 생략 가능. */
    void doIt();
    
    void doItAgain();
}
```

static, default 메서드를 정의할 수도 있으며 다른 형태의 메서드를 정의하더라도
<br/>추상 메서드는 하나이기 때문에 함수형 인터페이스라고 할 수 있다.
```Java
public interface RunSomething {
    
    /* public, abstract 키워드는 생략 가능. */
    void doIt();
    
    static void printName(){
        System.out.println("TheJava8");
    }
    
    default void printAge(){
        System.out.println("20");
    }
}
```

만약 함수형 인터페이스를 명시적으로 정의하고 싶다면
<br/>자바가 기본적으로 제공하는 어노테이션인 @FunctionalInterface를 사용하여 좀 더 견고하게 사용할 수 있다.
(추상 메서드가 2개 이상일 경우 오류를 노출함)

```Java
@FunctionalInterface
public interface RunSomething {
    
    /* public, abstract 키워드는 생략 가능. */
    void doIt();
    
    static void printName(){
        System.out.println("TheJava8");
    }
    
    default void printAge(){
        System.out.println("20");
    }
}
```

RunSomething 인터페이스를 구현해보자.
<br/>아래와 같은 형태를 익명 내부 클래스(anonymous inner class)라고 한다.
```Java
public class Foo {
    public static void main(String[] args) {
        
        // 익명 내부 클래스 (anonymous inner class)
        RunSomething runSomething = new RunSomething() {
            @Override
            public void doIt() {
                System.out.println("hello");
                System.out.println("Rambda");
            }
        };
        
        // hello
        // Rambda 출력
        runSomething.doIt();
    }
}
```

위와 같은 익명 내부 클래스를 람다 표현식의 형태로 표현하면 다음과 같다.
단, 이러한 코드는 내부코드가 한줄일 경우에만 가능하고, 2줄 이상일 경우 중괄호로 묶어줘야 한다.
```Java
public class Foo {
    public static void main(String[] args) {
        
        // rambda 표현식으로 변경 가능. 단, doIt안의 코드가 한 줄 일 경우에만
        RunSomething runSomething = () -> System.out.println("hello");
        runSomething.doIt();
        
        // 2줄 이상일 경우 중괄호로 묶어야 한다.
        RunSomething runSomething2 = () -> {
            System.out.println("hello");
            System.out.println("world");
        };
        runSomething2.doIt();
    }
}
```

위와 같이 람다 표현식을 사용하면 마치 다른 언어와 같이 함수를 정의한 것으로 보일 수 있지만
<br/>실제로는 함수형 인터페이스를 인라인으로 구현한 특수한 형태의 오브젝트이다.

또한 함수를 파라미터로 받는다거나, 함수가 함수를 리턴하는 것이 가능하다.
<br/>(함수를 First class object로 사용할 수 있다.)

<br/>위에서 선언한 추상 메서드인 doIt의 리턴 값을 int로 변경해보자.
```Java
@FunctionalInterface
public interface RunSomething {

    int doIt(int number);
}
```

그리고 아래와 같이 파라미터에 10을 더해서 값을 리턴하는 함수를 정의할 경우,
<br/>입력한 값에 따라 결과 값을 보장할 수 있다.

```Java
public class Foo {

    public static void main(String[] args) {

        // 입력 받은 값이 동일한 경우 결과가 같아야한다.
        RunSomething runSomething = (number) -> {
            return number + 10;
        };

        // 11 출력
        System.out.println( runSomething.doIt(1) );
        // 12 출력
        System.out.println( runSomething.doIt(2) );
    }
}

```

그런데 아래와 같이 리턴 값이 보장해주지 못하는 상황이 발생할 가능성이 있다면
<br/>함수형 프로그래밍이(퓨어 함수)라고 판단할 수 없다.
<br/>
<br/>
예) 함수 바깥의 변수를 사용하는 경우 
```Java
public class Foo {

    public static void main(String[] args) {
        
        // 함수의 바깥의 변수
        // int baseNumber = 10;
        
        RunSomething runSomething = new RunSomething() {
            
            // 함수 바깥의 변수
            int baseNumber = 10;
            
            @Override
            public void doIt(number) {
                return number + baseNumber;
            }
        };
        runSomething.doIt();
    }
}
```

예) 외부의 값을 변경하려는 경우
```Java
public class Foo {
    public static void main(String[] args) {
        int baseNumber = 10;
        
        RunSomething runSomething = new RunSomething() {
            @Override
            public void doIt() {
                // 컴파일 오류
                baseNumber++;
                return number + baseNumber;
            }
        };
        runSomething.doIt();
    }
}
```

출처 :
<br/> 인프런 강의 - 더 자바, Java 8 (백기선)
<br/>https://www.inflearn.com/course/the-java-java8
