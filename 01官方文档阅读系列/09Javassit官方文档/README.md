### Javassist官方文档阅读笔记

[源文地址](https://www.javassist.org)

`Javassist`(`Java`编程助手)使用`Java`字节码操作变得简单。

它是一个在`Java`中编辑字节码的类库。

它使`Java`程序可以在运行时定义一个新类、`JVM`加载类文件时修改类文件。

与其他类似的字节码编辑器不同，`Javassist`提供了两个级别的`API`：源代码级和字节码级。

如果用户使用源代码级`API`，那么他们可以在不了解`Java`字节码规范的情况下编辑类文件。

整个`API`只使用`Java`语言的词汇表进行设计。

您甚至可以以源文本的形式指定插入的字节码；`Javassist`动态编辑它。

另一方面，字节码级`API`允许用户像其他编辑器一样直接编辑类文件。

#### 有效性

```xml
<dependency>
    <groupId>org.javassist</groupId>
    <artifactId>javassist</artifactId>
    <version>3.27.0-GA</version>
</dependency>
```



[教程地址](https://www.javassist.org/tutorial/tutorial.html)

