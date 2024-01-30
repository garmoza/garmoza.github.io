+++ 
draft = false
date = 2024-01-30T10:26:58+03:00
title = "Java Ввод-вывод"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

## Потоковый ввод-вывод (java.io)

Представлен **абстрактными классами**:

- `InputStream` - читать байты.
- `OutputStream` - писать байты.
- `Reader` - читать символы.
- `Writer` - писать символы.

## InputStream

- `int read()` -> **-1 (конец потока) или 0..255** - читает ровно один байт. Блокируется в месте вызова - ждет, пока не появятся данные (например, при работе с сетью).
- `int read(byte[] b)` -> **-1 или кол-во прочитанных байт** - читает баты в массив в соответствии с его размером. Блокируется только один раз - если данных меньше, чем размер массива, то прочитает только доступные и выйдет (не будет ждать остальное).
- `int read(byte[] b, int offset, int length)` -> **-1 или кол-во прочитанных байт** - аналогично, для подмассива.

#### Вычитать всё в массив заданного размера

```java
static void readFully(InputStream is, byte[] b) throws IOException {
    int offset = 0;
    while (offset < b.length) {
        int count = is.read(b, offset, b.length - offset);
        if (count == -1) {
            throw new IOException("Stream has less than " + b.length + " bytes");
        }
        offset += count;
    }
}

public static void main(String[] args) throws IOException {
    byte[] b = new byte[100];
    readFully(System.in, b);
    System.out.println(Arrays.toString(b));
}
```

{{< notice note >}}
Считывает информацию из командной строки.
{{< /notice >}}

- `long skip(long n)` -> **кол-во пропущенных байт** - пропускает первые `n` байт, не читая их (быстрее). Блокируется только один раз.
- `int available()` -> **кол-во байт, доступных без блокировки** - необязательный в реализации - может всегда возвращать ноль, хотя данные будут доступны.
- `void mark(int readlimit)` - используется для того, чтобы прочитать определенное кол-во байт вперед, а после вернуться. Используется вместе с `reset`. Например, для определения сигнатуры файла в заголовке.
- `void reset()` - возвращает в место вызова `mark`.
- `boolean markSupported()` - если поток поддерживает маркировку - `mark`.

#### Читает, пока не прочитает нужное кол-во. Блокируются несколько раз

- `byte[] readAllBytes()` -> **все содержимое**.
- `byte[] readNBytes(int len)` -> **не больше len байт**.
- `int readNBytes(byte[] b, int offset, int length)` -> **кол-во прочитанных байт** - до ошибки или конца потока.
- `void skipNBytes(long n)` - пропускает `n` байт (`EOFException`, если меньше).

#### Другое

- `long transferTo(OutputStream out)` - перекачивает в выходной поток.
- `static InputStream nullInputStream()` -> **пустой поток**.
- `close()`

## InputStream реализации

- `FileInputStream` - файл. Лучше обворачивать в `BufferedInputStream`.
- `ByteArrayInputStream` - массив байт.
- `BufferedInputStream` - буферизированная обёртка.
- `DataInputStream` - структурированные двоичные данные. Содержит вспомогательные методы для чтения `int`, `double`, `String` и т.д. Для чтения собственного двоичного формата файлов. Для записи используется `DataOutputStream`.
- `PipedInputStream` - плохой, негодный.
- `ZipFile.getInputStream(ZipEntry)` - файл из zip (без распаковки).
- `Process.getInputStream()` - стандартный вывод процесса.
- `URL.openStream()` - содержимое по URL (http/https/file/ftp/etc.). Лучше избегать `URL` и использовать объекты `URI`, так как первые делаю DNS запросы при вызове `equals`/`hashCode`.
- `...`

## OutputStream

- `void write(int b)` - пишет 8 младших бит.
- `void write(byte[] b)` - пишет весь массив (если не упал, то всё записал).
- `void write(byte[] b, int offset, int length)` - часть массива.
- `void flush()` - сбрасывает кеши в точку назначения.
- `static OutputStream nullOutputStream()` - пустой поток, заглушка.
- `close()`

## OutputStream реализации

- `FileOutputStream` - файл.
- `ByteArrayOutputStream` - массива байт.
- `BufferedOutputStream` - буферизированная обёртка.
- `PrintStream` - для удобства печати (`System.out`).
- `DataOutputStream` - структурированные двоичные данные.
- `PipedOutputStream` - плохой, негодный.
- `Process.getOutputStream()` - стандартный ввод процесса.
- `URL.opeConnection().getOutputStream()` - писать в URL (HTTP POST).
- `...`

