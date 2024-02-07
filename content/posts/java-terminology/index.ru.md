+++ 
draft = true
date = 2024-02-07T15:04:27+03:00
title = "Терминология Java"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

# Класс

**Fields** (поля) - свойства класса.

**Methods** (методы) - поведение.

**Attributes** - fields + methods.

# Метод

Рассмотрим вызов `main` в качестве примера:

```java
public static void main(String[] args) {
  System.out.println("Hello World!");
}
```

## Code block (блок кода)

```java
{
  System.out.println("Hello World!");
}
```

## Statement (оператор, предложение, инструкция)

```java
System.out.println("Hello World!");
```

## Expression (выражение)

Вызов метода

```java
System.out.println("Hello World!")
```

Ссылка на поле; квалификатор вызова

```java
System.out
```

Аргумент метода

```java
"Hello World!"
```

{{< notice tip >}}
Все **expression** возвращают значение (за исключением некоторых, которые возвращают `void`).
{{< /notice >}}

## Аргумент и параметр

Рассмотрим на примере:

```java
public int sum(int a, int b) {
  return a + b;
}

public static void main(String[] args) {
  int value = 1;
  sum(value, 2);
}
```

**Argument** - значение, которое указывается при вызове метода (`value`, `2`).

**Parameter** - переменная/значение внутри метода (`a`, `b`).

Для обобщенных типов:

```java
class Gen<T> {}

Gen<Integer> object;
```

**Type argument** - `Integer`.

**Type parameter** - `T` (placeholder).

**Links**:

- https://www.youtube.com/watch?v=wkHU5akk2po
