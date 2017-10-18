---
title: Iterator的remove方法可保证从原集合中安全地删除对象
date: 2017-10-18 19:40:36
tags: 
- iterator
- remove
- ConcurrentModificationException
categories: java
---
如果对正在被迭代的集合进行结构上的改变（即对该集合使用add、remove或clear方法），那么迭代器就不再合法（并且在其后使用该迭代器将会有ConcurrentModificationException异常被抛出）。

如果使用迭代器自己的remove方法，那么这个迭代器就仍然是合法的。


```
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a","b","c","d"));
for(String s:list){
    if(s.equals("a")){
        list.remove(s);
    }
}
```
它会抛出一个ConcurrentModificationException异常。 相反下面的显示正常：
```
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a","b","c","d"));
Iterator<String> iter = list.iterator();
while(iter.hasNext()){
        String s = iter.next();
        if(s.equals("a")){
            iter.remove();
    }
}
```
.next()必须在.remove()之前调用。在一个foreach循环中，编译器会使.next()在删除元素之后被调用，因此就会抛出ConcurrentModificationException异常，你也许希望看一下ArrayList.iterator()的源代码。
http://www.cnblogs.com/softidea/p/4279574.html

Iterator 支持从源集合中安全地删除对象，只需在 Iterator 上调用 remove() 即可。这样做的好处是可以避免ConcurrentModifiedException ，这个异常顾名思意：当打开 Iterator 迭代集合时，同时又在对集合进行修改。有些集合不允许在迭代时删除或添加元素，但是调用 Iterator 的remove() 方法是个安全的做法。

ArrayList 简选两段源码
* 内部类，这个类实现了iterator接口
```
private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
```

* ArrayList 本身remove方法
```
public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
```
在内部类Itr中，有一个字段expectedModCount ，初始化时等于modCount，即当我们调用list.iterator()返回迭代器时，该字段被初始化为等于modCount。在类Itr中next/remove方法都有调用checkForComodification()方法，在该方法中检测modCount == expectedModCount，如果不相当则抛出异常ConcurrentModificationException。

使用集合的修改操作(add/remove)中，都对modCount进行了+1。
在看看刚开始提出的那段代码，在迭代过程中，执行list.remove(val)，使得modCount+1，当下一次循环时，执行 it.next()，checkForComodification方法发现modCount != expectedModCount，则抛出异常。

再来看看内部类Itr的remove()方法，在删除元素后，有这么一句expectedModCount = modCount，同步修改expectedModCount 的值。所以，如果需要在使用迭代器迭代时，删除元素，可以使用迭代器提供的remove方法。对于add操作，则在整个迭代器迭代过程中是不允许的。 其他集合(Map/Set)使用迭代器迭代也是一样。

{% blockquote %}
  勿以事小而不为。
{% endblockquote %}
