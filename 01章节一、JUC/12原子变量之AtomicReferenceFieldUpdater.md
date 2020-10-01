##### 原子变量之AtomicReferenceFieldUpdater

```java
public class Person {

    private volatile Teacher teacher;

    public Person(Teacher teacher) {
        this.teacher = teacher;
    }

    public Teacher getTeacher() {
        return teacher;
    }

    public void setTeacher(Teacher teacher) {
        this.teacher = teacher;
    }

    @Override
    public String toString() {
        return "Person{" +
                "teacher=" + teacher +
                '}';
    }

    public static void main(String[] args) {
        AtomicReferenceFieldUpdater<Person, Teacher> idUpdater = AtomicReferenceFieldUpdater.newUpdater(Person.class,
                Teacher.class, "teacher");
        Person person = new Person(new Teacher(0));

        int count = 100000;
        CountDownLatch latch = new CountDownLatch(count);
        // Person{teacher=Teacher{id=0}}
        System.out.println(person);
        for (int i = 0; i < count; i++) {
            new Thread(() -> {
                for (; ; ) {
                    Teacher oldTeacher = person.getTeacher();
                    Teacher newTeacher = new Teacher(oldTeacher.getId() + 1);
                    boolean success = idUpdater.compareAndSet(person, oldTeacher, newTeacher);
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
        // Person{teacher=Teacher{id=100000}}
        System.out.println(person);
    }
}
```



好处是源对象不可修改的情况下（来自第三个Jar包里面的类）,我们也可以原子性的修改这个类的属性



`Teacher`类的代码如下:

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