## Reader

Аналогичен `InputStream`, только для символов с небольшими отличиями.

- `int read()` -> **-1 или 0..0xFFFF** - считывает символ. Требуется проверка при преобразовании `int` в `char`, т.к. `-1` можно представить как `0xFFFF`.
- `int read(char[] cb)` -> **кол-во прочитанных символов**
- `int read(char[] cb, int offset, int length)` -> **кол-во прочитанных символов**
- `long skip(long n)` -> **кол-во пропущенных символов** - работает меленее своего аналога из `InputStream`, так как требуется прочитать символы (декодировать).
- `boolean ready()`
- `void mark(int readlimit)`
- `void reset()`
- `boolean markSupported()`
- `long transferTo(Writer out)`
- `static Reader nullReader()`
- `close()`

## Reader реализации

- `InputStreamReader` - InputStream + charset.
- `FileReader`
  - Java 11: FileReader(file, charset)
  - Java 18: UTF-8 by default
- `StringReader` - работает со строкой.
- `BufferedReader` - добавляет чтение до конца строки (`readLine(), lines()`).
- `...`

## Writer

- `void write(int b)` - пишет 16 младших бит.
- `void write(char[] cb)` - пишет весь массив (если не упал, то всё записал).
- `void write(char[] cb, int offset, int length)` - часть массива.
- `void write(String str)` - пишет всю строку (если не упал, то всё записал).
- `void write(String str, int offset, int length)` - часть строки.
- `void flush()`
- `static Writer nullWriter()`
- `close()`

## Writer реализации

- `OutputStreamWriter` - OutputStream + charset.
- `FileWriter`
  - Java 11: FileWriter(file, charset)
  - Java 18: UTF-8 by default
- `StringWriter`
- `BufferedWriter`
- `...`

## Управление внешними ресурсами

- **Закрыть явно**. `close()`. Всегда лучший вариант.
- **Положиться на Garbage Сollector** (PhantomReference, Cleaner). Если не известно, когда закончится работа с объектом.

#### Закрытие при помощи `close()`

Можно вызывать `close()` в секции `final` из `try-catch-final`, но
многие действия могут привести к выбрасыванию исключений, которые нужно обрабатывать:

- Открытие файла - `new FileOutputStream("/etc/passwd.bak")`.
- Закрытие файла - `close()`.
- Другие действия над файлом/потоком.

Вместо `try-catch-final` **лучше использовать** `try-with-resources`. При этом ресурсы будут закрыты в порядке их открывания. Исключения корректно отработаны.

```java
try (InputStream in = new FileInputStream("/etc/passwd");
     OutputStream out = new FileOutputStream("/etc/passwd.bak")) {
    in.transferTo(out);
}
```

{{< notice note >}}
При использовании `try-catch-final` нужно следить за последовательностью закрытия потоков. А также за правильной обработкой исключений (если используется несколько потоков, то порой нужно использовать двойные вложенные `try-catch-final`).
{{< /notice >}}

#### Закрытие при помощи GC

```java
import java.lang.ref.Cleaner;

public class MyFile implements AutoCloseable {

	private static final Cleaner CLEANER = Cleaner.create();
	private final int fd;
	private final Cleaner.Cleanable cleanable;

	public MyFile(int _fd) {
		this.fd = _fd;
		cleanable = CLEANER.register(this, () -> free(_fd));
	}

	private static void free(int fd) {
		System.out.println("Freeing file descriptor: " + fd);
	}

	public void read() {
		System.out.println("Reading from file descriptor: " + fd);
	}

	@Override
	public void close() {
		cleanable.clean();
	}
}
```

{{< notice note >}}
Нельзя `() -> free(fd)`, так как захватывается поле класса, а не значение.
{{< /notice >}}

```java
public static void main(String[] args) throws InterruptedException {
    readFile();
    System.out.println("GC");
    System.gc();
    System.out.println("Sleep");
    Thread.sleep(1000);
    System.out.println("Done");
}

private static void readFile() {
    MyFile f = new MyFile(1);
    f.read();
}
```

**Вывод**

```
Reading from file descriptor: 1
GC
Sleep
Freeing file descriptor: 1
Done
```

**Links**:

- https://www.youtube.com/live/801qM5vrYdc?si=7tWBBKe_Sn8L3NTx
