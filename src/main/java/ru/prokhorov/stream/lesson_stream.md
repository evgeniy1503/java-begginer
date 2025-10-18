# Java Stream API: Полное руководство

## Что такое Stream API?

**Stream API** - это новый способ работы с коллекциями данных в Java, предоставляющий мощный функциональный подход к обработке последовательностей элементов. Он позволяет выполнять сложные операции поиска, фильтрации, преобразования и агрегации данных в декларативном стиле.

## Когда появилось Stream API?

Stream API было добавлено в **Java 8** (вышла в марте 2014 года) как часть большого обновления, которое также включало лямбда-выражения и ссылки на методы.

## Какие проблемы решает Stream API?

### Проблемы до Stream API:

1. **Императивный стиль** - много шаблонного кода
2. **Сложность параллелизации** - ручное управление потоками
3. **Низкая читаемость** - сложно понять логику с первого взгляда
4. **Повторяющийся код** - одинаковые паттерны для разных операций

## Сравнение: до и после Stream API

### Пример 1: Фильтрация и преобразование

**До Stream API:**
```java
List<String> names = Arrays.asList("John", "Alice", "Bob", "Anna", "Mike");

// Фильтрация имен начинающихся на "A" и преобразование в верхний регистр
List<String> result = new ArrayList<>();
for (String name : names) {
    if (name.startsWith("A")) {
        result.add(name.toUpperCase());
    }
}
Collections.sort(result);

System.out.println(result); // [ALICE, ANNA]
```

**После Stream API:**
```java
List<String> names = Arrays.asList("John", "Alice", "Bob", "Anna", "Mike");

List<String> result = names.stream()
    .filter(name -> name.startsWith("A"))
    .map(String::toUpperCase)
    .sorted()
    .collect(Collectors.toList());

System.out.println(result); // [ALICE, ANNA]
```

### Пример 2: Поиск максимального значения

**До Stream API:**
```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6);

int max = Integer.MIN_VALUE;
for (int number : numbers) {
    if (number > max) {
        max = number;
    }
}
System.out.println("Max: " + max); // Max: 9
```

**После Stream API:**
```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6);

int max = numbers.stream()
    .max(Integer::compare)
    .orElse(Integer.MIN_VALUE);

System.out.println("Max: " + max); // Max: 9
```

### Пример 3: Группировка объектов

**До Stream API:**
```java
class Person {
    private String name;
    private int age;
    private String city;
    
    // конструктор, геттеры
}

List<Person> people = Arrays.asList(
    new Person("John", 25, "New York"),
    new Person("Alice", 30, "London"),
    new Person("Bob", 20, "New York"),
    new Person("Anna", 35, "London")
);

// Группировка людей по городу
Map<String, List<Person>> peopleByCity = new HashMap<>();
for (Person person : people) {
    String city = person.getCity();
    if (!peopleByCity.containsKey(city)) {
        peopleByCity.put(city, new ArrayList<>());
    }
    peopleByCity.get(city).add(person);
}

System.out.println(peopleByCity);
```

**После Stream API:**
```java
Map<String, List<Person>> peopleByCity = people.stream()
    .collect(Collectors.groupingBy(Person::getCity));

System.out.println(peopleByCity);
```

### Пример 4: Суммирование с условием

**До Stream API:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

int sumOfEvens = 0;
for (int number : numbers) {
    if (number % 2 == 0) {
        sumOfEvens += number;
    }
}
System.out.println("Sum of evens: " + sumOfEvens); // Sum of evens: 30
```

**После Stream API:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

int sumOfEvens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .mapToInt(Integer::intValue)
    .sum();

System.out.println("Sum of evens: " + sumOfEvens); // Sum of evens: 30
```

### Пример 5: Сложная обработка данных

**До Stream API:**
```java
List<String> words = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

// Найти самые длинные слова, отсортированные по алфавиту
List<String> longWords = new ArrayList<>();
int maxLength = 0;

// Первый проход: найти максимальную длину
for (String word : words) {
    if (word.length() > maxLength) {
        maxLength = word.length();
    }
}

// Второй проход: собрать слова максимальной длины
for (String word : words) {
    if (word.length() == maxLength) {
        longWords.add(word);
    }
}

// Сортировка
Collections.sort(longWords);
System.out.println(longWords); // [elderberry]
```

