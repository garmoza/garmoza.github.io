+++ 
draft = false
date = 2024-02-20T16:14:05+03:00
title = "Collection"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

{{< notice note >}}
Только новые методы (не наследуемые из `Iterable` или `Object`).
{{< /notice >}}

### Query Operations

```java
int size();
boolean isEmpty();
boolean contains(Object o);
Object[] toArray();
<T> T[] toArray(T[] a);
```

### Modification Operations

```java
boolean add(E e);
boolean remove(Object o);
```

### Bulk Operations

```java
boolean containsAll(Collection<?> c);
boolean addAll(Collection<? extends E> c);
boolean removeAll(Collection<?> c);
default boolean removeIf(Predicate<? super E> filter) {}
boolean retainAll(Collection<?> c);
void clear();
```

{{< notice warning >}}
`remove()/removeAll()`, `contains()/containsAll()`, `retainAll()` принимают в качестве параметра `Object`, что может привести к багам в коде из-за передачи другого типа (никогда не удаляется элемент, всегда отсутствует). При этом `add()/addAll()` в этом плане более безопасны.
{{< /notice >}}

### Stream API

```java
default Stream<E> stream() {}
default Stream<E> parallelStream() {}
```
