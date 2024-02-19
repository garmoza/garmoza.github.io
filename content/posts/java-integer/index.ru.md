+++ 
draft = false
date = 2024-02-19T21:07:47+03:00
title = "Целые числа Java"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

{{< notice note >}}
Все целочисленные типы **знаковые**, кроме `char`.
{{< /notice >}}

### `byte` (1 байт)

| MIN_VALUE     | MAX_VALUE       | Класс-обёртка |
| ------------- | --------------- | ------------- |
| -128 = $-2^7$ | 127 = $2^7 - 1$ | Byte          |

### `short` (2 байта)

| MIN_VALUE           | MAX_VALUE             | Класс-обёртка |
| ------------------- | --------------------- | ------------- |
| -32 768 = $-2^{15}$ | 32 767 = $2^{15} - 1$ | Short         |

### `char` (2 байта)

| MIN_VALUE | MAX_VALUE             | Класс-обёртка |
| --------- | --------------------- | ------------- |
| 0         | 65 535 = $2^{16} - 1$ | Character     |

### `int` (4 байта)

| MIN_VALUE                  | MAX_VALUE                    | Класс-обёртка |
| -------------------------- | ---------------------------- | ------------- |
| -2 147 483 648 = $-2^{31}$ | 2 147 483 647 = $2^{31} - 1$ | Integer       |

### `long` (8 байт)

| MIN_VALUE                    | MAX_VALUE                      | Класс-обёртка |
| ---------------------------- | ------------------------------ | ------------- |
| $-9 \cdot 10^{18} = -2^{63}$ | $9 \cdot 10^{18} = 2^{63} - 1$ | Long          |

{{< notice note >}}
Операция унарный минус **не определена для MIN_VALUE**.

```java
-Integer.MIN_VALUE == Integer.MIN_VALUE == -2147483648
Math.abs(Integer.MIN_VALUE) == -2147483648
"polygenelubricants".hashCode() == -2147483648
```

{{< /notice >}}

### Операции над целыми числами

- Унарный минус `-`
- Сложение `+`, `+=`
- Вычитание `-`, `-=`
- Умножение `*`, `*=`
- Деление `/`, `/=`, деление на 0: **ArithmeticException**
- Остаток `%`, `%=`, деление на 0: **ArithmeticException**
- Инкремент `++`
- Декремент `--`

```java
-Integer.MIN_VALUE == Integer.MIN_VALUE == -2147483648
Math.abs(Integer.MIN_VALUE) == -2147483648
"polygenelubricants".hashCode() == -2147483648
```

{{< notice note >}}
Операция остатка `%` для отрицательных значений, работает **аналогично положительными**. Знак результата зависит только **от левого операнда**.

```java
-7 % 4 == -3
-7 % -4 == -3
7 % -4 == 3
```

{{< /notice >}}

### Выбросить `ArithmeticException` при переполнении

```java
Math.addExact();
Math.subtractExact();
Math.multiplyExact();
Math.incrementExact();
Math.decrementExact();
Math.negateExact();
```

### Полезные штуки

```java
Math.abs();
Math.max();
Math.min();
Integer.parseInt();
Integer.parseUnsignedInt();
Integer.signum();
```
