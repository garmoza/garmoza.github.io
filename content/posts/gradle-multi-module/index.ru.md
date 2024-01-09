+++ 
draft = false
date = 2024-01-09T17:56:34+03:00
title = "Gradle Multi Module"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

Чтобы в корневой проект включить подпроект, необходимо изменить только `settings.gradle`, добавив `include`. При этом дочерние `build.gradle` могут быть пустые, а дочерние `settings.gradle` вообще отсутствовать.

```groovy
rootProject.name = 'example'

include 'sub-project-1'
include 'sub-project-2'
```

Все конфигурации (plugins, dependencies и другое) по умолчанию применятся только для **текущего корневого проекта**.

Чтобы передать конфигурации в дочерние проекты можно использовать слова

- `project` - позволяет указать конфигурацию для конкретного дочернего проекта
- `subprojects` - для всех дочерних проектов
- `allprojects` - для корневого и дочерних проектов

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '2.5.2'
}

project ("sub-project-1") {
    apply plugin: 'java'
}

subprojects {
    apply plugin: 'java'
    apply plugin: 'org.springframework.boot'
}

allprojects {
    group 'com.garmoza'
    version '1.0-SNAPSHOT'

    java.sourceCompatibility = JavaVersion.VERSION_1_8

    repositories {
        mavenCentral()
    }

    def springBootVersions = '2.5.2'

    dependencies {
        implementation "org.springframework.boot:spring-boot-starter-web:$springBootVersions"
        implementation group: "org.springframework.boot", name: 'spring-boot-starter-data-jpa', version: springBootVersions

        annotationProcessor "org.projectlombok:lombok:1.18.28"
    }

    configurations {
        compileOnly.extendsFrom annotationProcessor
        testCompileOnly.extendsFrom annotationProcessor
        testAnnotationProcessor.extendsFrom annotationProcessor
    }

    test {
        useJUnitPlatform()
    }
}
```

### Sub-project as dependency

При импорте одного подпроекта в другой, необходимо указать его как зависимость (dependency).

При этом транзитивные зависимости должны быть определены с областью видимости `api`, чтобы быть доступны в проекте, который использует другой подпроект.
