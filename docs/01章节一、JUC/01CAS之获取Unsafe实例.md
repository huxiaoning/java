###### 1 尝试下直接获取

```java
    public static void main(String[] args) {
        Unsafe unsafe = Unsafe.getUnsafe();
        // java.lang.SecurityException: Unsafe
        System.out.println(unsafe);
    }
```

抛异常了，既然直接获取不行，那就用反射来获取

##### 2 用反射来获取Unsafe实例

```java
public class UnsafeHolder {

    private static Unsafe unsafe;

    static {
        try {
            Field theUnsafe = Unsafe.class.getDeclaredField("theUnsafe");
            theUnsafe.setAccessible(true);
            unsafe = (Unsafe) theUnsafe.get(null);
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        }
    }

    public static Unsafe get() {
        return unsafe;
    }
}
```

