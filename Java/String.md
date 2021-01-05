# String

### String 클래스

```java
    public final class String extends Object implements Serializable, Comparable<String>, CharSequence {
        ...
    }
```

기본적으로 String은 Object 클래스를 상속받았다 또한, Serializable을 통하여 객체의 직렬화를 가능하게 하였고 Comparable<String>을 통하여 compareTo()를 구현하여 String 객체간 비교를 가능하게 하고 CharSequence를 통하여 해당 클래스가 문자열을 다루기 위한 클래스라는 것을 명시적으로 나타낸다.

### String 생성 방식

1. new를 이용한 방식

```java
String test = new String("test");
```

2. 리터럴을 이용한 방식

```java
String test2 = "test2";
```

- new를 이용한 방식에서는 "test"라는 String 객체가 힙 영역에 생성되어 주소를 참조하게 된다.
- 하지만, 리터럴을 사용하면 String Constant Pool이라는 영역에 생성되는데 이 영역에서는 같은 문자열 리터럴이 이 영역 안에 있다면 새로 문자열을 생성하여 반환하는 것이 아닌 원래 있던 문자열의 주소값을 참조하게 해준다.

예를 들면

```java
    public void test() {
        String text1 = "abc";
        String text2 = "abc";
        String test3 = new String("abc")

        System.out.println(test1==test2) //true
        System.out.println(test1==test3) //false
        System.out.println(test1.equals(test3)) //true
    }
```

이런 결과를 반환해준다.

- `==`은 객체의 주소값을 비교하여 boolean 값을 리턴한다.
- `equals()`는 객체의 주소값이 아니라 문자열 내용을 비교하여 boolean 값을 리턴한다.

### immutable한 String

String은 immutable하다. 즉, 불변이다. 하지만 더하기 연산이 가능하다. 이는 String 객체끼리 더하면 새로운 객체를 만들어 반환해주는 것이다. 그래서 String 클래스의 단점을 보완하기 나온 클래스가 StringBuffer와 StringBuilder이다. String Builder와 StringBuffer는 mutable하다. 단순히 append() 메소드를 이용하여 객체에 문자열을 추가할 수 있다. 하지만 Builder와 Buffer의 차이점은 StringBuilder는 스레드에 안전하지 않다는 것이고 StringBuffer는 Syncronized를 이용하여 구현하였기 때문에 스레드에 안전하지만 StringBuilder보다 느리다.

하지만 JDK 5 이상에서는 String 객체가 더하기 연산을 할 경우에도 컴파일할 때 내부적으로 StringBuilder로 변환해 준다. 그치만 for 루프와 같이 반복 연산을 할 때에는 자동으로 변환을 해주지 않으므로 StringBuilder와 StringBuffer가 필요하다.
