---
title: java关键字简单描述
date: 2017-10-18 20:12:14
tags:
- keywords
categories: java
---
## transient
实现了Serilizable接口序列化时，有些属性需要序列化，而其他属性不需要被序列化，就可以加上transient关键字。换句话说，这个字段的生命周期仅存于调用者的内存中而不会写到磁盘里持久化。

transient关键字只能修饰变量，而不能修饰方法和类

* Tips：
**1.对象的序列化可以通过实现两种接口来实现。**
若实现的是Serializable接口，则所有的序列化将会自动进行。
若实现的是Externalizable接口，则没有任何东西可以自动序列化，需要在writeExternal方法中进行手工指定所要序列化的变量，这与是否被transient修饰无关。
**2.一个静态变量不管是否被transient修饰，均不能被序列化**


