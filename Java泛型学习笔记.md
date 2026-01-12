# Java 泛型学习笔记

## 1. 为什么要用泛型
- **类型安全**：在编译期发现类型不匹配的问题，避免运行时 `ClassCastException`。
- **消除强制类型转换**：减少样板代码，让 API 更清晰。
- **提高可读性与可维护性**：使用类型参数表达意图，例如 `List<String>`。

## 2. 泛型的基本语法
### 2.1 泛型类
```java
public class Box<T> {
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}

Box<Integer> intBox = new Box<>();
intBox.set(10);
```

### 2.2 泛型接口
```java
public interface Repository<T> {
    void save(T entity);
    T findById(String id);
}
```

### 2.3 泛型方法
```java
public class ArrayUtils {
    public static <T> T first(T[] array) {
        return array[0];
    }
}
```

## 3. 类型参数命名约定
- `T`：Type（最常见）
- `E`：Element
- `K`、`V`：Key、Value
- `N`：Number

## 4. 有界类型参数（Bounded Type Parameter）
通过 `extends` 限定上界，允许在泛型中使用特定方法。

```java
public static <T extends Number> double sum(T a, T b) {
    return a.doubleValue() + b.doubleValue();
}
```

## 5. 通配符（Wildcard）
### 5.1 无界通配符
```java
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

### 5.2 上界通配符 `? extends`
用于“只读”场景（生产者）。

```java
public static double sumList(List<? extends Number> list) {
    double sum = 0;
    for (Number n : list) {
        sum += n.doubleValue();
    }
    return sum;
}
```

### 5.3 下界通配符 `? super`
用于“只写”场景（消费者）。

```java
public static void addIntegers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}
```

### 5.4 PECS 原则
- **Producer Extends**：如果你只从集合中读取，用 `? extends`。
- **Consumer Super**：如果你只往集合中写入，用 `? super`。

## 6. 类型擦除（Type Erasure）
- Java 泛型在编译期存在，运行期会擦除类型信息。
- `List<String>` 与 `List<Integer>` 在运行期都是 `List`。
- 不能直接创建泛型数组：`new T[]`（编译错误）。

常见替代方式：
```java
@SuppressWarnings("unchecked")
T[] array = (T[]) new Object[size];
```

## 7. 泛型的限制
- 不能实例化类型参数：`new T()`（需改用工厂或反射）。
- 不能创建泛型数组：`new T[10]`。
- 不能使用 `instanceof` 判断具体泛型类型：`obj instanceof List<String>`（编译错误）。

## 8. 常见应用场景
- 集合类：`List<T>`、`Map<K, V>`。
- 数据访问层：`Repository<T>`。
- 通用工具方法：`Collections`、`Arrays` 中的泛型方法。

## 9. 小结
- 泛型让 Java 在编译期就能保证类型安全。
- 掌握 **有界类型参数** 与 **通配符** 是核心。
- 牢记 **PECS** 原则，能在设计 API 时写出更合理的类型签名。

---

> 建议结合 IDE 的类型提示与编译器报错信息，多做练习题来巩固理解。
