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

        //合并 key为 Washington的映射
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
