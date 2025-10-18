Отлично! Вот три задачи по Generics разного уровня сложности, которые отлично подойдут для закрепления материала.

---

### Задача 1: Уровень "Начинающий" 📘

**Название:** "Универсальный хранитель пар"

**Описание:**
Создайте обобщенный класс `Pair`, который может хранить два объекта любого типа. Эти типы могут быть одинаковыми или разными.

**Требования:**
1. Создайте generic-класс `Pair` с двумя полями: `first` и `second`
2. Реализуйте конструктор, принимающий оба значения
3. Реализуйте геттеры и сеттеры для обоих полей
4. Реализуйте метод `swap()`, который меняет местами значения полей `first` и `second`
5. Переопределите метод `toString()`, который возвращает строковое представление пары

**Пример использования:**
```java
public class Main {
    public static void main(String[] args) {
        Pair<String, Integer> pair1 = new Pair<>("Age", 25);
        System.out.println(pair1); // Должно вывести: Pair{first=Age, second=25}
        
        pair1.swap();
        System.out.println(pair1); // Должно вывести: Pair{first=25, second=Age}
        
        Pair<Double, Double> coordinates = new Pair<>(10.5, 20.3);
        System.out.println(coordinates); // Должно вывести: Pair{first=10.5, second=20.3}
    }
}
```

---

### Задача 2: Уровень "Продолжающий" 📗

**Название:** "Типобезопасный калькулятор"

**Описание:**
Создайте утилитный класс `MathUtils` с обобщенными методами для математических операций, который будет работать только с числами.

**Требования:**
1. Создайте класс `MathUtils` с обобщенными статическими методами
2. Реализуйте метод `add(T a, T b)`, который возвращает сумму двух чисел
3. Реализуйте метод `max(T[] array)`, который находит максимальный элемент в массиве
4. Реализуйте метод `average(List<T> list)`, который вычисляет среднее значение
5. Все методы должны работать только с типами, являющимися наследниками `Number`
6. Используйте ограниченные типы (bounded type parameters)

**Пример использования:**
```java
public class Main {
    public static void main(String[] args) {
        // Работа с Integer
        Integer result1 = MathUtils.add(5, 10);
        System.out.println("5 + 10 = " + result1); // 15
        
        Integer[] numbers = {1, 5, 3, 8, 2};
        Integer max = MathUtils.max(numbers);
        System.out.println("Максимум: " + max); // 8
        
        // Работа с Double
        List<Double> doubles = Arrays.asList(1.5, 2.5, 3.5);
        Double avg = MathUtils.average(doubles);
        System.out.println("Среднее: " + avg); // 2.5
        
        // MathUtils.add("hello", "world"); // Должна быть ошибка компиляции!
    }
}
```

**Подсказка:** Вам понадобится использовать методы `doubleValue()`, `intValue()` из класса `Number`.

---

### Задача 3: Уровень "Продвинутый" 📙

**Название:** "Гибкая система фильтрации коллекций"

**Описание:**
Создайте утилитный класс `CollectionFilter` с методами для фильтрации коллекций по различным критериям, используя Generics и функциональные интерфейсы.

**Требования:**
1. Создайте класс `CollectionFilter` с обобщенными статическими методами
2. Реализуйте метод `filter(List<T> list, Predicate<T> predicate)`, который возвращает новый список элементов, удовлетворяющих условию предиката
3. Реализуйте метод `transform(List<T> list, Function<T, R> function)`, который преобразует каждый элемент списка и возвращает список результатов
4. Реализуйте метод `findMax(List<T> list, Comparator<T> comparator)`, который находит максимальный элемент используя компаратор
5. Реализуйте метод `groupBy(List<T> list, Function<T, K> classifier)`, который группирует элементы по ключу и возвращает `Map<K, List<T>>`

**Пример использования:**
```java
public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        // Фильтрация четных чисел
        List<Integer> evenNumbers = CollectionFilter.filter(numbers, n -> n % 2 == 0);
        System.out.println("Четные: " + evenNumbers); // [2, 4, 6, 8, 10]
        
        // Преобразование чисел в их квадраты
        List<Integer> squares = CollectionFilter.transform(numbers, n -> n * n);
        System.out.println("Квадраты: " + squares); // [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
        
        // Поиск максимального числа
        Integer max = CollectionFilter.findMax(numbers, Integer::compare);
        System.out.println("Максимум: " + max); // 10
        
        // Группировка строк по длине
        List<String> strings = Arrays.asList("apple", "banana", "cat", "dog", "elephant");
        Map<Integer, List<String>> groupedByLength = 
            CollectionFilter.groupBy(strings, String::length);
        System.out.println("Группировка по длине: " + groupedByLength);
        // {3=[cat, dog], 5=[apple], 6=[banana], 8=[elephant]}
    }
}
```

**Подсказка:** Используйте функциональные интерфейсы `Predicate<T>`, `Function<T, R>`, `Comparator<T>` из пакета `java.util.function`.

---

### Критерии оценки для каждой задачи:

**Задача 1 (Начинающий):**
- ✅ Код компилируется без ошибок
- ✅ Правильно использованы generics в классе
- ✅ Метод `swap()` корректно меняет значения
- ✅ Вывод соответствует ожидаемому

**Задача 2 (Продолжающий):**
- ✅ Правильное использование bounded type parameters
- ✅ Обработка разных числовых типов
- ✅ Корректные математические вычисления
- ✅ Обработка граничных случаев (пустые массивы/списки)

**Задача 3 (Продвинутый):**
- ✅ Сложные generic-сигнатуры методов
- ✅ Интеграция с функциональными интерфейсами
- ✅ Эффективная работа с коллекциями
- ✅ Чистота и читаемость кода

Эти задачи постепенно усложняются и охватывают различные аспекты Generics - от базового синтаксиса до сложных сценариев с функциональными интерфейсами!