**После Stream API:**
```java
List<String> words = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

List<String> longWords = words.stream()
    .collect(Collectors.groupingBy(
        String::length,
        TreeMap::new,
        Collectors.toList()
    ))
    .lastEntry()
    .getValue()
    .stream()
    .sorted()
    .collect(Collectors.toList());

System.out.println(longWords); // [elderberry]
```

**Или более читаемый вариант:**
```java
List<String> longWords = words.stream()
    .max(Comparator.comparingInt(String::length))
    .stream()
    .sorted()
    .collect(Collectors.toList());

System.out.println(longWords); // [elderberry]
```

### Пример 6: Статистика по числам

**До Stream API:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

int min = Integer.MAX_VALUE;
int max = Integer.MIN_VALUE;
int sum = 0;
long count = 0;

for (int number : numbers) {
    if (number < min) min = number;
    if (number > max) max = number;
    sum += number;
    count++;
}

double average = (double) sum / count;

System.out.println("Min: " + min);     // Min: 1
System.out.println("Max: " + max);     // Max: 10
System.out.println("Sum: " + sum);     // Sum: 55
System.out.println("Average: " + average); // Average: 5.5
```

**После Stream API:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

IntSummaryStatistics stats = numbers.stream()
    .mapToInt(Integer::intValue)
    .summaryStatistics();

System.out.println("Min: " + stats.getMin());     // Min: 1
System.out.println("Max: " + stats.getMax());     // Max: 10
System.out.println("Sum: " + stats.getSum());     // Sum: 55
System.out.println("Average: " + stats.getAverage()); // Average: 5.5
```

### Пример 7: Удаление дубликатов и сортировка

**До Stream API:**
```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 2, 5, 1, 8, 9);

// Удаление дубликатов
Set<Integer> uniqueNumbers = new HashSet<>(numbers);
List<Integer> sortedUniqueNumbers = new ArrayList<>(uniqueNumbers);
Collections.sort(sortedUniqueNumbers);

System.out.println(sortedUniqueNumbers); // [1, 2, 5, 8, 9]
```

**После Stream API:**
```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 2, 5, 1, 8, 9);

List<Integer> sortedUniqueNumbers = numbers.stream()
    .distinct()
    .sorted()
    .collect(Collectors.toList());

System.out.println(sortedUniqueNumbers); // [1, 2, 5, 8, 9]
```

## Ключевые преимущества Stream API:

### 1. **Читаемость и выразительность**
- Код описывает **ЧТО** сделать, а не **КАК**
- Цепочка методов читается как предложение на английском

### 2. **Меньше шаблонного кода**
- Нет необходимости в явных циклах
- Автоматическое управление временными коллекциями

### 3. **Легкая параллелизация**
```java
// Просто замените stream() на parallelStream()
List<String> result = names.parallelStream()
    .filter(name -> name.startsWith("A"))
    .map(String::toUpperCase)
    .collect(Collectors.toList());
```

### 4. **Ленивые вычисления**
Операции выполняются только когда действительно нужен результат

### 5. **Безопасность**
Stream API не изменяет исходные коллекции, создавая новые

### 6. **Упрощение сложных операций**
Сложные операции вроде группировки, статистики и агрегации становятся простыми однострочными выражениями

Stream API действительно изменило способ работы с коллекциями в Java, сделав код более чистым, читаемым и эффективным!

## Stream API на практике

### Создание потоков:

```java
// Из коллекции
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream1 = list.stream();

// Из массива
String[] array = {"a", "b", "c"};
Stream<String> stream2 = Arrays.stream(array);

// Из значений
Stream<String> stream3 = Stream.of("a", "b", "c");

// Бесконечный поток
Stream<Integer> numbers = Stream.iterate(0, n -> n + 1);
Stream<Double> randoms = Stream.generate(Math::random);
```

### Промежуточные операции (lazy):

```java
List<String> names = Arrays.asList("John", "Alice", "Bob", "Anna", "Mike", "Alex");

List<String> result = names.stream()
    .filter(name -> name.length() > 3)        // фильтрация
    .map(String::toUpperCase)                 // преобразование
    .sorted()                                 // сортировка
    .distinct()                               // удаление дубликатов
    .limit(5)                                 // ограничение количества
    .skip(1)                                  // пропуск первых элементов
    .collect(Collectors.toList());            // терминальная операция

System.out.println(result); // [ALEX, ALICE, ANNA, JOHN]
```

