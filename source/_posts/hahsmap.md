---
title: hahsmap
tags:
  - java
abbrlink: 21283
date: 2022-04-12 23:42:03
---
# HashMap源码分析（2）
---

这几天因为疫情问题，一直待在家里，前几天还得了流感，咳嗽咽痛，提心吊胆的。昨天终于好转，也接到通知延迟开学。想着在家也要保持好状态，好好学习，就想给自己找点事干。刚好之前的一篇博客对hashmap分析的不是很透彻，刚好趁现在的时间，好好写写博客，同时也提高一下自己。学疏才浅，有不当之处，望指出。

---

java version ：jdk 1.8

---

<!-- more -->

### 1. 构造方法

惯例，先看源码
```java
    public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);
        this.loadFactor = loadFactor;
        this.threshold = tableSizeFor(initialCapacity);
    }

    public HashMap(int initialCapacity) {
        this(initialCapacity, DEFAULT_LOAD_FACTOR);
    }

    public HashMap() {
        this.loadFactor = DEFAULT_LOAD_FACTOR; // all other fields defaulted
    }

    public HashMap(Map<? extends K, ? extends V> m) {
        this.loadFactor = DEFAULT_LOAD_FACTOR;
        putMapEntries(m, false);
    }
```
在分析这四个构造方法之前，还需要了解hashmap中的几个属性，  
- loadFactor : 装载因子  
- threshold  : 扩容阈值，也就是说当hashmap中的数据个数超过这个阈值，就扩容  

hashmap前三个构造方法都是初始化参数，在这些构造方法中并没有初始化节点数组，二十初始化了装载因子和扩容阈值这两个参数，在`put`方法中根据扩容阈值来初始化数组。  
```python
if ((tab = table) == null || (n = tab.length) == 0)
    n = (tab = resize()).length;
```
由此可以看出初始化数组使用的是`resize()`方法。
```java
    final Node<K,V>[] resize() {
        // 这段代码中关于扩容的分支被我删除了，留下的都是初始化数组的分支 
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;// 旧容量，初始化数组时为0
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldThr > 0) // 是否指定了数组大小
            newCap = oldThr;
        else { //使用默认参数
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) { // 判断数组大小是否越界
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        return newTab;
    }
```
构造方法一中还是用了另一个方法` this.threshold = tableSizeFor(initialCapacity)`，也就是说既指定了初始容量又指定了装载因子时，使用这个方法确定扩容阈值。
```java
    static final int tableSizeFor(int cap) {
        // Integer.numberOfLeadingZeros(cap - 1)`方法返回32位int类型数高位的0的个数。这里的源码巧妙地使用了二分法使用了四次右移操作，找出了高位0个数
        int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```

这个方法的作用是找到大于等于 `initialCapacity` 的最小的 2 的幂。假设输入的值是10 

```
cap = 10,cap - 1 = 9,高位的0有28个
n = -1 >>> 28
源码 10000000000000000000000000000001
反码 11111111111111111111111111111110
补码 11111111111111111111111111111111
右移 00000000000000000000000000001111
n = 8 + 4 + 2 + 1 = 15
n + 1 = 16
```

分析以上源码我们知道，如果默认不传入参数，那么hashmap初始化为大小为16，装载因子为0.75的数组。如果传入的有初始容量，那么数组大小会被初始化为大于等于给定值的最小的2的幂。之所以是2的幂，在之后求hash值时会用到。

### 2. put方法
惯例，先看源码
```java
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            // 数组未初始化则调用resize初始化
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

具体的代码执行流程上一篇博客里已经分析了，这里只分析几点。  
- hash方法  
```java
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```  
hashmap的`hash`方法会首先调用key的`hashCode()`方法，得到一个int类型的值，然后将其右移16位，并于原来的值按位异或运算。这么做的原因是，为了有更好的散列性同时又有较好的性能。因为计算数组下标时（`p = tab[i = (n - 1) & hash]`）是仅有hash值的低位参与运算，而这个方法将原来的值高位与低位异或之后放到低位，将高位也参与到运算中，所以具有较好的散列性。同时只是进行异或运算，也减小了系统开销。


- 插入null键值对  
从`hash`方法中可以看到，当插入的key为null时，方法返回0，而`p = tab[i = (n - 1) & hash]`计算下表使用了与方法，所以最终得到的下标为0，也就是说插入到数组的第一个位置。

### 3. 扩容操作
惯例，先看源码
```java
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                // 扩容为原来大小的两倍
                newThr = oldThr << 1; // double threshold
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
```
因为hashmap扩容之后大小为原来的二倍，旧数组中的元素在新数组中会重新计算下标。计算的方法是数组容量的值减一后与元素key的hash值做与运算。因此对于每个元素会有两种情况。
- 假设数组容量为16
```
cap - 1 = 15 = 1111
key1  00000000000000000000000000011001
cap-1 00000000000000000000000000001111
hash1 00000000000000000000000000001001 = 9

key1  00000000000000000000000000001001
cap-1 00000000000000000000000000001111
hash1 00000000000000000000000000001001 = 9
```
- 扩容之后数组容量为32
```
cap - 1 = 31 = 11111
key1  00000000000000000000000000011001
cap-1 00000000000000000000000000011111
hash1 00000000000000000000000000011001 = 25

key1  00000000000000000000000000001001
cap-1 00000000000000000000000000011111
hash1 00000000000000000000000000001001 = 9
```
也就是说，扩容后每个元素，要么在原来的位置，要么移动到原来的位置加上oldcap所在的位置。
### 4. hashmap线程不安全的原因

- 因为hashmap的put方法不是同步的，假如有两个线程同时put两个hash值相同的数据，那么不管是头插法还是尾插法，最终一定会有一个数据丢失。
- jdk1.8之前，hahsmap插入元素使用头插法，多线程操作时，会出现循环链表，此时get数据时会造成死循环。

