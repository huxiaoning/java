##### 1 使用Unsafe无锁操作类变量

```java
public class Client {

    private static volatile int x = 0;

    public static void main(String[] args) {
        Unsafe unsafe = UnsafeHolder.get();
        try {
            long offset = unsafe.staticFieldOffset(Client.class.getDeclaredField("x"));
            unsafe.compareAndSwapInt(Client.class, offset, 0, 1);
            // 输出 1
            System.out.println(x);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
    }
}
```

##### 2 使用Unsafe无锁操作实例变量

```java
public class Client {

    private volatile int x = 0;

    public int getX() {
        return x;
    }

    public static void main(String[] args) {
        Unsafe unsafe = UnsafeHolder.get();
        Client client = new Client();
        try {
            long offset = unsafe.objectFieldOffset(Client.class.getDeclaredField("x"));
            unsafe.compareAndSwapInt(client, offset, 0, 1);
            // 输出 1
            System.out.println(client.getX());
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
    }
}
```

