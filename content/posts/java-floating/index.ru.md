+++ 
draft = false
date = 2024-02-19T23:13:33+03:00
title = "Числа с плавающей точкой Java"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

Стандарт **IEEE 754** - число хранится в виде **мантиссы и порядка**:

- **мантисса** - значащие цифры
- **порядок** - показатель степени

Или S-E-F:

- **S** - знак
- **E** - показатель степени
- **F** - дробная часть

### `float` (4 байта)

| MIN_VALUE            | MAX_VALUE           | Класс-обёртка | Мантисса | Порядок |
| -------------------- | ------------------- | ------------- | -------- | ------- |
| $1.4 \cdot 10^{-45}$ | $3.4 \cdot 10^{38}$ | Float         | 23       | 8       |

| Поля  | Размеры полей |
| ----- | ------------- |
| S-E-F | 1-8-23        |

### `double` (8 байта)

| MIN_VALUE             | MAX_VALUE              | Класс-обёртка | Мантисса | Порядок |
| --------------------- | ---------------------- | ------------- | -------- | ------- |
| $4.9 \cdot 10^{-324}$ | $1.798 \cdot 10^{308}$ | Double        | 52       | 11      |

| Поля  | Размеры полей |
| ----- | ------------- |
| S-E-F | 1-11-52       |

{{< notice note >}}
Операция унарный минус **поддерживается для всех значений**.

```java
-Double.MIN_VALUE != Double.MAX_VALUE
Double.MAX_VALUE = 1.7976931348623157E308
Double.MIN_VALUE = 4.9E-324
-Double.MAX_VALUE = -1.7976931348623157E308
-Double.MIN_VALUE = -4.9E-324
```

{{< /notice >}}

50 знаков мантиссы, следовательно до $2^{50}$ влезают без потери точности.

### Особые числа

- `+0.0`, `-0.0` - равны по `==`, но различаются по `toString()`
- **Infinity** (`Double.POSITIVE_INFINITY`)
  - Больше всякого другого числа, положительное
  - `1 / Infinity = 0.0`, `Infinity / C = Infinity`
  - `Infinity + 1 = Infinity`, `Infinity + Infinity = Infinity`
  - `Infinity - Infinity = NaN`, `Infinity / Infinity = NaN`
  - `C / 0.0 = Infinity`
- **-Infinity** (`Double.NEGATIVE_INFINITY`)
  - Меньше всякого другого числа, отрицательное
  - `1 / -Infinity = -0.0`
- **NaN** (`Double.NaN`)
  - Не больше, не меньше и не равно никакому числу (в том числе себе)
  - Любая операция с NaN даст NaN (черная дыра)
  - `0.0 / 0.0 = NaN`

```java
-Double.POSITIVE_INFINITY == Double.NEGATIVE_INFINITY
```

### Операции над вещественными числами

- Унарный минус `-`
- Сложение `+`, `+=`
- Вычитание `-`, `-=`
- Умножение `*`, `*=`
- Деление `/`, `/=`, деление на 0: **Double.Infinity/Double.NaN**
- Остаток `%`, `%=`, деление на 0: **Double.NaN**
- Инкремент `++`
- Декремент `--`

### Полезные штуки

```java
Double.isNaN(), isFinite(), isInfinite(), doubleToLongBits()
Long.longBitsToDouble()
Math.sin(), cosh(), acos(), atan2(), pow(), floor(), ceil(), round(), log1p(), toRadians()
Math.ulp(), nextUp(), nextDown()
```

**Links**:

- https://www.youtube.com/watch?v=wkHU5akk2po
- https://ru.wikipedia.org/wiki/%D0%A7%D0%B8%D1%81%D0%BB%D0%BE_%D1%81_%D0%BF%D0%BB%D0%B0%D0%B2%D0%B0%D1%8E%D1%89%D0%B5%D0%B9_%D0%B7%D0%B0%D0%BF%D1%8F%D1%82%D0%BE%D0%B9
