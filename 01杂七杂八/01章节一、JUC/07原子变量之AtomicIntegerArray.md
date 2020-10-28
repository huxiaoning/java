##### 原子变量之AtomicIntegerArray

```java
public class Person {

    private AtomicIntegerArray arr = new AtomicIntegerArray(new int[]{0, 1, 2, 3, 4, 5});

    public int get() {
        return arr.get(0);
    }

    public void incr() {
        for (; ; ) {
            int oldValue = arr.get(0);
            boolean success = arr.compareAndSet(0, oldValue, oldValue + 1);
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



`AtomicIntegerArray`可以原子性的修改数组中某个索引处的元素。



当然，你也可以直接使用`AtomicIntegerArray`的API来自增某个元素

```java
    public void incr() {
        arr.incrementAndGet(0);
    }
```

