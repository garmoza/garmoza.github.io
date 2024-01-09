+++ 
draft = false
date = 2024-01-09T04:09:37+03:00
title = "Maven Multi Module"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

В проекте состоящем из нескольких модулей, корневой pom.xml (главный, родительский) требует

- `<packaging>pom</packaging>`
- перечислить дочерние модули в `<modules>`

`reactor` - родительский pom.xml

{{< tabgroup >}}
{{< tab name="parent pom.xml" >}}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.garmoza</groupId>
    <artifactId>example</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>pom</packaging>

    <modules>
        <module>sub-project-1</module>
        <module>sub-project-2</module>
    </modules>

    <!-- something -->

</project>
```

{{< /tab >}}

{{< tab name="sub-project" >}}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>com.garmoza</groupId>
        <artifactId>example</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>sub-project-2</artifactId>

    <!-- something -->

</project>
```

{{< /tab >}}
{{< /tabgroup >}}

### Dependencies

Если в реакторе указаны зависимости внутри `<dependencies>`, то **все дочерние модули** наследуют их, и **добавляют в выполняемые файлы, даже если не используют их**. При этом у каждого модуля могут быть свои собственные зависимости

Чтобы уменьшить размер исполняемых файлов, а также хранить информацию о зависимостях в одном месте (в родительском pom.xml) используется `<dependencyManagement>`. Когда зависимость нужна в дочернем модуле, она добавляется как обычно, только без указания версии

{{< tabgroup >}}
{{< tab name="parent pom.xml" >}}

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

{{< /tab >}}

{{< tab name="sub-project" >}}

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
    </dependency>
</dependencies>
```

{{< /tab >}}
{{< /tabgroup >}}

### BOM

**B**ill **o**f **M**aterials - pom.xml который может быть импортирован в другой pom.xml

### Sub-project as dependency

Чтобы модуль мог использовать другой модуль как зависимость, необходимо добавить его в `<dependencies>`

{{< tabgroup >}}
{{< tab name="sub-project-1 pom.xml" >}}

```xml
<dependencies>
    <dependency>
        <groupId>com.garmoza</groupId>
        <artifactId>sub-project-2</artifactId>
    </dependency>
</dependencies>
```

{{< /tab >}}
{{< /tabgroup >}}

При этом зависимости с видимостями `compile` и `runtime` (scopes) доступны, а `test`, `provided` нет
