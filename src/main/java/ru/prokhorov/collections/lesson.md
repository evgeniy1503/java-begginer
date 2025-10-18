# Урок по Java Collections Framework

## Оглавление
1. [Введение в Collections Framework](#введение)
2. [Иерархия коллекций](#иерархия)
3. [Основные интерфейсы](#интерфейсы)
4. [Реализации коллекций](#реализации)
5. [Методы коллекций](#методы)
6. [Выбор коллекции](#выбор)
7. [Примеры использования](#примеры)

## Введение в Collections Framework {#введение}

Java Collections Framework (JCF) - это архитектура для представления и манипулирования коллекциями объектов. Он предоставляет:
- Интерфейсы для разных типов коллекций
- Реализации этих интерфейсов
- Алгоритмы для работы с коллекциями (поиск, сортировка)

## Иерархия коллекций {#иерархия}

```
Iterable
    ↓
Collection
    ├── List
    ├── Set
    │   ├── SortedSet → NavigableSet
    └── Queue
        ├── Deque
        └── BlockingQueue

Map
    ├── SortedMap → NavigableMap
    └── ConcurrentMap
```

## Основные интерфейсы {#интерфейсы}

### 1. Collection<E>
Базовый интерфейс для всех коллекций

### 2. List<E>
- Упорядоченная коллекция (сохраняет порядок добавления)
- Допускает дубликаты
- Доступ по индексу

### 3. Set<E>
- Не допускает дубликаты
- Не гарантирует порядок (кроме LinkedHashSet и TreeSet)

### 4. Queue<E>
- Коллекция для хранения элементов перед обработкой
- FIFO (First-In-First-Out) или приоритетный порядок

### 5. Map<K,V>
- Не наследует Collection
- Хранит пары ключ-значение
- Ключи уникальны

## Реализации коллекций {#реализации}

### List реализации:
- **ArrayList** - динамический массив, быстрый доступ по индексу
- **LinkedList** - двусвязный список, быстрая вставка/удаление
- **Vector** - устаревший синхронизированный аналог ArrayList
- **Stack** - устаревший LIFO

### Set реализации:
- **HashSet** - хэш-таблица, не гарантирует порядок
- **LinkedHashSet** - сохраняет порядок добавления
- **TreeSet** - отсортированное множество (красно-черное дерево)

### Queue реализации:
- **PriorityQueue** - очередь с приоритетом
- **ArrayDeque** - двусторонняя очередь на массиве
- **LinkedList** - также реализует Deque

### Map реализации:
- **HashMap** - хэш-таблица
- **LinkedHashMap** - сохраняет порядок добавления
- **TreeMap** - отсортированная по ключам карта
- **Hashtable** - устаревший синхронизированный аналог HashMap
- **ConcurrentHashMap** - потокобезопасная HashMap

## Методы коллекций {#методы}

### Основные методы Collection:
```java
// Добавление
boolean add(E e)
boolean addAll(Collection<? extends E> c)

// Удаление
boolean remove(Object o)
boolean removeAll(Collection<?> c)
boolean retainAll(Collection<?> c)
void clear()

// Поиск и проверка
boolean contains(Object o)
boolean containsAll(Collection<?> c)
boolean isEmpty()
int size()

// Итерация
Iterator<E> iterator()
Object[] toArray()
<T> T[] toArray(T[] a)

// Java 8+ Stream API
Stream<E> stream()
Stream<E> parallelStream()
```

### Методы List:
```java
// Доступ по индексу
E get(int index)
E set(int index, E element)
void add(int index, E element)
E remove(int index)

// Поиск
int indexOf(Object o)
int lastIndexOf(Object o)

// Подсписки
List<E> subList(int fromIndex, int toIndex)

// Сортировка
void sort(Comparator<? super E> c)
```

### Методы Map:
```java
// Основные операции
V put(K key, V value)
V get(Object key)
V remove(Object key)
boolean containsKey(Object key)
boolean containsValue(Object value)

// Просмотр
Set<K> keySet()
Collection<V> values()
Set<Map.Entry<K, V>> entrySet()

// Java 8+
V getOrDefault(Object key, V defaultValue)
V putIfAbsent(K key, V value)
void forEach(BiConsumer<? super K, ? super V> action)
```

## Выбор коллекции {#выбор}

### Когда использовать:
- **ArrayList** - частый доступ по индексу, редко изменяемый размер
- **LinkedList** - частые вставки/удаления в середине, реализация стека/очереди
- **HashSet** - уникальные элементы, порядок не важен
- **LinkedHashSet** - уникальные элементы + порядок добавления
- **TreeSet** - уникальные отсортированные элементы
- **HashMap** - пары ключ-значение, порядок не важен
- **LinkedHashMap** - пары ключ-значение + порядок добавления
- **TreeMap** - отсортированные по ключу пары
- **PriorityQueue** - обработка по приоритету

## Примеры использования {#примеры}

### 1. ArrayList
```java
import java.util.ArrayList;
import java.util.List;

public class ArrayListExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        
        // Добавление элементов
        list.add("Яблоко");
        list.add("Банан");
        list.add("Апельсин");
        list.add("Банан"); // Дубликат разрешен
        
        System.out.println("ArrayList: " + list);
        
        // Доступ по индексу
        System.out.println("Элемент по индексу 1: " + list.get(1));
        
        // Удаление
        list.remove("Банан");
        System.out.println("После удаления 'Банан': " + list);
        
        // Итерация
        for (String fruit : list) {
            System.out.println(fruit);
        }
    }
}
```

### 2. LinkedList
```java
import java.util.LinkedList;
import java.util.List;

public class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<String> linkedList = new LinkedList<>();
        
        // Добавление элементов
        linkedList.add("Первый");
        linkedList.add("Второй");
        linkedList.addFirst("Новый первый");
        linkedList.addLast("Новый последний");
        
        System.out.println("LinkedList: " + linkedList);
        
        // Использование как стека и очереди
        linkedList.push("Элемент стека"); // добавить в начало
        System.out.println("Pop: " + linkedList.pop()); // удалить первый
        
        // Получение первого и последнего
        System.out.println("Первый: " + linkedList.getFirst());
        System.out.println("Последний: " + linkedList.getLast());
    }
}
```

### 3. HashSet
```java
import java.util.HashSet;
import java.util.Set;

public class HashSetExample {
    public static void main(String[] args) {
        Set<String> set = new HashSet<>();
        
        // Добавление элементов
        set.add("Красный");
        set.add("Зеленый");
        set.add("Синий");
        set.add("Красный"); // Дубликат не добавится
        
        System.out.println("HashSet: " + set);
        
        // Проверка наличия
        System.out.println("Содержит 'Зеленый': " + set.contains("Зеленый"));
        
        // Удаление
        set.remove("Синий");
        System.out.println("После удаления: " + set);
        
        // Размер
        System.out.println("Размер: " + set.size());
    }
}
```

### 4. TreeSet
```java
import java.util.TreeSet;
import java.util.Set;

public class TreeSetExample {
    public static void main(String[] args) {
        Set<String> treeSet = new TreeSet<>();
        
        // Элементы автоматически сортируются
        treeSet.add("Яблоко");
        treeSet.add("Банан");
        treeSet.add("Апельсин");
        treeSet.add("Киви");
        
        System.out.println("TreeSet (отсортированный): " + treeSet);
        
        // Первый и последний элементы
        TreeSet<String> ts = (TreeSet<String>) treeSet;
        System.out.println("Первый: " + ts.first());
        System.out.println("Последний: " + ts.last());
        
        // Подмножество
        System.out.println("Подмножество: " + ts.subSet("Банан", "Яблоко"));
    }
}
```

### 5. HashMap
```java
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        Map<Integer, String> map = new HashMap<>();
        
        // Добавление элементов
        map.put(1, "Яблоко");
        map.put(2, "Банан");
        map.put(3, "Апельсин");
        map.put(1, "Груша"); // Перезапишет значение для ключа 1
        
        System.out.println("HashMap: " + map);
        
        // Получение значения
        System.out.println("Ключ 2: " + map.get(2));
        
        // Проверка ключа и значения
        System.out.println("Содержит ключ 3: " + map.containsKey(3));
        System.out.println("Содержит значение 'Банан': " + map.containsValue("Банан"));
        
        // Итерация по записям
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.println("Ключ: " + entry.getKey() + ", Значение: " + entry.getValue());
        }
        
        // Java 8 forEach
        map.forEach((k, v) -> System.out.println(k + " => " + v));
    }
}
```

### 6. TreeMap
```java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapExample {
    public static void main(String[] args) {
        Map<String, Integer> treeMap = new TreeMap<>();
        
        // Элементы автоматически сортируются по ключу
        treeMap.put("Яблоко", 10);
        treeMap.put("Банан", 5);
        treeMap.put("Апельсин", 8);
        treeMap.put("Киви", 3);
        
        System.out.println("TreeMap (отсортированный по ключу): " + treeMap);
        
        // Первый и последний ключ
        TreeMap<String, Integer> tm = (TreeMap<String, Integer>) treeMap;
        System.out.println("Первый ключ: " + tm.firstKey());
        System.out.println("Последний ключ: " + tm.lastKey());
        
        // Получение подкарты
        System.out.println("Подкарта: " + tm.subMap("Банан", "Яблоко"));
    }
}
```

### 7. PriorityQueue
```java
import java.util.PriorityQueue;
import java.util.Queue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Очередь с натуральным порядком
        Queue<Integer> priorityQueue = new PriorityQueue<>();
        
        priorityQueue.offer(5);
        priorityQueue.offer(1);
        priorityQueue.offer(3);
        priorityQueue.offer(2);
        
        System.out.println("PriorityQueue: " + priorityQueue);
        
        // Извлечение в порядке приоритета
        while (!priorityQueue.isEmpty()) {
            System.out.println("Извлечен: " + priorityQueue.poll());
        }
        
        // С пользовательским компаратором
        Queue<String> stringQueue = new PriorityQueue<>(
            (s1, s2) -> s2.length() - s1.length() // по убыванию длины
        );
        
        stringQueue.offer("короткое");
        stringQueue.offer("очень длинное");
        stringQueue.offer("среднее");
        
        while (!stringQueue.isEmpty()) {
            System.out.println("Извлечен: " + stringQueue.poll());
        }
    }
}
```

### 8. Stack и ArrayDeque
```java
import java.util.ArrayDeque;
import java.util.Deque;
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        // Устаревший Stack
        Stack<String> stack = new Stack<>();
        stack.push("Первый");
        stack.push("Второй");
        stack.push("Третий");
        
        System.out.println("Stack: " + stack);
        System.out.println("Pop: " + stack.pop());
        System.out.println("Peek: " + stack.peek());
        
        // Современная альтернатива - ArrayDeque
        Deque<String> deque = new ArrayDeque<>();
        deque.push("Первый"); // добавить в начало
        deque.push("Второй");
        deque.push("Третий");
        
        System.out.println("ArrayDeque как стек: " + deque);
        System.out.println("Pop: " + deque.pop());
        
        // Использование как очереди
        Deque<String> queue = new ArrayDeque<>();
        queue.offer("Первый"); // добавить в конец
        queue.offer("Второй");
        queue.offer("Третий");
        
        System.out.println("ArrayDeque как очередь: " + queue);
        System.out.println("Poll: " + queue.poll());
    }
}
```

# Глубокий разбор ArrayList, LinkedList и HashMap

## ArrayList

### Что это такое?
**ArrayList** - это реализация динамического массива, которая автоматически увеличивает свой размер при добавлении элементов.

```java
// Внутреннее устройство
public class ArrayList<E> {
    private static final int DEFAULT_CAPACITY = 10;
    private Object[] elementData;  // Внутренний массив
    private int size;              // Текущее количество элементов
}
```

### Процесс ВСТАВКИ

#### 1. Добавление в конец (`add(element)`)
```java
List<String> list = new ArrayList<>();
list.add("A");
```

**Шаги:**
1. Проверка capacity: `if (size == elementData.length)`
2. Если массив полный - увеличение размера:
    - Создание нового массива: `newCapacity = oldCapacity + (oldCapacity >> 1)` (увеличение на 50%)
    - Копирование элементов: `Arrays.copyOf(elementData, newCapacity)`
3. Добавление элемента: `elementData[size] = element`
4. Увеличение счетчика: `size++`

**Временная сложность:** O(1) амортизированная

#### 2. Вставка по индексу (`add(index, element)`)
```java
list.add(1, "B");  // Вставка на позицию 1
```

**Шаги:**
1. Проверка индекса: `rangeCheckForAdd(index)`
2. Проверка capacity (при необходимости увеличение)
3. Сдвиг элементов вправо: `System.arraycopy(elementData, index, elementData, index + 1, size - index)`
4. Вставка элемента: `elementData[index] = element`
5. Увеличение счетчика: `size++`

**Временная сложность:** O(n)

### Процесс ПОИСКА

#### 1. Поиск по индексу (`get(index)`)
```java
String element = list.get(1);
```

**Шаги:**
1. Проверка индекса: `rangeCheck(index)`
2. Возврат элемента: `return (E) elementData[index]`

**Временная сложность:** O(1)

#### 2. Поиск по значению (`indexOf(element)`)
```java
int index = list.indexOf("B");
```

**Шаги:**
1. Поиск по массиву:
    - Для null: `for (int i = 0; i < size; i++) if (elementData[i] == null) return i;`
    - Для объектов: `for (int i = 0; i < size; i++) if (element.equals(elementData[i])) return i;`
2. Возврат -1 если не найден

**Временная сложность:** O(n)

### Процесс УДАЛЕНИЯ

#### 1. Удаление по индексу (`remove(index)`)
```java
list.remove(1);
```

**Шаги:**
1. Проверка индекса: `rangeCheck(index)`
2. Получение удаляемого элемента: `E oldValue = elementData(index)`
3. Расчет количества элементов для сдвига: `int numMoved = size - index - 1`
4. Если numMoved > 0, сдвиг влево: `System.arraycopy(elementData, index + 1, elementData, index, numMoved)`
5. Обнуление последнего элемента: `elementData[--size] = null`

**Временная сложность:** O(n)

#### 2. Удаление по значению (`remove(element)`)
```java
list.remove("B");
```

**Шаги:**
1. Поиск индекса элемента (аналогично `indexOf()`)
2. Если найден - вызов `remove(index)`
3. Возврат true/false в зависимости от успеха

**Временная сложность:** O(n)

---

## LinkedList

### Что это такое?
**LinkedList** - это двусвязный список, где каждый элемент содержит ссылки на предыдущий и следующий элементы.

```java
// Внутреннее устройство
public class LinkedList<E> {
    private static class Node<E> {
        E item;         // Данные
        Node<E> next;   // Следующий элемент
        Node<E> prev;   // Предыдущий элемент
    }
    
    private Node<E> first;  // Первый элемент
    private Node<E> last;   // Последний элемент
    private int size;       // Размер списка
}
```

### Процесс ВСТАВКИ

#### 1. Добавление в конец (`add(element)`)
```java
List<String> list = new LinkedList<>();
list.add("A");
```

**Шаги:**
1. Создание нового узла:
   ```java
   final Node<E> l = last;
   final Node<E> newNode = new Node<>(l, element, null);
   ```
2. Обновление ссылок:
    - `last = newNode`
    - Если список был пуст: `first = newNode`
    - Иначе: `l.next = newNode`
3. Увеличение размера: `size++`

**Временная сложность:** O(1)

#### 2. Вставка по индексу (`add(index, element)`)
```java
list.add(1, "B");
```

**Шаги:**
1. Проверка индекса
2. Если index == size: вставка в конец
3. Иначе поиск узла по индексу:
   ```java
   Node<E> x = first;
   for (int i = 0; i < index; i++)
       x = x.next;
   ```
4. Создание нового узла и обновление ссылок соседних узлов

**Временная сложность:** O(n) - из-за поиска позиции

### Процесс ПОИСКА

#### 1. Поиск по индексу (`get(index)`)
```java
String element = list.get(1);
```

**Шаги:**
1. Проверка индекса
2. Поиск узла:
    - Если index < size/2: поиск с начала
    - Иначе: поиск с конца
3. Возврат элемента узла

**Временная сложность:** O(n)

#### 2. Поиск по значению (`indexOf(element)`)
```java
int index = list.indexOf("B");
```

**Шаги:**
1. Поиск по списку:
   ```java
   int index = 0;
   for (Node<E> x = first; x != null; x = x.next) {
       if (element.equals(x.item))
           return index;
       index++;
   }
   ```
2. Возврат -1 если не найден

**Временная сложность:** O(n)

### Процесс УДАЛЕНИЯ

#### 1. Удаление по индексу (`remove(index)`)
```java
list.remove(1);
```

**Шаги:**
1. Поиск узла по индексу
2. Обновление ссылок соседних узлов:
   ```java
   final E element = x.item;
   final Node<E> next = x.next;
   final Node<E> prev = x.prev;
   
   if (prev == null) {
       first = next;
   } else {
       prev.next = next;
       x.prev = null;
   }
   
   if (next == null) {
       last = prev;
   } else {
       next.prev = prev;
       x.next = null;
   }
   ```
3. Обнуление данных: `x.item = null`
4. Уменьшение размера: `size--`

**Временная сложность:** O(n) - из-за поиска узла

#### 2. Удаление по значению (`remove(element)`)
```java
list.remove("B");
```

**Шаги:**
1. Поиск узла с заданным значением
2. Если найден - удаление узла (аналогично удалению по индексу)
3. Возврат true/false

**Временная сложность:** O(n)

---

## HashMap

### Что это такое?
**HashMap** - структура данных, основанная на хэш-таблице, хранящая пары ключ-значение.

```java
// Внутреннее устройство (Java 8+)
public class HashMap<K,V> {
    static class Node<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;  // Для разрешения коллизий
    }
    
    private Node<K,V>[] table;     // Массив бакетов
    private int size;              // Количество элементов
    private float loadFactor;      // Коэффициент загрузки (по умолчанию 0.75)
    private int threshold;         // Порог расширения (capacity * loadFactor)
}
```

### Процесс ВСТАВКИ (`put(key, value)`)

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
```

**Шаги:**

#### 1. Расчет хэша
```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

#### 2. Определение бакета
```java
int index = (n - 1) & hash;  // n - размер таблицы
```

#### 3. Вставка в бакет
- Если бакет пустой: создание новой ноды
- Если бакет занят:
    - Ключ существует: обновление значения
    - Коллизия: добавление в цепочку (LinkedList) или дерево (Tree)

#### 4. Проверка расширения
```java
if (++size > threshold)
    resize();  // Увеличение таблицы в 2 раза
```

**Временная сложность:** O(1) в среднем случае

### Процесс ПОИСКА (`get(key)`)

```java
Integer value = map.get("apple");
```

**Шаги:**

#### 1. Расчет хэша ключа
```java
int hash = hash(key);
```

#### 2. Определение бакета
```java
int index = (n - 1) & hash;
```

#### 3. Поиск в бакете
- Если бакет содержит одну ноду: проверка ключа
- Если цепочка: линейный поиск по LinkedList
- Если дерево: поиск в красно-черном дереве

#### 4. Возврат значения или null

**Временная сложность:** O(1) в среднем случае

### Процесс УДАЛЕНИЯ (`remove(key)`)

```java
map.remove("apple");
```

**Шаги:**

#### 1. Расчет хэша и поиск бакета (аналогично get)

#### 2. Удаление ноды
- Если первая нода в бакете: `table[index] = node.next`
- Если в цепочке: обновление ссылок соседних нод
- Если в дереве: балансировка дерева

#### 3. Уменьшение размера: `size--`

**Временная сложность:** O(1) в среднем случае

### Особенности Java 8+

#### Преобразование в дерево
Когда цепочка в бакете становится длинной (TREEIFY_THRESHOLD = 8), LinkedList преобразуется в красно-черное дерево:

```java
if (binCount >= TREEIFY_THRESHOLD - 1)
    treeifyBin(tab, hash);
```

#### Обратное преобразование
Когда размер дерева уменьшается (UNTREEIFY_THRESHOLD = 6), дерево преобразуется обратно в LinkedList.

## Сравнительная таблица

| Операция | ArrayList | LinkedList | HashMap |
|----------|-----------|------------|---------|
| **Вставка в конец** | O(1) амортизированная | O(1) | - |
| **Вставка в начало** | O(n) | O(1) | - |
| **Вставка в середину** | O(n) | O(n) | - |
| **Вставка пары** | - | - | O(1) |
| **Поиск по индексу** | O(1) | O(n) | - |
| **Поиск по значению** | O(n) | O(n) | - |
| **Поиск по ключу** | - | - | O(1) |
| **Удаление по индексу** | O(n) | O(n) | - |
| **Удаление по значению** | O(n) | O(n) | - |
| **Удаление по ключу** | - | - | O(1) |
| **Память** | Меньше (только данные) | Больше (данные + ссылки) | Средняя |

## Практические примеры

### ArrayList в действии
```java
// Вставка - быстро в конец, медленно в начало
ArrayList<Integer> list = new ArrayList<>();
long start = System.nanoTime();
for (int i = 0; i < 100000; i++) {
    list.add(i);  // Быстро - O(1)
}
long end = System.nanoTime();
System.out.println("Вставка в конец: " + (end - start) + " ns");

start = System.nanoTime();
for (int i = 0; i < 1000; i++) {
    list.add(0, i);  // Медленно - O(n)
}
end = System.nanoTime();
System.out.println("Вставка в начало: " + (end - start) + " ns");
```

### LinkedList в действии
```java
// Вставка в начало - быстро
LinkedList<Integer> linked = new LinkedList<>();
long start = System.nanoTime();
for (int i = 0; i < 100000; i++) {
    linked.addFirst(i);  // Быстро - O(1)
}
long end = System.nanoTime();
System.out.println("LinkedList вставка в начало: " + (end - start) + " ns");
```

### HashMap в действии
```java
// Быстрый поиск по ключу
HashMap<String, Integer> map = new HashMap<>();
for (int i = 0; i < 100000; i++) {
    map.put("key" + i, i);
}

long start = System.nanoTime();
Integer value = map.get("key50000");
long end = System.nanoTime();
System.out.println("HashMap поиск: " + (end - start) + " ns");
```

## Ключевые выводы

1. **ArrayList** - лучший выбор когда:
    - Частый доступ по индексу
    - Добавление преимущественно в конец
    - Размер известен заранее

2. **LinkedList** - лучший выбор когда:
    - Частые вставки/удаления в начале/середине
    - Реализация стека/очереди
    - Нужен двусторонний доступ

3. **HashMap** - лучший выбор когда:
    - Нужен быстрый поиск по ключу
    - Порядок элементов не важен
    - Уникальные ключи

Понимание внутреннего устройства этих коллекций поможет выбирать оптимальные структуры данных для конкретных задач и писать более эффективный код.

## Лучшие практики

1. **Используйте интерфейсы в объявлениях**: `List<String> list = new ArrayList<>();`
2. **Выбирайте коллекцию по потребностям**: доступ по индексу, порядок, уникальность
3. **Инициализируйте с начальной емкостью** если известен размер: `new ArrayList<>(100)`
4. **Используйте Collections.unmodifiableList()** для неизменяемых коллекций
5. **Для многопоточности** используйте ConcurrentHashMap, CopyOnWriteArrayList

Этот урок охватывает основные аспекты Java Collections Framework. Практикуйтесь с разными типами коллекций, чтобы лучше понять их особенности и выбрать подходящую для каждой задачи.