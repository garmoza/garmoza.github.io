+++ 
draft = false
date = 2024-01-16T14:05:23+03:00
title = "Соглашения о коде"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

## Отступы

Единица отступа = **4 пробела**.

### Для методов

- перенос после `,`
- выравнивание по `(`
- либо отступ в **8 пробелов**, если нет места

```java
// CONVENTIONAL INDENTATION
someMethod(int anArg, Object anotherArg, String yetAnotherArg,
           Object andStillAnother) {
    ...
}

// INDENT 8 SPACES TO AVOID VERY DEEP INDENTS
private static synchronized workingLongMethodName(int anArg,
        Object anotherArg, String yetAnotherArg,
        Object andStillAnother) {
    ...
}
```

```java
function(longExpression1, longExpression2, longExpression3,
         longExpression4, longExpression5);

var = function1(longExpression,
                function2(longExpression2,
                          longExpression3));

longName1 = longName2 * (longName3 + longName4 - longName5)
            + 4 * longName6 // PREFER

longName1 = longName2 * (longName3 + longName4
                         - longName5) + 4 * longName6 // AVOID
```

### Для `if`

- отступ в **8 пробелов**, если нет места

```java
// DON'T USE THIS INDENTATION
if ((condition1 && condition2)
    || (condition3 && condition4)
    ||!(condition5 && condition6)) { // BAD WRAPS
    doSomethingAboutIt();             // MAKE THIS LINE EASY TO MISS
}

// USE THIS INDENTATION INSTEAD
if ((condition1 && condition2)
        || (condition3 && condition4)
        ||!(condition5 && condition6)) {
    doSomethingAboutIt();
}
```

### Для тернарных выражений

```java
alpha = (aLongBooleanExpression) ? beta : gamma;

alpha = (aLongBooleanExpression) ? beta
                                 : gamma;

alpha = (aLongBooleanExpression)
        ? beta
        : gamma;
```

## Переменные

Переменные **должны объявляться в начале блока** (любой код окруженный `{}`).
При этом не должно возникать никаких трудностей с нахождением места обращения к переменной,
так как методы должны быть короткими.

```java
private static void readPreference() {
  InputStream is = null;
  try {
    is = new FileInputStream(getPreferencesFile());
    setPreferences(new Properties(getPreferences()));
    getPreferences().load(is);
  } catch (IOException e) {
    try {
      if (is != null) {
        is.close();
      }
    } catch (IOException el) {

    }
  }
}
```

Управляющие переменные циклов обычно внутри конструкции цикла

```java
public int countTestCases() {
  int count = 0;
  for (Test each : tests)
    count += each.countTestCases();
  return count;
}
```

Но существуют отдельные случаи

```java
for (XmlTest test : m_suite.getTests()) {
  TestRunner tr = runnerFactory.newTestRunner(this, test);
  // using TestRunner (tr)
}
```

## Комментарии

- **реализации** (Implementation Comment Format)
- **документации** (Documentation Comments)

Комментарии реализации предназначены для комментирования кода или комментариев о конкретной реализации (имеют C-style). Комментарии документации предназначены для описания спецификации кода с точки зрения реализации. для чтения разработчиками, у которых может не быть под рукой исходного кода (специально для описания Java API).

### Комментарии реализации

- **Block Comments**

Используется для комментирования логических частей программы, там, где не достаточно однострочного комментария.

```java
/*
 * Here is a block comment with some very special
 * formatting that I want indent(1) to ignore.
 *
 *      one
 *          two
 *              three
 */
```

- **Single-Line Comments**

Аналогичен **Block Comments**, только в одну строку.

```java
if (condition) {
  /* Handle the condition. */
  ...
}
```

- **Trailing Comments**

Для комментирования в той же строке.

```java
if (a == 2) {
  return true;        /* special case */
} else {
  return isPrime(a);  /* works only for odd a */
}
```

{{< notice note >}}
Если используются несколько комментариев для одного участка кода, то они должны иметь **одинаковые отступы**.
{{< /notice >}}

- **End-Of-Line Comments**

Используется **для комментирования следующей строки** (с новой линии). Не следует использовать для нескольких
последовательных строк.

```java
if (foo > 1) {
  // Do a double-flip.
  ...
} else
  return false; //  Explain why here.
```

### Комментарии документации

Описывают классы, интерфейсы, конструкторы, методы и поля для API. Один комментарий на каждую сущность.

```java
/**
 * The example class provides ...
 */
public class Example {

    /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * Constructs an empty object.
     */
    public Example() {
    }

    /**
     * Doc example for method.
     * More lines.
     */
    public void todoSomething() {
    }
}
```

{{< notice note >}}
Необходимо использовать отступ в один пробел после `*` для каждой строки.
{{< /notice >}}

**Links**: https://www.oracle.com/technetwork/java/codeconventions-150003.pdf
