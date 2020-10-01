##### 原子变量之AtomicReference

```java
public class Person {

    private int id;

    private AtomicReference<Person> teacher = new AtomicReference();

    public Person(int id) {
        this.id = id;
    }

    public Person getTeacher() {
        return teacher.get();
    }

    public void setTeacher(Person teacher) {
        this.teacher.set(teacher);
    }

    public void changeTeacher() {
        for (; ; ) {
            Person oldTeacher = this.teacher.get();
            Person newTeacher = new Person(oldTeacher.id + 1);
            if (this.teacher.compareAndSet(oldTeacher, newTeacher)) {
                break;
            }
        }

    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                '}';
    }

    public static void main(String[] args) {
        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        final Person person = new Person(-1);
        person.setTeacher(new Person(0));
        // Person{id=0}
        System.out.println(person.getTeacher());
        for (int i = 0; i < count; i++) {
            new Thread(() -> {
                person.changeTeacher();
                latch.countDown();
            }).start();
        }

        try {
            latch.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        // Person{id=100000}
        System.out.println(person.getTeacher());
    }
}
```



注意：`AtomicReference`存在ABA问题,推荐使用`AtomicStampedReference`