# 1.数组

## 1.1 Array类

```java
java.lang.Array
```

Array类提供了动态创建和访问Java数组的静态方法

### 1.1.1 Java中数组的一些规则：

- 数组是一种数据结构，用来存储同一类型值的集合
- 一旦创建了数组，就不能改变它的长度
- 数组元素的下标从0开始
- 默认初始值
  - 数字数组默认值初始值为0
  - boolean数组默认初始值为false
  - 对象数组默认初始值为null

### 1.1.2 Java中数组初始化

```java
//数组的声明与初始化
int[] arr;	//声明一个对象变量arr，但引用指向的是null
int[] arr = new int[3];	//声明一个长度为3的数组，初始值默认为0,0,0
//也可以声明为一个匿名数组：
int[] arr = new int[]{1,4,7};	//匿名数组会根据大括号中的初始值个数设置数组长度
//匿名数组的简写方式：
int[] arr = {1,4,7};
```

### 1.1.3 用for each循环访问数组中的元素

在Java中，可以遍历数组下标来访问数组

```java
int[] arr = new int[]{1,4,7};
for(int index = 0;index<arr.length;index++)
{
	System.out.println(arr[index]);
}
```

Java中，也可以通过遍历元素来访问数组

```java
int arr = new int[]{1,4,7};
for(int num : arr)
{
	System.out.println(num);
}
```

### 1.1.4 数组的一些方法

```java
//数组.length 获取数组长度
int[] arr = new int[3];
System.out.println(arr.length);

//数组复制

/*
	在Java中，允许将一个数组变量拷贝到另一个数组变量。
	这时，两个数组变量将引用同一个数组
*/
int[] arr1 = {10,20,30,40,50};
int[] arr2 = arr1;

/*
	第一个参数：源数组
	第二个参数：源数组的起始下标
    第三个参数：目标数组
    第四个参数：目标数组的起始下标
    第五个参数：要复制的元素个数
*/
int[] arr1 = {10,20,30,40,50};
int[] arr2 = new int[6];
System.arraycopy(a,1,b,0,4);
```

## 1.2 Arrays类

```java
java.util.Arrays
```

该类为数组的工具类。该类包含用于操作数组的各种方法。

```java
/*
	public static String toString(类型[] arr)
	作用:将数组以字符串形式返回
		- 在Arrays中，该方法使用重载来完成转化字符串的操作
*/
int[] arr = {10,20,30,49};
String str = Arrays.toString(arr);
System.out.println(str);

/*
	public static <T> T[] copyOf(T[] original, int newLength)	
	作用:数组复制
	第一个参数：源数组
	第二个参数：目标数组的长度(元素个数)
	-- 若目标数组>源数组长度，则末尾补默认值
	-- 若目标数组<源数组长度，则将末尾的裁掉
	-- 该方法适用于给现有数组扩容
	   由于数组长度不可变，因此扩容时是创建一个新数组
	   并把源数组数据复制进去
*/
int[] arr1 = {10,20,30,40,50};
int[] arr2 = Arrays.copyOf(a,6);	//10,20,30,40,50,0
int[] arr2 = Arrays.copyOf(a,4);	//10,20,30,40

/*
	public static void sort(int[] a)	
	作用:数组排序
		- 升序排列，从小到大
		- 该方法使用了优化的快速排序算法。
		- 只能排序数字类型
*/
int[] arr = {10,20,30,40};
Arrays.sort(arr);

/*
	public static <T> void sort(T[] a, Comparator<? super T> c)
	作用:指定一个比较器规则，将数组a中的元素两两传入，进行比较
		- 该方法不能排序数字类型，但可以使用包装类定义数组
		- 比较器规则默认以下情况为升序
			-- 如果左边数据大于右边数据 返回正整数
			-- 如果左边数据小于右边数据 返回负整数
			-- 如果左边数据等于右边数据 返回0
		- 如果需要降序，则相反
*/
Integer[] arr = {34,12,42,23};
System.out.println(Arrays.toString(arr));
Arrays.sort(arr, new Comparator<Integer>() {
            
	public int compare(Integer o1, Integer o2) {
		return o1-o2;	//升序
	}
});
System.out.println(Arrays.toString(arr));		


/*
	public static int binarySearch(int[] a, int key)
	作用:使用二分查找算法，在a数组中查找key的位置
		- 使用二分查找时，必须先将数组进行排序，否则会出现bug
		- 如果数组中有key这个元素，则返回值为排序后key所在的下标
		- 如果数组中没有key这个元素，则返回-(该元素应该在的下标 +1)
*/
int[] arr1 = {10,3,43,1,20,23,56,546};
System.out.println(Arrays.toString(arr1));
int index = Arrays.binarySearch(arr1,43);
System.out.println(index);

/*
	Arrays提供了一个静态方法asList,可以将一个数组转换为一个List集合
	注意：
		- 对该集合的操作就是对原数组对应的操作
		- 会改变数组长度的操作都是不支持的，因为数组是定长的。
		  会抛出异常:java.lang.UnsupportedOperationException(不支持的操作异常)
		  
		  可以使用一个新集合来将其装进去再进行操作
		  所有的集合都支持一个参数为Collection的构造器，作用是在创建当前集合的同时
          包含给定集合中的所有元素
          List<String> list2 = new ArrayList<>(list);
*/
List<String> list = Arrays.asList(array);
```



