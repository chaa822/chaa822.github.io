---
title: 더 자바, Java8 - 인터페이스 기본 메소드와 스태틱 메소드
date: 2022-06-23
tags:
- TheJava-Java8
keywords:
- 더 자바
- Java8
---

인터페이스 생성, 메서드 정의
```Java
public interface Foo {
    
    void printName();
}
```

인터페이스를 구현할 클래스 생성, 메서드 구현
```Java
public class DefaultFoo implements Foo {

    String name;

    // 파라미터로 name을 받는다고 가정
    public DefaultFoo(String name){
        this.name = name;
    }

    @Override
    public void printName(){
        System.out.println(this.name);
    }
}
```

인스턴스 생성 및 메서드 호출
```Java
public class App {

    public static void main(String[] args) {

        Foo foo = new DefaultFoo("TheJava8");
        
        // TheJava8 출력
        foo.printName();
    }
}

```

인터페이스에 새로운 메서드를 추가할 경우, 인터페이스를 구현한 모든 클래스에서 오류가 발생한다.
<br/>이럴 경우, default 키워드를 이용해 기본 메서드를 오류 없이 추가할 수 있다.
```Java
public interface Foo {
    
    void printName();
    
    default void printNameUpperCase(){
        System.out.println(getName().toUpperCase());
    }
    
    String getName();
}
```
```Java
public class DefaultFoo implements Foo {
    
    @Override
    public void printName(){
        System.out.println(this.name);
    }
    
    String name;
    
    // 파라미터로 name을 받는다고 가정
    public DefaultFoo(String name){
        this.name = name;
    }
    
    // getName 메서드 구현
    @Override
    public String getName() {
        return this.name;
    }
}
```
```Java
public class App {

    public static void main(String[] args) {
    
        Foo foo = new DefaultFoo("TheJava8");
        
        // TheJava8 출력
        foo.printName();
        
        // THEJAVA8 출력
        foo.printNameUpperCase();
    }
}
```

기본 메서드는 구현하는 클래스에서 재정의할 수도 있다.
```Java
public class DefaultFoo implements Foo {

    @Override
    public void printName(){
        System.out.println(this.name);
    }

    String name;

    // 파라미터로 name을 받는다고 가정
    public DefaultFoo(String name){
        this.name = name;
    }
    
    @Override
    public void printNameUpperCase() {
        System.out.println("Overrided " + this.name.toUpperCase());
    }

    // getName 메서드 구현
    @Override
    public String getName() {
        return this.name;
    }
}
```
```Java
public class App {

    public static void main(String[] args) {

        Foo foo = new DefaultFoo("TheJava8");
        
        // TheJava8 출력
        foo.printName();
        
        // Overrided THEJAVA8 출력
        foo.printNameUpperCase();
    }
}
```

하지만 인터페이스에서 제공하는 default 기능이 모든 구현체에서 정상적으로 동작하리라는 보장이 없다.
<br/>(어떻게 구현했는지 모르기 때문에)
<br/>
<br/>
따라서 오류 방지를 위해 최소한의 노력으로 아래와 같이 @ImplSpec을 이용해 문서화를 진행해야한다.

```Java
public interface Foo {
    
    void printName();
    
    /**
     * @ImplSpec 이 구현체는 getName()으로 가져온 문자를 대문자로 출력한다.
     */
    default void printNameUpperCase(){
        System.out.println(getName().toUpperCase());
    }
    
    String getName();
}
```

Object가 제공하는 기능은 default 메서드로 제공할 수 없다.
<br/> 하지만 추상 메서드로 선언하는 것은 가능하다.
```Java
public interface Foo {
    .
    .
    .
    // 불가능 (error)
    default String toString(){}
    
    // 가능
    String toString();
}
```

Foo 인터페이스를 상속받은 Bar 인터페이스에서 Foo가 제공하는 기본 메서드를 제공하고 싶지 않을 경우
<br/>Bar 인터페이스에서 다시 추상 메서드로 선언한다.
```Java
public interface Bar extends Foo {

    // 이렇게 하면 Bar를 구현한 클래스에서 Override 해야한다.
    void printNameUpperCase();
}
```
```Java
public class DefaultBar implements Bar {

    String name;

    public DefaultBar(String name){
        this.name = name;
    }

    @Override
    public void printName() {
        System.out.println(this.name);
    }

    @Override
    public String getName() {
        return this.name;
    }
    
    @Override
    public void printNameUpperCase() {
        System.out.println("BAR");
    }
}
```
```Java
public class App {

    public static void main(String[] args) {

        Bar bar = new DefaultBar("TheJava8");
        
        // TheJava8 출력
        bar.printName();
        
        // BAR 출력
        bar.printNameUpperCase();
    }
}

```

Foo 인터페이스에도 default가 있고 Bar에도 default가 있는 경우, 구현하는 인터페이스에서 Foo/Bar 인터페이스를 둘 다 implements하게 되면 논리적으로 둘 중에 어떤 것을 사용해야할지 모르기 때문에 애매한 상황이 발생하여 컴파일 오류가 발생한다.
</br>이렇게 충돌하는 경우에는 직접 Override를 해야 한다.
```Java
public interface Bar {

    void printName();

    default void printNameUpperCase(){
        System.out.println(getName().toUpperCase());
    }

    String getName();
}
```
```Java
public interface Foo {

    void printName();

    default void printNameUpperCase(){
        System.out.println(getName().toUpperCase());
    }

    String getName();
}
```
```Java
public class DefaultBar implements Foo, Bar {

    String name;

    public DefaultBar(String name){
        this.name = name;
    }

    @Override
    public void printName() {
        System.out.println(this.name);
    }

    @Override
    public String getName() {
        return this.name;
    }

    @Override
    public void printNameUpperCase() {
        System.out.println("DEFAULT BAR");
    }
}
```
```Java
public class App {

    public static void main(String[] args) {

        Bar bar = new DefaultBar("TheJava8");
        
        // TheJava8 출력
        bar.printName();
        
        // DEFAULT BAR 출력
        bar.printNameUpperCase();
    }
}

```

유틸리티나 헬퍼 메서드를 제공하고 싶은 경우, static 키워드를 사용하여 메서드를 제공할 수 있다.
```Java
public interface Foo {
    .
    .
    static void printAnything(){
        System.out.println("Foo");
    }
}
```
```Java
public class App {

    public static void main(String[] args) {
        
        Foo.printAnything();
    }
}

```

출처 :
<br/> 인프런 강의 - 더 자바, Java 8 (백기선)
<br/>https://www.inflearn.com/course/the-java-java8
