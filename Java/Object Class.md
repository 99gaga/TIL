# 모든 클래스의 부모 Object Class

자바에서는 모든 클래스는 Object 클래스를 기본적으로 확장하고있다. 그러므로 Object의 메소드를 사용할 수 있다.

### 왜 Object클래스를 상속하게 한걸까?

최소한 클래스가 만들어진다면 이 정도의 메소드는 정의 되있어야 하고 처리해 주어야하기 때문에 Obejct 클래스를 상속받아 해결한 것이다.

### Object 클래스의 메소드

**1. 객체를 처리하기 위한 메소드**

- 객체의 복사본을 만들어 리턴한다.

```java
    protected Object clone()
```

- 현재 객체와 매개 변수로 받은 객체가 같은지 확인한다.

```java
    public boolean equals(Object obj)
```

- 현재 객체가 더 이상 쓸모가 없어졌을 때 GC에 의해서 이 메소드가 호출된다.

```java
    protected void finalize()
```

- 현재 객체의 Class 클래스의 객체를 리턴한다.

```java
    public Class<?> getClass()
```

- 객체에 대한 해시코드 값을 리턴한다. (16진수로 제공되는 객체의 메모리주소)

```java
    public int hashcode()
```

- 객체를 문자열로 표현하는 값을 리턴한다.

```java
    public String toString()
```

<br>

**2. 쓰레드를 위한 메소드**

- 이 객체의 모니터에 대기하고 있는 단일 쓰레드를 깨운다.

```java
    public void notify()
```

- 이 객체의 모니터에 대기하고 있는 모든 쓰레드를 깨운다.

```java
    public void notifyAll()
```

- 다른 쓰레드가 현재 객체에 대해 notify() 나 nofiyAll()메소드를 호출할 때까지 현재 쓰레드를 대기시킨다.

```java
    public void wait()
```

- 매개 변수에 지정한 시간만큼 대기한다. ex) 1000 = 1초

```java
    public void wait(long timeout)
```

- wait(long timeout)과 동일 하지만 + 나노초만큼 더 세밀히 지정할 수 있다.

```java
    public void wait(long timeout, int nanos)
```

### toString()

toString이 자동으로 호출되는 경우가 있다.

- System.out.println() 메소드에 매개 변수로 들어가는 경우
- 객체에 대하여 더하기 연산을 하는 경우

```java
    public static void main(String[] args) {
        ToString toString = new ToString();
        toString.toStringMethod();
    }

    public void toStringMethod() {
        System.out.println(this); //default.ToString@7c30a502
        System.out.println(toString()); //default.ToString@7c30a502
        System.out.println("plus " + this); //plus default.ToString@7c30a502
    }
```

실제 구현되어 있는 메소드는 `getClass().getName() + '@' + Integer.toHexString(hashCode())` 이다.
보통 DTO클래스에서 toString을 오버라이딩 하여 간편하게 출력하는 경우 오버라이딩을 하여 구현한다.

### 동등성의 비교 equals()

보통 기본 자료형을 사용할 때는 == 만으로 비교가 가능하다. 하지만 객체를 비교할때는 equals() 메소드를 활용하여야 한다. 객체 참조를 가진 변수로 비교를 할 때 객체의 주소값을 비교하기 때문에 객체의 값이 동일해도 false를 리턴한다. 때문에 equals() 메소드를 통하여 동일성을 검증하여 값을 비교하는 것이 좋다.<br>
<br>
클래스를 새로 만들어 비교를 할때는 equals()메소드를 오버라이딩하여야 한다. 오버라이딩을 하지않으면 기존에 equals()는 hashCode()값을 비교한다.

```java
    @Override
    public boolean equals(Object obj) {

        if (this == obj) return true;
        if (obj == null) return false;
        if (getClass() != obj.getClass()) return false;

        MemberDTO other = (MemberDTO) obj;

        if (name == null) {
            if (other.name != null) return false;
        } else (!name.equals(other.name)) return false;

        return true;
    }
```

이와 같이 비교 과정을 거친다. 하지만 String을 비교할 시에는 equals를 그대로 써서 비교했는데 이는 String 클래스는 이미 equals() 메소드를 구현해 놓았기 때문이다. <br>

### 동일성 hashCode()

기존 오버라이딩 되기전 equals()는 hashCode()값을 이용해 같은 객체인지 확인했지만 equals를 오버라이딩하여 안에 값이 같으면 true를 리턴하게하였다. 그치만 hashCode()값은 오버라이딩 하지 않으면 두 객체는 equals()는 true지만 hashCode()가 다른 상황이 생긴다. 따라서 hashCode()도 같은 값을 리턴하도록 오버라이딩을 해줘야 한다.

##### 제약사항

- 자바 애플리케이션 수행중 항상 동일한 hashCode()값을 리턴해주어야 한다. 자바를 실행할때마다는 바뀌어도 된다.
- equals() 비교가 true면 두 객체의 hashCode()값은 같아야 한다.
- equals() 비교가 false라고 hashCode값이 무조건 달라야 할 필요는 없다. 하지만 다르다면 hashTable의 성능을 향상시키는데 도움이 된다.
  <br>
  이런 복잡한 제약 때문에 직접 equals()와 hashCode()를 오버라이딩 하는것을 권장하지않고 IDE의 기능을 활용하자.