# 2.集合

集合是Java中存储对象数据的一种容器

集合的特点：

- 集合的大小不固定，启动后可以动态变化，类型也可以选择不固定。
- 集合非常适合做元素的增删操作
- 集合中只能存储引用类型，并且存储的是元素的地址
- 集合均支持泛型来限制其类型

## 2.1 Collection类

```java
java.util.Collection	//Java中的单列集合，为接口
```

![Collection特点](C:\WorkSpace\笔记\Java笔记\Java数组与集合\assets\Collection特点.png)

### 2.1.1 Collection类的一些方法

```java
Collection c = new ArrayList();
/*
	boolean add(E e)
	向当前集合添加一个元素，元素成功添加后返回true。
*/
c.add("one");
c.add("two");
c.add("three");

/*
	int size()
    返回当前集合的元素个数
*/
int size = c.size();

/*
	boolean isEmpty()
	判断当前集合是否为一个空集
*/
boolean isEmpty = c.isEmpty();
System.out.println("isEmpty:"+isEmpty);

/*
	void clear()
	清空集合
*/
c.clear();

/*
	boolean contains(Object o)
	判断当前集合是否包含给定元素。判断依据是该元素是否与集合现有元素存在equals
	方法比较为true的情况，存在则认为包含。
*/
 boolean contains = c.contains(p);

/*
	删除元素也是删除与给定元素equals比较为true的元素
	对于List而言，重复元素只会删除第一次出现的元素。
*/
c.remove(p);

/*
	Collection上定义了一个方法:toArray可以将当前集合转换为一个数组
*/
Collection<String> c = new ArrayList<>();
	c.add("one");
    c.add("two");
    c.add("three");
    c.add("four");
    c.add("five");
System.out.println(c);
Object[] array = c.toArray();

/*
	boolean addAll(Collection c)
	将给定集合中所有元素添加到当前集合中。添加后当前集合发生了改变则返回true.
*/
Collection<Integer> collection = new ArrayList<Integer>();
collection.add(18);
collection.add(20);
collection.add(40);
collection.add(20);
System.out.println(collection);

Collection<Integer> collection1 = new ArrayList<>();
collection1.add(11);
collection1.add(1);
collection.addAll(collection1);
System.out.println(collection);

/*
	boolean containsAll(Collection c)
	判断当前集合是否包含给定集合中的所有元素，全部包含则返回true。
*/
boolean re = collection.containsAll(collection1);
System.out.println(re);

/*
	取交集，仅保留当前集合中与给定集合的共有元素。
*/
c2.retainAll(c3);
/*
	删交际，将当前集合中与给定集合的共有元素删除
*/
c2.removeAll(c3);
```

### 2.1.2 遍历集合

遍历集合有两种方式：

- 迭代器
- for-each循环

#### foreach循环

```java
/*
	使用新循环遍历时，如果集合指定了泛型，
	那么接收元素时可以直接用对应的类型接收元素。
*/
Collection<Integer> collection = new ArrayList<Integer>();
	collection.add(18);
    collection.add(20);
	collection.add(40);
    collection.add(20);
for(Integer i : collection)
{
    System.out.printlin(i);
}
```

#### 迭代器

````java
java.util.Iterator<E>	//迭代器接口
/*
	不同的集合都提供了一个迭代器实现类用于遍历当前集合元素。迭代器接口上规定了遍历集合的
 	相关方法，使用迭代器遍历集合的步骤遵循:问->取->删的步骤进行。其中删除不是必要操作。
*/
````

> 迭代器只能遍历集合，不能遍历数组

```java
/*
	boolean hasNext()
	使用迭代器询问是否还有下一个元素可以迭代。迭代器默认开始的位置是集合第一个
	元素之前，因此第一次调用就是询问是否有第一个，依此类推。

	E next()
	使迭代器向后移动并获取下一个元素。
	
	使用迭代器遍历集合元素的过程中不要通过集合的add,
    remove这样的操作增删元素，否则迭代器会抛出异常:
    java.util.ConcurrentModificationException
    
    迭代器提供了一个remove方法，删除迭代器当前位置的集合元素
    迭代器也支持泛型，指定时要与其遍历的集合指定的泛型一致
*/
Iterator<Integer> it = collection.iterator();
while(it.hasNext())
{
	Integer integer = it.next();
	System.out.println(integer);
}
```

