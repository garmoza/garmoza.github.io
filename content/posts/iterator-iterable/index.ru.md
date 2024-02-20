+++ 
draft = false
date = 2024-02-20T02:47:29+03:00
title = "Интерфейс Iterator, Iterable"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

### Iterator

Позволяет обходить коллекции.

```java
boolean hasNext();
E next();
default void remove() { throw new UnsupportedOperationException("remove"); }
```

{{< notice note >}}
Метод `remove()` не обязан быть реализован. Или может специально выбрасывать `UnsupportedOperationException`, как для **unmodifiable** коллекций.
{{< /notice >}}

### Iterable

Расширение данного интерфейса позволяет объекту быть целью **for-each** (цикла).

{{< notice note >}}
`Collection<E> extends Iterable<E>`
{{< /notice >}}

```java
void printAll(Iterable<?> iterable) {
  for (Object obj : iterable) {
    System.out.println(obj);
  }
}

// внутри байт-кода
void printAll(Iterable<?> iterable) {
  Iterator<?> iterator = iterable.iterator();
  while (iterator.hasNext()) {
    Object object = iterator.next();
    System.out.println(obj);
  }
}
```

Интерфейс `Iterable` требует реализовать только метод `iterator()`, поэтому можно
использовать лямбда-выражение

```java
static <T> Iterable<T> nCopies(T value, int count) throws IllegalAccessException {
	if (count < 0) {
		throw new IllegalAccessException("Negative count: " + count);
	}
	return () -> new Iterator<T>() {
		int rest = count;

		@Override
		public boolean hasNext() {
			return rest > 0;
		}

		@Override
		public T next() {
			if (rest == 0) {
				throw new NoSuchElementException();
			}
			rest--;
			return value;
		}
	};
}
```

{{< notice note >}}
Объект `Iterable` может быть **неизменяемым**, при этом `Iterator` **всегда изменяемый** (разве что если пустой).
{{< /notice >}}
