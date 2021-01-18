# Thread Pool & Future Interface

### Thread Pool

병렬 작업 처리가 많아지면 쓰레드 개수가 증가하게되고 쓰레드의 생성과 스케줄링으로 인하여 CPU가 바빠져 메모리 사용량이 늘어난다. 따라서 애플리케이션의 성능이 저하된다. 갑작스런 병렬 작업의 폭증으로 인한 쓰레드의 폭증을 막으려면 Thread Pool이 필요하다. Thread Pool은 미리 쓰레드를 만들어 놓거나 개수를 제한함으로써 작업큐를 이용해 급작스런 성능 저하를 막을 수 있다.

- **Thread Pool 생성**
  `newCachedThreadPool()`은 초기 쓰레드와 코어 스레드 수가 0이지만 최대 쓰레드 수는 Integer.MAX_VALUE값을 지원하기에 최대 생성 개수는 운영체제에 따라 다르다. 또한 작업이 끊나고 60초 동안 다른 작업을 하지 않으면 스레드를 Thread Pool 에서 제거한다.
  <br>
  `newFixedThreadPool(int nTreads)`은 초기 스레드 수는 0이며 최대 스레드 수를 nTreads로 제한한다. 이 스레드 풀은 스레드가 아무 작업을 하지 않더라도 생성한 스레드 개수를 줄이지 않는다.
  <br>

아래 코드는 CPU 코어의 수만큼 최대 스레드를 사용하는 스레드 풀이다.

```java
ExecutorService executor = Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());
```

- **종료**
  스레드풀의 스레드는 기본적으로 데몬 스레드가 아니기 때문에 main 스레드가 종료되더라도 작업을 처리하기 우해 계속 실행 상태로 남아있다. 그래서 애플리케이션을 종료시키기 위해서는 스레드를 모두 종료시켜 줘야하는데 이 때 `shutdown()`메소드는 스레드의 작업을 모두 마무리 하고 종료시키고 `shutdownNow()`메소드는 작업 종료와 관련없이 즉시 모든 스레드를 종료시킨다.
  <br>
- **Thread Pool에 작업 요청**
  하나의 작업은 Runnable 또는 Callable 구현 클래스로 표현한다. 차이점은 Runnable은 반환값이 없는 반면 Callable은 반환값이 있다.

```java
Runnable task = new Runnable() {
    @Override
    public void run() {
        ...
    }
}
```

```java
Callable<T> task = new Callable<T>() {
    @Override
    public T call() throws Exception {
        ...
    }
}
```

이렇게 만들어진 구현체를 `void execute(Runnable command)`와 `Future<T> submit(Callable<V> task)`로 넘겨주면 스레드 풀에서 스레드를 할당하여 작업을 처리해준다. execute 메소드는 작업 처리의 결과를 받지 못하는 반면 submit은 반환 결과를 알려준다. execute는 스레드는 예외가 발생하면 스레드를 제거하고 다음 작업에 새로운 스레드를 만든다. 하지만 submit은 예외처리에 대해서 스레드를 따로 종료시키지 않기때문에 생성 오버헤드 측면에서 submit()을 사용하는 것이 좋다.

### Future Interface

ExecutorService의 submit() 메소드로 작업을 요청하면 즉시 Future 객체를 리턴한다. Future 객체는 작업 결과가 아니라 작어빙 완료될 때까지 기다렸다가(블로킹) 최종 결과를 얻을 수 있다. Future 객체는 get() 메소들르 호출하면 작업을 완료될 때까지 블로킹 되었다가 작업을 완료하면 처리결과를 리턴하는 것이다. 이것이 블로킹을 사용하는 작업완료 통보 방식이다.
<br>
여기서 get()메소드를 호출하면 작업을 완료하기 전까지 어떠한 작업을 할 수 없다. 그렇기 떄문에 get()메소드를 호출하는 스레드는 새로운 스레드이거나 스레드풀의 또 다른 스레드가 되어야 한다.

```java
// 새로운 thread를 이용
new Thread(new Runnable() {
    @Override
    public void run() {
        try {
            future.get();
        } catch (Exception e) {

        }
    }
}).start();

// thread pool을 이용
executorService.submit(new Runnable() {
    @Overrid
    public void run() {
        try {
            future.get();
        } catch (Exception e) {

        }
    }
})
```
