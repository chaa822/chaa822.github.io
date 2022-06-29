---
title: 더 자바, Java8 - 자바에서 제공하는 함수형 인터페이스
date: 2022-02-12
tags:
- TheJava-Java8
keywords:
- 더 자바
- Java8
---

먼저, 클래스에서 Function 인터페이스를 구현하여 사용할 수 있다.
```Java
public class Plus10 implements Function<Integer, Integer> {

    // Function 인터페이스를 구현한다.
    
    @Override
    public Integer apply(Integer integer){
        return integer + 10;
    }
}
```
```Java
public class Foo {

    public static void main(String[] args) {

        // Plus10 클래스의 객체를 생성하여 호출한다.
        Plus10 plus10 = new Plus10();
        
        // 11출력
        System.out.println( plus10.apply(1) );
    }
}
```

아니면 자바에서 기본적으로 제공하는 함수형 인터페이스를 사용할 수 있다.
 
```Java
public class Foo {

    public static void main(String[] args) {

        // Function : T 타입을 받아서 R 타입을 리턴하는 함수 인터페이스
        // 별도의 클래스 없이 바로 구현할 수 있다.
        
        // 파라미터에 20을 더하는 함수
        Function<Integer, Integer> plus20 = (i) -> i + 20;
        // 21 출력
        System.out.println( plus20.apply(1) );

        // 파라미터에 2를 곱하는 함수
        Function<Integer, Integer> multiply2 = (i) -> i * 2;
        // 2 출력
        System.out.println( multiply2.apply(1) );

        // 조합도 가능
        // compose : 10을 더하기 전에, 2를 곱하겠다.
        Function<Integer, Integer> multiply2AndPlus10 = plus10.compose(multiply2);
        // 14 출력
        System.out.println( multiply2AndPlus10.apply(2) );

        // andThen : 10을 더한 뒤, 2를 곱하겠다.
        Function<Integer, Integer> plus10AndMultiply2 = plus10.andThen(multiply2);
        // 24 출력
        System.out.println( plus10AndMultiply2.apply(2) );

        // Consumer : T 타입을 받아서 아무값도 리턴하지 않는 함수 인터페이스
        // 파라미터를 받아서 출력하겠다.
        Consumer<Integer> printT = (i) -> System.out.println(i);
        // 10 출력
        printT.accept(10);

        // Supplier : T 타입의 값을 제공하는 함수 인터페이스 (입력 값을 받지 않는다.)
        // 무조건 10을 리턴하는 함수
        Supplier<Integer> get10 = () -> 10;
        // 10 출력
        System.out.println( get10.get() );

        // Predicate : T 타입을 받아서 boolean을 리턴하는 함수 인터페이스
        // a로 시작하는지 true/false를 리턴하는 함수
        Predicate<String> startsWith = (s) -> s.startsWith("a");
        // true 출력
        System.out.println( startsWith.test("abcd") );

        // 짝수인지 검사하는 함수
        Predicate<Integer> isEven = (num) -> num % 2 == 0;
        // true 출력
        System.out.println( isEven.test(2) );

        // Predicate 조합도 가능
        // negate : true/false에 대해서 not을 붙인다.
        Predicate<Integer> isOdd = isEven.negate();
        // false 출력
        System.out.println( isOdd.test(2) );

        // and : 동시 조건
        // false 출력
        System.out.println( isEven.and(isOdd).test(1) );

        // or : 따로 조건
        // true 출력
        System.out.println( isEven.or(isOdd).test(1));

        // UnaryOperator :
        // Function<T, R>의 특수한 형태로, 입력값 하나를 받아서 동일한 타입을 리턴하는 함수 인터페이스
        UnaryOperator<Integer> integerUnaryOperator = (i) -> i + 10;
        // 11 출력
        System.out.println( integerUnaryOperator.apply(1) );
    }
}

```

출처 :
<br/> 인프런 강의 - 더 자바, Java 8 (백기선)
<br/>https://www.inflearn.com/course/the-java-java8
