# Java 8에 추가된 것들

### Optional

객체를 다루다 보면 null의 위험에 휩싸이게된다. 그럴 경우 Optional을 이용해 편리하게 NullPointerException으로 부터 안전해질 수 있다.

- Optional 클래스는 객체를 생성하지 않고 메소드를 사용한다.

##### Optional 객체 생성 방법 3가지

1. 데이터가 없는 Optional 객체를 생성

```java
    Optional<String> emptyString Optional.empty()
```

2. null의 위험이 있다면

```java
    String common = null;
    Optional<String> nullableString = Optional.ofNullable(common);
```

3. null일 경우가 없고 반드시 데이터가 존재하는 상황에는

```java
    String common = "common";
    Optional<String> nullableString = Optional.of(common);
```

##### Optional 객체에 값이 있는지 확인하는 방법

```java
    private void checkOptionalData() {
        System.out.println(Optional.of("present").isPresent()); //true
        System.out.println(Optional.ofNullable(null).isPresent()); //false
    }
```

##### Optional 값을 꺼내는 방법

1. `get()` 데이터가 없을 경우 null 리턴<br>
2. `orElse("test")` 값이 없을 경우 기본값 리턴 ex)test<br>
3. Supplier\<T> 인터페이스를 활용하여 `orElseGet()`이용<br>
4. Supplier\<T> 인터페이스를 활용하여 `orElseThrow()`이용<br>

### Default Method

기존 인터페이스는 메소드를 구현하면 안됐었다. 하지만 엄청 큰 프로젝트에서 인터페이스에 새로운 메소드를 만들어야 하는 상황이 생길수 도 있다. 이런 상황을 위해서 하위 호완성을 위해 Default Method를 추가하였다.

```java
    public interface Test {
        static final int since = 2021;
        String getSince();
        default String getEmail() {
            return "@naver.com";
        }
    }
```

### Lamda 표현식

익명 클래스는 가독성도 떨어지고 불편하다 그래서 람다 표현식이 나왔다. 대신에 람다표현식은 인터페이스에 메소드가 `하나`인 것들만 적용 가능하다.

```java
    interface Calculate {
        int operation(int a, int b);
    }
```

```java
    private void Test() {
        Calculate calculateAdd = new Calculate() {
            @Override
            public int operation(int a, int b) {
                return a+b;
            }
        }
        System.out.println(calculateAdd);
    }
```

원래는 이처럼 익명클래스를 구현하였다. 하지만 람다식으로 표현한다면

```java
    private void Test() {
        Calculate calculateAdd = (a, b) -> a+b;
        System.out.println(calculateAdd);
    }
```

간결하게 표현할 수 있다.<br>
<br>
하지만, 협업하면 누군가 이 인터페이스만 보고 람다식으로 구현했는지 알기 힘들다. 근데 이 인터페이스에 누군가 메소드를 추가해버리면 기존에 있던 코드가 컴파일 에러가 날 것이다. 이러한 혼동을 피하기 위하여
어노테이션을 사용하여 선언할 수 있는 메소드를 `하나`로 지정한다.

```java
    @FunctionalInterface
    interface Calculate {
        int operation(int a, int b);
    }
```

### Stream

스트림은 `연속된 정보`를 처리하는 데 사용한다. `연속되는 정보`란 배열과 컬렉션이 있다. 하지만 스트림에서 배열은 사용할 수 없다. 그렇기에 배열을 컬렉션으로 변환한뒤 사용할 수는 있다.

##### 스트림의 구조

```java
    list.stream().filter(x -> x>10).count()
```

- `스트림 생성` : 컬렉션 목록을 스트림 객체로 변환한다.
- `중개 연산` : 스트림 객체를 사용하여 연산을 처리한다. 하지만 결과는 리턴하지 못한다.
- `종단 연산` : 스트림의 결과를 리턴해준다. (중개연산은 없어도 된다.)

##### 종단 연산 forEach()

- 스트림의 요소를 하나씩 꺼내어 처리할 수 있다.

```java
    list.stream().forEach(x -> System.out.println(x));
```

##### 메소드 참조

위의 코드를 아래와 같이 참조형으로 만들 수 있다 :)

```java
    list.stream().forEach(System.out::println));
```

요소를 하나씩 꺼내어 왼쪽에 클래스의 오른쪽 메소드를 이용하겠다는 뜻이다.

##### 중개 연산

```java
    list.stream().filter(x->x>10).count();
    list.stream().map(x->x*3).forEach(System.out::println);
```

`filter()`는 안의 조건식으로 x를 걸러낸다는 뜻이고, `map()`은 요소를 하나씩 꺼내어 해당 하는 코드를 실행해준다.
이 외에도 다양한 중간 연산과 종단 연산이 있다. API를 확인해보자
