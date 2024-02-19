+++ 
draft = false
date = 2024-02-19T22:48:01+03:00
title = "Символы (char) Java"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

### Литерал

Все символы Java в формате **Unicode** и занимают 2 байта.

```java
char c = 'a';
char c = 'a' + 1;
char c = '\u65e5';
char c = '\\';
char c = '\n';
```

{{< notice note >}}
**Суррогатные пары** (один символ из двух 16-разрядных кодовых единиц), не могут быть представлены в `char`

```java
char cake = '🍰'; // not working
String cake = "\uD83C\uDF70"; // it's OK
cake.length() == 2; // length in char!
```

{{< /notice >}}

### Некоторые термины Unicode

- Code point - номер символа (0-0x10FFFF = 1114111)
- Basic Multilingual Plane (BMP) - символы 0-65535 (`\uFFFF`), "плоскость 0"
- Supplementary Plane - плоскости 1-16 (0x10000-0x10FFFF)
- Surrogate code point - 0xD800-0xDFFF
  - High surrogate code point - 0xD800-0xDBFF
  - Low surrogate code point - 0xDC00-0xDFFF
- Code unit - минимальная последовательность бит для представления кодовых точек в заданной кодировке
  - UTF-8: 1 байт, 0..255
  - UTF-16: 2 байта, 0..65535 (Java!)
  - UTF-32: 4 байта

### Операции над символами

```java
Character.toUpperCase();
Character.toTitleCase();
Character.isAlphabetic();
Character.isDigit();
Character.getNumericValue();
...
```

**Links**:

- https://www.youtube.com/watch?v=wkHU5akk2po
