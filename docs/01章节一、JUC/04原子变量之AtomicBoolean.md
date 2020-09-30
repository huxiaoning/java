##### 原子变量之AtomicBoolean

```java
public class Person {

    private AtomicBoolean flag = new AtomicBoolean(false);

    public boolean getFlag() {
        return flag.get();
    }

    public void toggle() {
        for (; ; ) {
            boolean oldValue = flag.get();
            boolean success = flag.compareAndSet(oldValue, !oldValue);
            if (success) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        final Person person = new Person();
        // 输出默认值 false
        System.out.println(person.getFlag());
        for (int i = 0; i < count; i++) {
            new Thread(() -> {
                person.toggle();
                latch.countDown();
            }).start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // 输出结果false
        System.out.println(person.getFlag());
    }
}
```
