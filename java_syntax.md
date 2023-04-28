Import java.util.scanner;

import java.util.HashMap;

import java.util.Map;

import java.util.HashSet;

import java.util.Set;

 

 

选择数：

​                                （配合快速幂）

 

Math.pow(x, y)

 

二分查找: l <= r和 l = m不能同时出现

 

 

**常用数据结构**

#### MAP

```java
Map<T, T> map = new HashMap<>();

map.containsKey(key)

map.put(key, value)

map.get(key)

for (T key : map.keySet()){

       map.get(key);

}

for (T value: map.values()){

}
```



 

#### **HashSet**

```java
Set<T> set = new HashSet<>()

set.contains;

set.add();
```



 

 

#### **Deque**

```java
Deque <T> stack = new LinkedList<>();
```

 

 

##### 单调栈：

找到前一个大于/小于本元素的元素

根据次大值求最大值

```java
While（!stack.isEmpty() && peek < cur）{
       Stack.pop();
}
```





 

#### **Collection**

##### **1.Collection.sort()**

```java
new Comparator<T>(){

       public int compare(T o1, T o2){
       return
}
}
```





同样适用于**treeMap** (底层为红黑树)**, priority queue**

```java
TreeMap<K, V> map = new TreeMap<>();

Queue<Integer> queue = new PriorityQueue<>((v1, v2) -> v1 – v2);

 

Collections.sort(list, new Comparator<Map.Entry<Character,Integer>>(){
    public int compare(Map.Entry<Character, Integer> o1, Map.Entry<Character, Integer> o2){
        return o1.getValue().
            compareTo(o2.getValue()) == 0 
            ? (int)o1.getKey() - (int)o2.getKey() 
            : o1.getValue().compareTo(o2.getValue());
    }
});
```





 

##### **2. Collection.reverse()**

 

#### Priority queue:

``` java
Queue<Integer> queue = new PriorityQueue<>((v1, v2) -> v1 – v2);
```



 

#### **String**

```java
s.replaceAll(“ ”, “”)
```





#### **Character**

```java 
Character.isDigit(Ch);
Character.isDigitOrCharacter(ch);
Character.toLowerCase(ch);
```



 

#### **List**

```java
List<int[]>

ans.toArray(new int[ans.size()][])

List<Integer> list = new ArrayList<>(Arrays.asList(1,2,3)); 
```



 

 