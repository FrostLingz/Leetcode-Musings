# HashMap

### hashmap.merge(key, value, remappingFunction)
```java
import java.util.HashMap;

class Main {
    public static void main(String[] args) {
    //创建一个HashMap
    HashMap<String, Integer> prices = new HashMap<>();

    // 往 HashMap 插入映射
    prices.put("Shoes", 200);
    prices.put("Bag", 300);
    prices.put("Pant", 150);
    System.out.println("HashMap: " + prices);

    int returnedValue = prices.merge("Shirt", 100, (oldValue, newValue) -> oldValue + newValue);
    System.out.println("Price of Shirt: " + returnedValue);

    // 输出更新后的 HashMap
    System.out.println("Updated HashMap: " + prices);
    }
}
```

```java
Output:
HashMap: {Pant=150, Bag=300, Shoes=200}
Price of Shirt: 100
Updated HashMap: {Pant=150, Shirt=100, Bag=300, Shoes=200}
```

```java
import java.util.HashMap;

class Main {
    public static void main(String[] args) {
        // 创建一个 HashMap
        HashMap<String, String> countries = new HashMap<>();

        // 往HashMap插入映射项
        countries.put("Washington", "America");
        countries.put("Canberra", "Australia");
        countries.put("Madrid", "Spain");
        System.out.println("HashMap: " + countries);

        //合并key为 Washington的映射
        String returnedValue = countries.merge("Washington", "USA", (oldValue, newValue) -> oldValue + "/" + newValue);
        System.out.println("Washington: " + returnedValue);

        //输出更新后的HashMap
        System.out.println("Updated HashMap: " + countries);
    }
}
```

```java
Output:
HashMap: {Madrid=Spain, Canberra=Australia, Washington=America}
Washington: America/USA
Updated HashMap: {Madrid=Spain, Canberra=Australia, Washington=America/USA}
```

### hashmap.putIfAbsent(K key, V value)

```java
import java.util.HashMap;

class Main {
    public static void main(String[] args) {
        // 创建一个 HashMap
        HashMap<Integer, String> sites = new HashMap<>();

        // 往 HashMap 添加一些元素
        sites.put(1, "Google");
        sites.put(2, "Runoob");
        sites.put(3, "Taobao");
        System.out.println("sites HashMap: " + sites);
       

        // HashMap 中不存在该key
        sites.putIfAbsent(4, "Weibo");

        // HashMap 中存在 Key
        sites.putIfAbsent(2, "Wiki");
        System.out.println("Updated Languages: " + sites);
    }
}
```

```java
Output:
sites HashMap: {1=Google, 2=Runoob, 3=Taobao}
Updated sites HashMap: {1=Google, 2=Runoob, 3=Taobao, 4=Weibo}
```

### Map.Entry

```java
Map<String, String> map = new HashMap<String, String>();  
  map.put("1", "value1");  
  map.put("2", "value2");  
  map.put("3", "value3");  
    
  //第一种：普遍使用，由于二次取值,效率会比第二种和第三种慢一倍
  System.out.println("通过Map.keySet遍历key和value：");  
  for (String key : map.keySet()) {  
   System.out.println("key= "+ key + " and value= " + map.get(key));  
  }  
    
  //第二种  
  System.out.println("通过Map.entrySet使用iterator遍历key和value：");  
  Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();  
  while (it.hasNext()) {  
   Map.Entry<String, String> entry = it.next();  
   System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());  
  }  
   
  //Map.Entry是为了更方便的输出map键值对。一般情况下，要输出Map中的key 和 value  是先得到key的集合keySet()，然后再迭代（循环）由每个key得到每个value。values()方法是获取集   合中的所有值，不包含键，没有对应关系。而Entry可以一次性获得这两个值。
  //第三种：无法在for循环时实现remove等操作  
  System.out.println("通过Map.entrySet遍历key和value");  
  for (Map.Entry<String, String> entry : map.entrySet()) {  
   System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());  
  }  
  
  //第四种：只能获取values, 不能获取key 
  System.out.println("通过Map.values()遍历所有的value，但不能遍历key");  
  for (String v : map.values()) {  
   System.out.println("value= " + v);  
  } 
  ```
