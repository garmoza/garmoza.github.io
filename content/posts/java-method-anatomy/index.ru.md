+++ 
draft = true
date = 2024-02-07T15:04:27+03:00
title = "Анатомия Java-метода"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

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

**Links**:

- https://www.youtube.com/watch?v=wkHU5akk2po
