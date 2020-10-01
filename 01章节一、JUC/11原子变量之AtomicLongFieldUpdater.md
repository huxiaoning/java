##### 原子变量之AtomicLongFieldUpdater

```java
public class Person {

    private volatile long id;

    public Person(long id) {
        this.id = id;
    }

    public long getId() {
        return id;
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                '}';
    }

    public static void main(String[] args) {
        AtomicLongFieldUpdater<Person> idUpdater = AtomicLongFieldUpdater.newUpdater(Person.class, "id");
        Person person = new Person(0);

        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        // Person{id=0}
        System.out.println(person);
        for (int i = 0; i < count; i++) {
            new Thread(() -> {
                for (; ; ) {
                    long oldId = person.getId();
                    boolean success = idUpdater.compareAndSet(person, oldId, oldId + 1L);
                    if (success) {
                        break;
                    }
                }
                latch.countDown();
            }).start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // Person{id=100000}
        System.out.println(person);
    }
}
```



好处是源对象不可修改的情况下（来自第三个Jar包里面的类）,我们也可以原子性的修改这个类的属性

