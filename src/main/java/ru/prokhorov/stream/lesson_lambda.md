# Лямбда-выражения в Java: Полное руководство с примерами

## Что такое лямбда-выражение?

**Лямбда-выражение** - это краткий способ представления анонимной функции (функции без имени) в Java. Это блок кода, который можно передавать и выполнять позже.

## Синтаксис лямбда-выражений

```java
(параметры) -> выражение
(параметры) -> { операторы; }
```

## Эволюция: от анонимных классов к лямбдам

### До лямбда-выражений (Java 7 и ранее):

```java
// Создание Runnable с анонимным классом
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello World!");
    }
};
new Thread(runnable).start();
```

### С лямбда-выражениями (Java 8+):

```java
// Тот же код с лямбда-выражением
Runnable runnable = () -> System.out.println("Hello World!");
new Thread(runnable).start();

// Или еще короче
new Thread(() -> System.out.println("Hello World!")).start();
```

## Базовые примеры лямбда-выражений

### 1. Без параметров
```java
// Анонимный класс
Runnable oldStyle = new Runnable() {
    @Override
    public void run() {
        System.out.println("Привет!");
    }
};

// Лямбда-выражение
Runnable newStyle = () -> System.out.println("Привет!");
Runnable multiLine = () -> {
    System.out.println("Строка 1");
    System.out.println("Строка 2");
};
```

### 2. С одним параметром
```java
// Анонимный класс
Consumer<String> oldConsumer = new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
};

// Лямбда-выражение
Consumer<String> newConsumer = (String s) -> System.out.println(s);
Consumer<String> shorter = s -> System.out.println(s); // Тип можно опустить
Consumer<String> evenShorter = System.out::println; // Ссылка на метод
```

### 3. С несколькими параметрами
```java
// Анонимный класс
Comparator<Integer> oldComparator = new Comparator<Integer>() {
    @Override
    public int compare(Integer a, Integer b) {
        return a.compareTo(b);
    }
};

// Лямбда-выражение
Comparator<Integer> newComparator = (Integer a, Integer b) -> a.compareTo(b);
Comparator<Integer> shorter = (a, b) -> a.compareTo(b); // Типы опущены
Comparator<Integer> shortest = Integer::compareTo; // Ссылка на метод
```

## Практические примеры использования лямбд

### Пример 1: Обработка коллекций
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "Diana");

// До лямбд
for (String name : names) {
    System.out.println(name);
}

// С лямбдами
names.forEach(name -> System.out.println(name));
names.forEach(System.out::println); // Ссылка на метод
```

### Пример 2: Фильтрация с Stream API
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Четные числа
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

// Числа больше 5
List<Integer> largeNumbers = numbers.stream()
    .filter(n -> n > 5)
    .collect(Collectors.toList());
```

### Пример 3: Преобразование объектов
```java
List<String> words = Arrays.asList("java", "lambda", "stream");

// Преобразование в верхний регистр
List<String> upperCase = words.stream()
    .map(word -> word.toUpperCase())
    .collect(Collectors.toList());

// Длина каждого слова
List<Integer> lengths = words.stream()
    .map(word -> word.length())
    .collect(Collectors.toList());
```

### Пример 4: Сортировка
```java
List<String> names = Arrays.asList("John", "Alice", "Bob", "Anna");

// Сортировка по длине имени
names.sort((name1, name2) -> Integer.compare(name1.length(), name2.length()));

// Сортировка в обратном порядке
names.sort((name1, name2) -> name2.compareTo(name1));
```

## Функциональные интерфейсы и лямбды

Лямбда-выражения работают с **функциональными интерфейсами** - интерфейсами с одним абстрактным методом.

### Основные функциональные интерфейсы:

```java
// Predicate - проверка условия
Predicate<String> isLong = s -> s.length() > 5;
boolean result = isLong.test("Hello World"); // true

// Function - преобразование
Function<String, Integer> stringToLength = s -> s.length();
int length = stringToLength.apply("Hello"); // 5

// Consumer - выполнение действия
Consumer<String> printer = s -> System.out.println(s);
printer.accept("Hello!");

// Supplier - поставщик значений
Supplier<Double> randomSupplier = () -> Math.random();
double random = randomSupplier.get();

// UnaryOperator - оператор над одним типом
UnaryOperator<String> toUpper = s -> s.toUpperCase();
String result = toUpper.apply("hello"); // "HELLO"

// BinaryOperator - оператор над двумя значениями
BinaryOperator<Integer> sum = (a, b) -> a + b;
int total = sum.apply(5, 3); // 8
```