### 2.1.3 List集合

```java
java.util.List	
/*
	List集合:List接口继承自Collection，是可以存放重复元素且有序的集合。
	常用实现类:
		- java.util.ArrayList:内部使用数组实现，查询性能更好。
		- java.util.LinkedList:内部使用链表实现，增删性能更好，首尾增删性能最佳
		- 在对性能没有特别苛刻的情况下通常使用ArrayList即可。
*/
```

#### List集合的一些方法

```java
List<String> list = new ArrayList<>();
	list.add("one");
    list.add("two");
    list.add("three");
    list.add("four");
    list.add("five");

/*
	E get(int index)
	获取指定下标对应的元素，下标从0开始
*/
String e = list.get(2);
System.out.println(e);

/*
	E set(int index,E e)
	将给定元素设置到指定位置，返回值为该位置原有的元素。
	替换元素操作
*/
String old = list.set(1,"six");
System.out.println(list);
System.out.println("被替换的元素是:"+old);

/*
	void add(int index,E e)
	将给定元素插入到指定位置，为重载方法
*/
list.add(2,"six");
System.out.println(list);

/*
	E remove(int index)
	删除并返回指定位置上的元素
*/
String e = list.remove(1);
System.out.println(list);
System.out.println("被删除的元素:"+e);

/*
	List subList(int start,int end)
	获取当前集合中指定范围内的子集。两个参数为开始与结束的下标(含头不含尾)
	注意：对子集进行相关操作，会影响到原集合
*/
List<Integer> subList = list.subList(3,8);
```



## 2.2 Map类

```java
java.util.Map
```

Map接口
 - Map称为查找表，体现的结构是一个多行两列的表格。其中左列称为key,右列称为value.
 - Map总是根据key获取对应的value

常用实现类:

- java.util.HashMap:称为散列表，哈希表。是使用散列算法实现的Map，当今查询速度最快的数据结构
- java.util.TreeMap:使用二叉树算法实现的Map

### 2.2.1 map常用方法

```java
/*
	Map的key和value可以分别指定不同类型
*/
Map<String,Integer> map = new HashMap<>();

/*
	V put(K k,V v)
	将一组键值对存入Map中。
	Map要求key不允许重复。
	如果put方法存入的键值对中，key不存在时，则直接将key-value存入，返回值为null
	如果key存在，则是替换value操作，此时返回值为被替换的value
*/
map.put("数学",98);
map.put("英语",97);
map.put("物理",96);
map.put("化学",99);
System.out.println(map);

value = map.put("物理",60);
System.out.println(value);

/*
	V get(Object k)
	根据给定的key获取对应的value，如果给定的key不存在，则返回值为null
*/
value = map.get("化学");
System.out.println(value);

/*
	可判断Map是否包含给定的key或value
	boolean containsValue(Object value)
	boolean containsKey(Object key)
*/
boolean ck = map.containsKey("数学");
boolean cv = map.containsValue(99);

/*
	V remove(Object key)
	删除给定的key所对应的键值对，返回值为这个key对应的value
	如果没有key，会返回null
*/
value = map.remove("语文");
System.out.println(map);
System.out.println(value);
```

### 2.2.2 map遍历

```java
Map<String,Integer> map = new HashMap<>();
	map.put("语文",99);
    map.put("数学",98);
    map.put("英语",97);
    map.put("物理",96);
    map.put("化学",99);
    System.out.println(map);
/*
	Set keySet()
    将当前Map中所有的key以一个Set集合形式返回，遍历该集合等同于遍历所有的key
*/
Set<String> keySet = map.keySet();
for(String key : keySet)
{
	System.out.println("key:"+key);
}

/*
	Set entrySet()
	将当前Map中每一组键值对以一个Entry实例形式表示，并存入Set集合后返回
	java.util.Map.Entry的每一个实例用于表示Map中的一组键值对，其中方法:
	K getKey():获取对应的key
	V getValue():获取对应的value
*/
Set<Map.Entry<String,Integer>> entrySet = map.entrySet();
for(Map.Entry<String,Integer> e : entrySet)
{
	String key = e.getKey();
	Integer value = e.getValue();
	System.out.println(key+":"+value);
}

/*
	Collection values()
	将当前Map中所有的value以一个集合形式返回
*/
Collection<Integer> values = map.values();
for(Integer value : values)
{
	System.out.println("value:"+value);
}
```

