##### 原子变量之AtomicInteger

```java
public class Person {

    private AtomicInteger id = new AtomicInteger();

    public int getId() {
        return id.get();
    }

    public void incrId() {
        for (; ; ) {
            int oldValue = id.get();
            boolean success = id.compareAndSet(oldValue, oldValue + 1);
            if (success) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        final Person person = new Person();
        for (int i = 0; i < count; i++) {
            new Thread(() -> {
                person.incrId();
                latch.countDown();
            }).start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(person.getId());
    }
}
```

当然，`incrId()`方法也可以写成下面这样：

```java
    public void incrId() {
        id.getAndIncrement();
    }
```

或者这样：

```java
    public void incrId() {
        id.incrementAndGet();
    }
```

为什么要写这个样子？只是为了感受一下自旋的代码风格

```java
        for (; ; ) {
            int oldValue = id.get();
            boolean success = id.compareAndSet(oldValue, oldValue + 1);
            if (success) {
                break;
            }
        }
```

