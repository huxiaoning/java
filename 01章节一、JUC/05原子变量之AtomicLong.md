##### 原子变量之AtomicBoolean

```java
public class Person {

    private AtomicLong id = new AtomicLong();

    public long getId() {
        return id.get();
    }

    public void incrId() {
        for (; ; ) {
            long oldValue = id.get();
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
        // 输出：100000
        System.out.println(person.getId());
    }
}
```
