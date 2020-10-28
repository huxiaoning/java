##### 原子变量之AtomicLongArray

```java
public class Person {

    private AtomicLongArray arr = new AtomicLongArray(new long[]{0L, 1L, 2L, 3L, 4L, 5L});

    public long get() {
        return arr.get(0);
    }

    public void incr() {
        for (; ; ) {
            long oldValue = arr.get(0);
            boolean success = arr.compareAndSet(0, oldValue, oldValue + 1L);
            if (success) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        Person person = new Person();
        // 输出初始值 0
        System.out.println(person.get());
        for (int i = 0; i < count; i++) {
            new Thread(() -> {
                person.incr();
                latch.countDown();
            }).start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 输出初始值 100000
        System.out.println(person.get());
    }
}
```
