---
title: 더 자바, Java8 - 람다 표현식
date: 2022-02-13
tags:
- TheJava-Java8
keywords:
- 더 자바
- Java8
---

람다의 구조
```Java
pubic class Foo {
    
    // main
    public static void main(String[] args) {
    
        // () 부분 : 인자를 선언
        // -> 이후 부분 : 함수의 바디
        // 바디가 한 줄이면 중괄호를 생략 가능
        UnaryOperator<Integer> plus10 = (i) -> i + 10;
        UnaryOperator<Integer> plus20 = (i) -> {
            i = i + 10;
            i = i + 10;
            return i;
        };
        // 11 출력
        System.out.println(plus10.apply( 1 ));
        // 21 출력
        System.out.println(plus20.apply( 1 ));
        
        // Supplier의 경우, 인자를 받지 않기 때문에 ()부분이 비어 있음
        // 마찬가지로 한줄일 경우 생략 가능
        Supplier<Integer> get10 = () -> 10;
        // 10 출력
        System.out.println( get10.get() );
        
        // BiFuction의 경우, 인자 두개를 받아서 하나의 결과 값을 리턴하는 함수형 인터페이스
        BiFunction<Integer, Integer, Integer> sum1 = (a, b) -> a + b;
        // 인자와 리턴 값이 변수형이 같다면 BinaryOperator<T>로 줄일 수 있다.
        BinaryOperator<Integer> sum2 = (a, b) -> a + b;
        // 20 출력
        System.out.println( sum1.apply(10, 10) );
        // 20 출력
        System.out.println( sum2.apply(10, 10) );
    }
}
```

람다의 범위
<br/>
<br/>
로컬 클래스나 익명 클래스의 경우, 함수 바깥에 있는 변수를 참조할 수 있다.
<br/>
이 때, 참조하는 baseNumber 변수가 사실상 변경되지 않는 경우 final 키워드를 생략할 수 있다.
<br/>


```Java
public class Foo {
    public static void main(String[] args) {
        Foo foo = new Foo();
        foo.run();
    }
    
    priavte void run() {
        // effective final
        // final 키워드를 생략할 수 있는 경우 : 이 변수가 사실상 final인 경우 (변경되지 않음)
        final int baseNumber = 10;
        
        // 로컬 클래스
        class localClass {
            void printNumber(){
                System.out.println(baseNumber);
            }
        }
        
        // 익명 클래스
        Consumer<Integer> integerConsumer = new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {
                System.out.println(baseNumber);
            }
        };
    }
}
```

또한 로컬 클래스나 익명 클래스 내부에 baseNumber 변수를 선언해도 함수의 범위가 다르기 때문에
<br/>
오류가 발생하지 않으며, 내부의 baseNumber 변수가 우선 시 된다.
<br/>
이를 쉐도잉 (로컬 클래스 or 익명 클래스 함수 안의 변수가 외부의 변수보다 우선시 됨)이라고 한다.

```Java
public class Foo {

    public static void main(String[] args) {
        Foo foo = new Foo();
        foo.run();
    }
    
    private void run(){
        // effective final
        // final 키워드를 생략할 수 있는 경우 : 이 변수가 사실상 final인 경우 (변경되지 않음)
        final int baseNumber = 10;
        
        // 로컬 클래스
        class localClass {
            void printNumber(){
                // 함수의 범위가 다르기 때문에 내부의 baseNumber가 출력되며
                // 오류가 발생하지 않는다.
                int baseNumber = 12;
                // 12 출력
                System.out.println(baseNumber);
            }
        }

        // 익명 클래스
        Consumer<Integer> integerConsumer = new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {
                // 함수의 범위가 다르기 때문에 내부의 baseNumber가 출력되며
                // 오류가 발생하지 않는다.
                int baseNumber = 13;
                // 13 출력
                System.out.println(baseNumber);
            }
        };
    }
}
```

하지만 람다의 함수 범위는 상위 run 함수와 같기 때문에 (쉐도잉이 되지 않는다)
<br/>
baseNumber 변수를 선언할 경우 컴파일 오류가 발생한다.
<br/>


```Java
public class Foo {
    
    public static void main(String[] args) {
        Foo foo = new Foo();
        foo.run();
    }
    
    private void run() {
        
        final int baseNumber = 10;
        .
        .
        .
        // 람다
        IntConsumer printInt = (i) -> {
            // ERROR : Variable baseNumber is already defined in the scope.
//            int baseNumber = 11;
            System.out.println(i + baseNumber);
        };
    }
}
```

또한 람다 밑에서 baseNumber의 값을 변경하려고 할 경우,
<br/>
이미 baseNumber는 effective final이기 때문에 컴파일 오류가 발생한다.
```Java
public class Foo {
    
    public static void main(String[] args) {
        Foo foo = new Foo();
        foo.run();
    }
    
    private void run() {
        
        final int baseNumber = 10;
        .
        .
        .
        // 람다
        IntConsumer printInt = (i) -> {
            // ERROR
            // Variable used in lambda expression should be final or effectively final
            System.out.println(i + baseNumber);
        };
        
        baseNumber++;
    }
}
```

출처 :
<br/> 인프런 강의 - 더 자바, Java 8 (백기선)
<br/>https://www.inflearn.com/course/the-java-java8