### Терминальные операции (eager):

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// forEach - выполнение действия для каждого элемента
numbers.stream().forEach(System.out::println);

// collect - сбор результатов
List<Integer> evenNumbers = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

// reduce - свертка
int sum = numbers.stream().reduce(0, Integer::sum);

// count - подсчет элементов
long count = numbers.stream().filter(n -> n > 5).count();

// anyMatch/allMatch/noneMatch - проверка условий
boolean hasEven = numbers.stream().anyMatch(n -> n % 2 == 0);
boolean allPositive = numbers.stream().allMatch(n -> n > 0);

// findFirst/findAny - поиск элементов
Optional<Integer> first = numbers.stream().findFirst();
```

## Более сложные примеры

### Работа с объектами:

```java
class Person {
    private String name;
    private int age;
    private String city;
    
    // конструктор, геттеры, сеттеры
}

List<Person> people = Arrays.asList(
    new Person("John", 25, "New York"),
    new Person("Alice", 30, "London"),
    new Person("Bob", 20, "Paris"),
    new Person("Anna", 35, "New York")
);

// Группировка по городу
Map<String, List<Person>> peopleByCity = people.stream()
    .collect(Collectors.groupingBy(Person::getCity));

// Средний возраст
double averageAge = people.stream()
    .collect(Collectors.averagingInt(Person::getAge));

// Самый старший человек
Optional<Person> oldest = people.stream()
    .max(Comparator.comparingInt(Person::getAge));

// Имена людей старше 25
List<String> names = people.stream()
    .filter(p -> p.getAge() > 25)
    .map(Person::getName)
    .collect(Collectors.toList());
```

### Параллельные потоки:

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// Последовательный поток
long startTime = System.currentTimeMillis();
int sequentialSum = numbers.stream()
    .mapToInt(Integer::intValue)
    .sum();
long sequentialTime = System.currentTimeMillis() - startTime;

// Параллельный поток
startTime = System.currentTimeMillis();
int parallelSum = numbers.parallelStream()
    .mapToInt(Integer::intValue)
    .sum();
long parallelTime = System.currentTimeMillis() - startTime;

System.out.println("Sequential: " + sequentialTime + "ms");
System.out.println("Parallel: " + parallelTime + "ms");
```

## Особенности и лучшие практики

### 1. **Потоки одноразовые**
```java
Stream<String> stream = Stream.of("a", "b", "c");
stream.forEach(System.out::println); // OK
stream.forEach(System.out::println); // IllegalStateException!
```

### 2. **Ленивые вычисления**
Промежуточные операции выполняются только при вызове терминальной операции.

### 3. **Не изменяют исходную коллекцию**
```java
List<String> original = Arrays.asList("a", "b", "c");
List<String> result = original.stream()
    .map(String::toUpperCase)
    .collect(Collectors.toList());

System.out.println(original); // [a, b, c] - не изменился
System.out.println(result);   // [A, B, C]
```

### 4. **Оптимизация с помощью short-circuit**
```java
List<String> names = Arrays.asList("John", "Alice", "Bob", "Anna");

// findFirst прекратит обработку после нахождения первого элемента
Optional<String> first = names.stream()
    .filter(name -> name.startsWith("A"))
    .findFirst();
```

### 5. **Работа с примитивами**
```java
IntStream.range(1, 10)                    // для int
LongStream.range(1, 100)                  // для long
DoubleStream.of(1.1, 2.2, 3.3)           // для double
```

## Заключение

Stream API revolutionized Java programming by:

- ✅ **Упрощение кода** - меньше шаблонного кода
- ✅ **Улучшение читаемости** - декларативный стиль
- ✅ **Легкая параллелизация** - автоматическое распараллеливание
- ✅ **Функциональный подход** - использование лямбда-выражений
- ✅ **Оптимизация** - ленивые вычисления и short-circuit операции

Stream API стало неотъемлемой частью современной Java-разработки и значительно упростило работу с коллекциями данных.