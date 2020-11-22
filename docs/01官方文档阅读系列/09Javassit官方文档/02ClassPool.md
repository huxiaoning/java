### `ClassPool`

一个`ClassPool`对象是`CtClass`对象的容器（这个容里面只放`CtClass`对象）。一个`CtClass`对象一旦被创建了，它将永远被记录在`ClassPool`中。这是因为编译器在编译引用由`CtClass`表示的类的源代码时可能需要访问`CtClass`对象。