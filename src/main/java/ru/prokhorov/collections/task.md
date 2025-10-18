# Задачи по Java Collections разного уровня сложности

## Задача 1: Базовый уровень - "Управление списком студентов"

### Цель:
Закрепить базовые операции с ArrayList, работу с объектами и сортировкой.

### Описание:
Создай класс `Student` и программу для управления списком студентов.

### Требования:

#### 1. Класс Student:
```java
public class Student {
    private String name;
    private int age;
    private double averageGrade;
    
    // конструктор, геттеры, сеттеры, toString()
}
```

#### 2. Основная программа должна уметь:
- Добавлять студентов в список
- Удалять студента по имени
- Находить студента с самым высоким средним баллом
- Выводить список студентов, отсортированный по имени
- Выводить список студентов, отсортированный по возрасту
- Рассчитывать средний возраст всех студентов

#### 3. Пример использования:
```java
StudentManager manager = new StudentManager();
manager.addStudent(new Student("Анна", 20, 4.5));
manager.addStudent(new Student("Иван", 22, 4.2));
manager.addStudent(new Student("Мария", 21, 4.8));

manager.printAllStudents();
// Вывод:
// Студент: Анна, Возраст: 20, Средний балл: 4.5
// Студент: Иван, Возраст: 22, Средний балл: 4.2
// Студент: Мария, Возраст: 21, Средний балл: 4.8

System.out.println("Лучший студент: " + manager.findBestStudent());
// Вывод: Лучший студент: Студент: Мария, Возраст: 21, Средний балл: 4.8

manager.removeStudent("Иван");
manager.printStudentsSortedByAge();
```

### Ожидаемый результат:
- Работающий класс Student
- Класс StudentManager с методами управления
- Демонстрация работы всех методов

---

## Задача 2: Средний уровень - "Система учета заказов"

### Цель:
Научиться работать с HashMap, вложенными коллекциями и сложными операциями.

### Описание:
Создай систему учета заказов для интернет-магазина, где каждый клиент может иметь несколько заказов.

### Требования:

#### 1. Классы:
```java
public class Product {
    private String name;
    private double price;
    private String category;
    // конструктор, геттеры, equals(), hashCode()
}

public class Order {
    private int orderId;
    private List<Product> products;
    private LocalDate orderDate;
    private OrderStatus status; // NEW, PROCESSING, COMPLETED, CANCELLED
    // методы: addProduct(), removeProduct(), getTotalPrice()
}

public class Customer {
    private String email;
    private String name;
    private List<Order> orders;
    // методы: addOrder(), getTotalSpent()
}
```

#### 2. Класс OrderSystem:
```java
public class OrderSystem {
    private Map<String, Customer> customers; // key: email
    
    // Основные методы:
    public void addCustomer(Customer customer)
    public void placeOrder(String email, Order order)
    public List<Order> getCustomerOrders(String email)
    public double getCustomerTotalSpent(String email)
    public List<Customer> getTopCustomers(int count)
    public Map<String, Integer> getProductsStatistics() // сколько раз каждый товар был заказан
    public List<Order> getOrdersByStatus(OrderStatus status)
    public boolean cancelOrder(String email, int orderId)
}
```

#### 3. Пример использования:
```java
OrderSystem system = new OrderSystem();

Customer customer1 = new Customer("ivan@mail.ru", "Иван");
system.addCustomer(customer1);

Product laptop = new Product("Ноутбук", 50000, "Электроника");
Product mouse = new Product("Мышь", 1500, "Электроника");

Order order1 = new Order(1);
order1.addProduct(laptop);
order1.addProduct(mouse);

system.placeOrder("ivan@mail.ru", order1);

System.out.println("Общая сумма заказов Ивана: " + 
    system.getCustomerTotalSpent("ivan@mail.ru"));
    
System.out.println("Статистика товаров: " + 
    system.getProductsStatistics());
```

### Дополнительные задания:
1. Реализуйте поиск всех заказов за определенный период
2. Добавьте метод для получения самого популярного товара
3. Реализуйте скидочную систему для постоянных клиентов

### Ожидаемый результат:
- Полностью рабочая система учета заказов
- Эффективное использование HashMap для быстрого доступа
- Обработка всех возможных edge cases

---

## Задача 3: Продвинутый уровень - "Кэширующая система с LFU стратегией"

### Цель:
Понять принципы работы кэширования, реализовать сложную структуру данных, работать с производительностью.

### Описание:
Реализуй кэширующую систему с стратегией LFU (Least Frequently Used - наименее часто используемый).

### Требования:

#### 1. Интерфейс Cache:
```java
public interface Cache<K, V> {
    void put(K key, V value);
    V get(K key);
    void remove(K key);
    int size();
    void clear();
    boolean containsKey(K key);
}
```

