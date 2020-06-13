---
title: ArrayList数据结构
date: 2020-06-02 13:44:37
updated: 2020-06-02 13:44:37
tags: Collection, ArrayList
category: [Java, Collection]
---

`ArrayList`是`Java`集合体系中最常用的集合类之一，这一篇主要是对`ArrayList`背后的数据结构进行探讨。

<!-- more -->

### 1. `ArrayList`使用数组作为存储介质

在`ArrayList`类中有下面的成员变量：
```java
    transient Object[] elementData;
    private int size;
```
`ArrayList`定义了一个`Object[]`类型的数组 **`elementData`** 来存储被添加进`ArrayList`对象的元素。而且还定义了一个整型变量 **`size`** 用来存储对象中存储的元素的实际数量。

其中`transient`变量修饰符的作用是，当对象被序列化时，`transient`修饰的变量不会被序列化。

### 2. 添加元素

`ArrayList`提供了两个`public`修饰的`add()`方法以及一个`private`的。分开来看：

#### 2.1 在列表的末尾加入元素
```java
    public boolean add(E e) {
        modCount++;
        add(e, elementData, size);
        return true;
    }
```
当调用了这个只有一个参数的`add(e)`之后，在方法内部又调用了`add(e, elementData, size)`，参数加入了存储元素的数组变量`elementData`和元素的数量`Size`：
```java
    private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)
            elementData = grow();
        elementData[s] = e;
        size = s + 1;
    }
```
在加入这个参数前，首先判断数组时候还有空间，如果没有空间，会首先进行扩容，再添加元素。
```java
    if (s == elementData.length)
        elementData = grow();
```
空间足够的情况下，会要加入的元素放在将数组下标为`size`的位置，并将`size`修改成元素加入后的实际值：
```java
    elementData[s] = e;
    size = s + 1;
```
#### 2.2 在指定的位置插入元素

与在数组末尾插入不同，在指定位置插入元素时，需要先确定，这个位置是否有效，是否在数组的有效索引范围内：
```java
    rangeCheckForAdd(index);
```
如果要插入的位置无效，会抛出**数组下标越界异常**:
```java
    private void rangeCheckForAdd(int index) {
        if (index > size || index < 0)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
```
<!-- 之后，方法中创建了两个局部变量，作用后面会提到：
```java
    final int s;
    Object[] elementData;
``` -->
在执行插入操作之前，跟普通插入一样，需要判断`elementData`是否还有足够的空间插入元素，
```java
    if ((s = size) == (elementData = this.elementData).length)
        elementData = grow();
```
