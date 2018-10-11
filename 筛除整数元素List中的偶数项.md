#### 筛除整数元素List中的偶数项

题目源于《数据结构与算法分析——Java语言描述》3.3.4的例题。假如一个包含元素6、5、1、4、2的list，经过该算法筛选后剩下5、1。

算法传入一个List，以下分析基于List的两个实现ArrayList和LinkedList。

- ##### Answer 1

```java
public static void removeEvenVerl(List<Integer> lst) {
    int i = 0;
    while (i < lst.size()) {
        if (lst.get(i) % 2 == 0)
            lst.remove(i);
        else
            i++;
    }
}
```

上面代码使用递增角标来遍历list。这种角标遍历让我们想到数组，即ArrayList实现。但ArrayList的remove方法效率不高（因为底层是数组实现，被删除元素之后所有元素都要前移一位，删除元素的效率取决于元素在数组中所在的位置），而对于LinkedList，get方法的效率不高（LinkedList底层是链表实现，需要一个一个遍历，寻找元素也是取决于在链表中的位置），并且remove方法由于需要先找到元素位置再删除，所以效率也没有如正经链表一般想象中的高。所以这个算法对于两种List花费的都是二次时间。

- ##### Answer 2

```java
public static void removeEvenVerl(List<Integer> lst) {
    for (Integer x : lst)
        if (x % 2 == 0)
            lst.remove(x);
}
```

上面代码使用增强for循环来遍历list，避免了使用get的低效率问题。但是有一个十分严重的问题：增强for循环其实是使用List的迭代器实现的，而迭代器有一个基本法则是**如果对正在被迭代的集合进行结构上的改变，即添加、删除等操作，那么再次使用迭代器将会抛出ConcurrentModificationException异常**。上面的算法正是违背了这一点，在已经启动的迭代器后又想要删除List的元素，将会抛出异常。

- ##### Answer 3

```java
public static void removeEvenVerl(List<Integer> lst) {
    Iterator<Integer> itr = lst.iterator();
    while (itr.hasNext())
        if (itr.next() % 2 == 0)
            itr.remove();
}
```

上面代码自定义了一个Iterator迭代器，用hasNext作为条件来遍历。相对于Answer2，删除元素时使用迭代器的remove方法而不是使用List的remove方法，就不会出现异常。remove方法对于LinkedList花费常数时间，整个算法花费的是线性时间。而对于ArrayList依然由于remove平均需要移动半数元素，花费二次时间。