## Создание собственных функциональных интерфейсов

```java
// Пометка @FunctionalInterface не обязательна, но рекомендуется
@FunctionalInterface
interface StringProcessor {
    String process(String input);
    
    // Можно иметь default методы
    default String processTwice(String input) {
        return process(process(input));
    }
    
    // Можно иметь static методы
    static StringProcessor createRepeater(int times) {
        return input -> input.repeat(times);
    }
}

// Использование
StringProcessor toUpper = s -> s.toUpperCase();
StringProcessor repeater = s -> s + " " + s;

System.out.println(toUpper.process("hello")); // HELLO
System.out.println(repeater.process("java")); // java java
```

## Лямбды с захватом переменных

### Захват final переменных:
```java
final String prefix = "Hello, ";
List<String> names = Arrays.asList("Alice", "Bob");

// Лямбда захватывает final переменную prefix
names.forEach(name -> System.out.println(prefix + name));
```

### Effectively final переменные:
```java
String suffix = "!";
int multiplier = 2;

// Переменные effectively final (не изменяются после инициализации)
names.forEach(name -> {
    String message = name + suffix;
    System.out.println(message.repeat(multiplier));
});

// ОШИБКА! Переменная не effectively final
// multiplier = 3; // Раскомментировать для ошибки
```

## Более сложные примеры

### Пример 1: Обработка исключений в лямбдах
```java
List<String> urls = Arrays.asList("http://example.com", "invalid_url");

// Обработка проверяемых исключений
urls.forEach(url -> {
    try {
        // URL processing code
        new URL(url);
    } catch (MalformedURLException e) {
        throw new RuntimeException(e);
    }
});
```

### Пример 2: Композиция функций
```java
Function<String, String> toUpper = String::toUpperCase;
Function<String, String> addExclamation = s -> s + "!";
Function<String, String> addPrefix = s -> "Hello " + s;

// Композиция функций
Function<String, String> greeting = addPrefix
    .andThen(toUpper)
    .andThen(addExclamation);

System.out.println(greeting.apply("world")); // HELLO WORLD!
```

### Пример 3: Цепочка предикатов
```java
Predicate<String> startsWithA = s -> s.startsWith("A");
Predicate<String> longerThan3 = s -> s.length() > 3;
Predicate<String> containsE = s -> s.contains("e");

// Комбинация предикатов
Predicate<String> complexCondition = startsWithA
    .and(longerThan3)
    .or(containsE)
    .negate();

List<String> names = Arrays.asList("Alice", "Bob", "Anna", "Eve", "Tom");
List<String> filtered = names.stream()
    .filter(complexCondition)
    .collect(Collectors.toList()); // [Bob, Tom]
```

## Особенности лямбда-выражений

### 1. **Краткость**
```java
// Вместо:
Collections.sort(list, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }
});

// Пишем:
Collections.sort(list, (s1, s2) -> s1.compareTo(s2));
```

### 2. **Отсутствие имени**
Лямбды анонимны - у них нет имени метода

### 3. **Тип выводится из контекста**
```java
// Компилятор понимает, что это Comparator<String>
Comparator<String> comparator = (s1, s2) -> s1.compareTo(s2);
```

### 4. **Могут захватывать переменные из окружающего контекста**
```java
String external = "external";
Function<String, String> func = s -> s + external;
```

## Когда использовать лямбда-выражения

1. **Обработка коллекций** (forEach, filter, map, etc.)
2. **Потоки данных** (Stream API)
3. **Асинхронное программирование** (CompletableFuture)
4. **Обработка событий** (GUI, listeners)
5. **Шаблоны проектирования** (Strategy, Command)

Лямбда-выражения сделали Java более выразительной и современной, позволив писать более чистый и читаемый код!