#### 2. Класс LFUCache должен:
- Иметь ограниченный размер (capacity)
- При превышении capacity удалять наименее часто используемый элемент
- При равенстве частоты использования использовать LRU (Least Recently Used)
- Обеспечивать O(1) сложность для операций get и put

#### 3. Внутренняя структура:
```java
public class LFUCache<K, V> implements Cache<K, V> {
    private final int capacity;
    private Map<K, V> valueMap; // Хранилище значений
    private Map<K, Integer> frequencyMap; // Частота использования для каждого ключа
    private Map<Integer, LinkedHashSet<K>> frequencyLists; // Списки ключей по частоте
    
    // Минимальная частота для быстрого доступа
    private int minFrequency;
    
    public LFUCache(int capacity) {
        this.capacity = capacity;
        this.valueMap = new HashMap<>();
        this.frequencyMap = new HashMap<>();
        this.frequencyLists = new HashMap<>();
        this.frequencyLists.put(1, new LinkedHashSet<>());
        this.minFrequency = 1;
    }
}
```

#### 4. Алгоритм работы:

**Метод get(K key):**
1. Если ключа нет в кэше - вернуть null
2. Увеличить частоту использования ключа
3. Переместить ключ в список с более высокой частотой
4. Обновить minFrequency если нужно
5. Вернуть значение

**Метод put(K key, V value):**
1. Если ключ уже существует - обновить значение и увеличить частоту
2. Если кэш полный:
    - Найти ключ с минимальной частотой (minFrequency)
    - Если таких несколько - удалить самый старый (LRU)
    - Удалить выбранный ключ из всех структур
3. Добавить новый ключ со частотой 1
4. Установить minFrequency = 1

#### 5. Пример использования:
```java
Cache<String, String> cache = new LFUCache<>(3);

cache.put("A", "Apple");
cache.put("B", "Banana");
cache.put("C", "Cherry");

// Используем некоторые элементы несколько раз
cache.get("A");
cache.get("A");
cache.get("B");
cache.get("C");

// Добавляем новый элемент - должен вытеснить наименее используемый
cache.put("D", "Date");

// В кэше должны остаться: A (частота 2), B (частота 1), D (частота 1)
// Но так как B использовался раньше C, и у них одинаковая частота,
// то при добавлении D будет удален C
```

### Дополнительные задания:
1. Добавьте статистику кэша (hit/miss ratio)
2. Реализуйте TTL (Time To Live) для записей
3. Сделайте потокобезопасную версию
4. Добавьте возможность изменения capacity во время работы
5. Реализуйте сериализацию/десериализацию кэша

### Тесты для проверки:
```java
@Test
public void testLFUBehavior() {
    LFUCache<Integer, String> cache = new LFUCache<>(2);
    
    cache.put(1, "One");
    cache.put(2, "Two");
    
    cache.get(1); // увеличиваем частоту для 1
    cache.get(1); // еще раз увеличиваем
    
    cache.put(3, "Three"); // должен вытеснить 2 (частота 1) а не 1 (частота 2)
    
    assertNull(cache.get(2)); // 2 должен быть удален
    assertNotNull(cache.get(1)); // 1 должен остаться
    assertNotNull(cache.get(3)); // 3 должен быть в кэше
}

@Test
public void testLFUWithLRUFallback() {
    LFUCache<Integer, String> cache = new LFUCache<>(2);
    
    cache.put(1, "One");
    cache.put(2, "Two");
    
    cache.get(1); // частота 1 -> 2
    cache.get(2); // частота 1 -> 2
    
    // теперь у обоих частота 2
    cache.put(3, "Three"); 
    // должен использовать LRU: кто дольше не использовался?
    // зависит от реализации LinkedHashSet
}
```

### Ожидаемый результат:
- Полностью рабочая LFU кэширующая система
- Эффективные алгоритмы с O(1) сложностью основных операций
- Корректная обработка всех граничных случаев
- Чистый, хорошо документированный код

---

## Критерии оценки для каждой задачи:

### Задача 1 (Базовый уровень):
- ✅ Корректная реализация класса Student
- ✅ Рабочие методы управления списком
- ✅ Правильная сортировка с использованием Comparator
- ✅ Чистота кода и правильные модификаторы доступа

### Задача 2 (Средний уровень):
- ✅ Эффективное использование HashMap и вложенных коллекций
- ✅ Корректная работа с equals() и hashCode()
- ✅ Реализация всех требуемых бизнес-методов
- ✅ Обработка исключительных ситуаций

### Задача 3 (Продвинутый уровень):
- ✅ Корректная реализация сложного алгоритма LFU
- ✅ Обеспечение заявленной сложности O(1)
- ✅ Работоспособность в различных сценариях
- ✅ Качественные тесты и документация

Каждая задача развивает разные аспекты работы с коллекциями и подходит для студентов с соответствующим уровнем подготовки.