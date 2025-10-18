Отличная идея! Вот структурированный урок по Generics в Java, который вы можете использовать.

---

### Урок по Generics в Java

#### 1. Что такое Generics?

**Generics (обобщения)** — это механизм в Java, который позволяет создавать классы, интерфейсы и методы, работающие с типами, указанными в качестве параметров. Проще говоря, это способ сделать ваш код типобезопасным и многократно используемым, позволяя указывать конкретный тип данных, с которым будет работать класс или метод, во время его использования, а не во время написания.

Представьте себе контейнер. Без Generics он мог бы хранить `Object`, а значит, что угодно. С Generics мы можем сказать: "Этот контейнер предназначен только для String" или "только для Integer".

#### 2. Когда появились Generics?

Generics были добавлены в Java в **версии 5.0 (J2SE 5.0)**, которая вышла в 2004 году. До этого все коллекции (List, Set, Map) хранили элементы как `Object`, что приводило к необходимости частого приведения типов (casting) и потенциальным ошибкам во время выполнения (Runtime Exceptions).

#### 3. Какие проблемы решают Generics?

До появления Generics существовало три основные проблемы:

1.  **Отсутствие типобезопасности:**
    *   В коллекцию можно было положить объекты любого типа.
    *   Компилятор не проверял, какой именно тип вы извлекаете.

    ```java
    // Код до Java 5
    List list = new ArrayList();
    list.add("Привет");
    list.add(123); // Integer тоже можно добавить
    list.add(new MyCustomObject()); // И любой свой объект

    String str = (String) list.get(0); // Требуется явное приведение типа
    String str2 = (String) list.get(1); // ОШИБКА! ClassCastException во время выполнения!
    ```

2.  **Необходимость явного приведения типов (Casting):**
    *   При каждом извлечении объекта из коллекции приходилось вручную приводить его к нужному типу. Это делало код громоздким и неудобочитаемым.

3.  **Ошибки на этапе выполнения, а не компиляции:**
    *   Ошибка несоответствия типов (`ClassCastException`) возникала только когда программа уже работала, что затрудняло отладку и делало программы менее надежными.

**Generics решают эти проблемы, перенося проверку типов на этап компиляции.**

#### 4. Как использовать Generics? Основные концепции

##### a) Обобщенные классы

Создадим свой простой контейнер с использованием Generics.

```java
// T - это параметр типа (Type Parameter)
// Он является "заполнителем" для реального типа, который будет указан позже.
public class Box<T> {
    private T content;

    public void setContent(T content) {
        this.content = content;
    }

    public T getContent() {
        return content;
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        // Создаем Box для String. T заменяется на String
        Box<String> stringBox = new Box<>();
        stringBox.setContent("Строка внутри");
        // stringBox.setContent(123); // ОШИБКА КОМПИЛЯЦИИ! Нельзя положить Integer в Box<String>
        String content = stringBox.getContent(); // Приведение типа не требуется!

        // Создаем Box для Integer. T заменяется на Integer
        Box<Integer> integerBox = new Box<>();
        integerBox.setContent(100);
        int number = integerBox.getContent(); // Авто-распаковка (unboxing)
    }
}
```

##### b) Обобщенные методы

Методы также могут быть параметризованы. Параметр типа объявляется перед возвращаемым типом метода.

```java
public class Util {
    // Обобщенный статический метод
    public static <T> boolean isEqual(Box<T> box1, Box<T> box2) {
        return box1.getContent().equals(box2.getContent());
    }

    // Другой пример: метод для преобразования массива в список
    public static <E> List<E> fromArrayToList(E[] array) {
        List<E> list = new ArrayList<>();
        for (E element : array) {
            list.add(element);
        }
        return list;
    }
}
```

**Использование:**

```java
public class Main {
    public static void main(String[] args) {
        Box<String> box1 = new Box<>();
        box1.setContent("test");
        Box<String> box2 = new Box<>();
        box2.setContent("test");

        // Компилятор сам выводит тип T (String) из аргументов
        boolean result = Util.isEqual(box1, box2);
        System.out.println(result); // true

        // Работа с массивом
        Integer[] intArray = {1, 2, 3};
        List<Integer> intList = Util.fromArrayToList(intArray);
        System.out.println(intList); // [1, 2, 3]
    }
}
```

##### c) Ограниченные параметры типов (Bounded Type Parameters)

Иногда мы хотим ограничить типы, которые можно использовать в качестве аргумента типа. Например, только числами.

*   **Верхнее ограничение (Upper Bounded):** `<T extends Number>` - T может быть Number или его подклассом (Integer, Double и т.д.).
*   **Нижнее ограничение (Lower Bounded):** `<? super Integer>` - может быть Integer или его суперклассом (Number, Object).

**Пример с верхним ограничением:**

```java
public class NumberBox<T extends Number> {
    private T number;

    public NumberBox(T number) {
        this.number = number;
    }

    // Метод, который может работать с любым Number
    public double getSquare() {
        return number.doubleValue() * number.doubleValue();
    }
}
```

**Использование:**

```java
NumberBox<Integer> integerBox = new NumberBox<>(5);
System.out.println(integerBox.getSquare()); // 25.0

NumberBox<Double> doubleBox = new NumberBox<>(2.5);
System.out.println(doubleBox.getSquare()); // 6.25

// NumberBox<String> stringBox = new NumberBox<>("test"); // ОШИБКА КОМПИЛЯЦИИ! String не extends Number
```

##### d) Wildcards (Подстановочные знаки) - `?`

Wildcards используются, когда точный тип неизвестен или не важен. Они часто встречаются в API коллекций.

*   **`List<?>`** - список неизвестного типа (можно читать как `Object`, но нельзя добавлять элементы кроме `null`).
*   **`List<? extends Number>`** - список объектов, которые являются Number или его подклассами (только для чтения).
*   **`List<? super Integer>`** - список объектов, которые являются Integer или его суперклассами (можно добавлять Integer).

**Пример:**

```java
public static void printList(List<?> list) {
    for (Object elem : list) {
        System.out.print(elem + " ");
    }
    System.out.println();
}

public static double sumOfList(List<? extends Number> list) {
    double sum = 0.0;
    for (Number number : list) {
        sum += number.doubleValue();
    }
    return sum;
}
```

#### 5. Стирание типов (Type Erasure)

Важно понимать, что Generics в Java реализованы через **стирание типов**. Это значит, что информация о generic-типах удаляется во время компиляции и заменяется на "сырые" (raw) типы или границы (например, `Object` или `Number`).

*   `Box<String>` и `Box<Integer>` после компиляции становятся просто `Box` (сырой тип).
*   Параметр типа `T` внутри класса `Box` заменяется на `Object` (или на верхнюю границу, если она указана, например `T extends Number`).

Это сделано для обратной совместимости с кодом, написанным до Java 5.

**Следствие:** Вы **не можете** использовать `new T()` или `T.class`, так как во время выполнения тип `T` неизвестен.

---

### Резюме

*   **Generics** — это параметры типов для классов, интерфейсов и методов.
*   **Появились** в Java 5.
*   **Решают проблемы:** типобезопасность (ошибки на этапе компиляции), устранение явного приведения типов, улучшение читаемости кода.
*   **Основные концепции:** обобщенные классы/методы, ограниченные типы (`extends`, `super`), wildcards (`?`).
*   **Реализация:** через стирание типов (Type Erasure), что обеспечивает обратную совместимость.

Generics — это фундаментальная часть современного Java-программирования, и их понимание критически важно для написания надежного и поддерживаемого кода.