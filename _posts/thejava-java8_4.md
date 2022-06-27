---
title: 더 자바, Java8 - 메소드 레퍼런스
date: 2022-02-14
tags:
- TheJava-Java8
keywords:
- 더 자바
- Java8
---

람다를 사용해 문자를 인자로 받아 문자를 리턴하는 함수를 만들 경우, 다음과 같이 만든다.

```Java
public class App {

    public static void main(String[] args) {

        // Function : String을 받아서 String을 내보냄
        Function<String, String> hi1 = (s) -> "hi" + s;
        // hi TheJava8 출력
        System.out.println( hi1.apply("TheJava8") );
        
        // Input과 Ouput이 같은 경우, String을 하나로 줄일 수 있음
        UnaryOperator<String> hi2 = (s) -> "hi " + s;
        // hi TheJava8 출력
        System.out.println( hi2.apply("TheJava8") );
    }
}
```

위와 같이 인라인으로 람다 표현식을 작성할 수도 있지만, 다음과 같이 클래스를 이용해 생성할 수 있다.

Greeting 클래스 생성
```Java
public class Greeting {

    private String name;

    public Greeting() {

    }

    public Greeting(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public String hello(String name) {
        return "hello " + name;
    }

    public static String hi(String name) {
        return "hi " + name;
    }
}
```

```Java
public class App {
    public static void main(String[] args) {
        .
        .
        .
        // 클래스를 이용해 생성 -> 메서드가 static이어야 한다.
        UnaryOperator<String> hi3 = Greeting::hi;
        
        // 스태틱 메서드가 아닌, 인스턴스 메서드를 이용해 생성할 경우 다음과 같이 작성
        Greeting greeting = new Greeting();
        UnaryOperator<String> hello = greeting::hello;
        System.out.println( hello.apply("TheJava8") );
    }
}
```
Supplier를 이용해 Greeting 클래스의 인스턴스를 가져올 수도 있다.
<br/>
다만 주의할 것은, newGreeting이 Greeting 클래스의 인스턴스가 아니므로 get 함수를 호출해 객체 생성을 해줘야 한다는 것이다.

```Java
public class App {
    public static void main(String[] args) {
        .
        .
        .
        // Supplier를 이용해 greeting 객체를 가져오는 함수 생성
        Supplier<Greeting> newGreeting = Greeting::new;
        // 주의 : newGreeting이 Greeting 클래스의 객체가 아니므로
        // .get() 함수를 이용해 객체 생성을 해줘야 한다.
        Greeting greeting2 = newGreeting.get();
    }
}
```

String name을 파라미터로 받는 생성자를 호출하고 싶다면 Function 인터페이스를 사용함이 옳다.
<br/>
다만, 아래의 Function과 위의 Supplier는 똑같이 'Greeting::'라는 키워드를 사용하지만
<br/>호출되는 생성자는 서로 다르므로 혼동되지 말아야한다.

```Java
public class App {

    public static void main(String[] args) {
        .
        .
        .
        Function<String, Greeting> greeting3 = Greeting::new;
        Greeting theJava8 = greeting3.apply("TheJava8");
        System.out.println( theJava8.getName() );
    }
}
```

일반적으로, 배열을 정렬할 때 다음과 같이 한다.
```Java
public class App {
    
    public static void main(String[] args) {
        .
        .
        .
        String[] names = {"TheJava8", "spring", "dev"};
        Arrays.sort(names, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return 0;
            }
        });
    }
}
```

Comparator도 함수형 인터페이스로 변경되어 다음과 같이 작성이 가능하다.
```Java
public class App {
    public static void main(String[] args) {
        .
        .
        .
        Arrays.sort(names, (o1, o2) -> 0);
        Arrays.sort(names, String::compareToIgnoreCase);
        System.out.println(Arrays.toString(names));
    }
}
```

출처 :
<br/> 인프런 강의 - 더 자바, Java 8 (백기선)
<br/>https://www.inflearn.com/course/the-java-java8
