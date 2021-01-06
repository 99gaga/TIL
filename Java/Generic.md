# Generic

### 제네릭이 왜 필요한가?

아무리 개발자가 테스트를 열심히 해도 생각지 못한 부분에서 실행 도중 예외가 발생할 수도 있다. 그 예외 중에 형변환시 예외가 발생할 수도 있고 정확하게 하려면 번거롭게 instancof로 타입점검을 할 수 있다. 하지만 제네릭으로 이런 번거롭고 장애가 날 수 있는 상황을 제네릭으로 해결할 수 있다.<br>
또한 유지보수시에도 타입을 지정했기 때문에 다른사람이 이해하기 쉽다는 장점이 있다.

### 제네릭이란?

제네릭이란 실행중이 아닌 컴파일시에 타입을 사전 확인해주고 컴파일 에러를 발생시킨다.

```java
    public class Test<T> {
        private T object;
        public void setObject(T obj) {
            this.object = obj;
        }
        public T getObject() {
            return object;
        }
    }
```

```java
    public static void main(String[] args) {
        Test<String> test = new Test();
        test.set("abcdefg");
        System.out.println(test.getObject()); //abcdefg
    }
```

이처럼 < T > 같이 클래스를 만듦으로써 객체 생성시에 타입을 지정해줄 수 있다. 그래서 `test.set(3)`을 한다면 컴파일 에러가 날 것이다. 따라서 "실행 시"에 다른 타입으로 잘못 형 변환하여 예외가 발생하는 일은 없다.
<b>명시적으로 타입을 지정할 때 사용하는 것이다.</b>

### 제네릭 타입 이름

<> 안에 들어가는 T는 T대신 어떤 이름으로 사용해도 괜찮지만 자바에서 정한 규칙이 있으니 따르는 것이 좋다.
`E: 요소`, `K: 키`, `N: 숫자`, `T: 타입`, `V: 값`, `S, U, V: 두 번째, 세번째, 네 번째에 선언된 타입`

### WildCard Type

객체를 생성할 때 타입을 지정할 수 있었다. 하지만 제네릭으로 구현한 클래스를 매개변수로 받는 메소드가 있다면 타입을 어떤 것으로 지정해야할까? 만약 String 하나만으로 제한한다면 Integer나 Double같이 다른 타입으로 생성한 객체로는 메소드에 매개변수로 전달할 수 가 없다. 그래서 이를 해결한 것이 와일드 카드 타입이다.

```java
    public static void main(String[] args) {
        Test<String> test = new Test();
        test.set("wild");
        wildcardMethod(test);
    }

    public void wildcardMethod(Test<?> c) {
        Object value = c.Object();
        System.out.println(value);
    }
```

이와 같이 메소드 안에 매개변수를 `<?>` 물음표로 타입을 유동적으로 가져올 수 있다. 하지만 메소드 내부에서는 어떤 타입이 올지 모르기 때문에 Object로 처리해야만 한다.

### Bounded WildCards

```java
    public void wildcardMethod(Test<? extends Car> c) {
        Object value = c.Object();
        System.out.println(value);
    }
```

`<? extends Test>`는 Car 클래스를 상속받은 클래스 타입만 제한적으로 허용하겠다는 의미이다 이런 방식으로 만들면 `wildcardMethod(Test<? extends Test> c)`를 사용할 수 있는 타입의 범위를 지정할 수 있다.

### 제니릭한 메소드 예제

```java
    public <T extends Car> void wildcardMethod(Test<T> c, T addValue) {
        c.setObject(addValue);
        T value = c.getObject();
        System.out.println(value);
    }
```

```java
    public <S, T extends Car> void multiGenericMethod(Test<T> c, T addValue, S otherValue) {

    }
```
