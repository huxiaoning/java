##### 原子变量之AtomicReferenceArray

```java
public class Person {

    private AtomicReferenceArray<Teacher> arr = new AtomicReferenceArray(
            new Teacher[]{new Teacher(0), new Teacher(1), new Teacher(2), new Teacher(3), new Teacher(4),
                    new Teacher(5)});

    public Teacher get() {
        return arr.get(0);
    }

    public void incr() {
        for (; ; ) {
            Teacher oldValue = arr.get(0);
            Teacher newValue = new Teacher(oldValue.getId() + 1);
            boolean success = arr.compareAndSet(0, oldValue, newValue);
            if (success) {
                break;
            }
        }
    }

    public static void main(String[] args) {
        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        Person person = new Person();
        // Teacher{id=0}
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
        // Teacher{id=100000}
        System.out.println(person.get());
    }
}
```

引用的`Teacher`类源码如下:

```java
public class Teacher {

    private int id;

    public Teacher(int id) {
        this.id = id;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "id=" + id +
                '}';
    }
}
```

