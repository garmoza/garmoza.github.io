+++ 
draft = false
date = 2024-02-20T17:20:34+03:00
title = "List"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

Коллекция упорядоченных элементов (добавляются индексы).

`List<E> extends Collection<E>`.

### Bulk Operations

```java
boolean addAll(int index, Collection<? extends E> c);
default void sort(Comparator<? super E> c);
```

### Positional Access Operations

```java
E get(int index);
E set(int index, E element);
void add(int index, E element);
E remove(int index); // boolean remove(Object o);
```

### Search Operations

```java
int indexOf(Object o);
int lastIndexOf(Object o);
```

### List Iterators

```java
ListIterator<E> listIterator();
ListIterator<E> listIterator(int index);
```

### View

```java
List<E> subList(int fromIndex, int toIndex);
```

{{< notice note >}}
`hashCode()` реализован как

```java
int hashCode = 1;
for (E e : list)
  hashCode = 31*hashCode + (e==null ? 0 : e.hashCode());
```

{{< /notice >}}
