+++ 
draft = false
date = 2024-01-09T19:57:08+03:00
title = "Hibernate Checklist"
description = ""
slug = ""
authors = ["Никита Гармоза"]
tags = []
categories = []
externalLink = ""
series = []
+++

- **примитивные типы**, где это возможно (+ нет необходимости в null значениях)
- **id для каждой сущности**, как отдельное поле
- **инкремент id** при помощи `@SequenceGenerator` и `@GeneratedValue`
- не использовать `@Data` и `@ToString`
- **инициализировать коллекции**
- **двусторонние отношения**
- везде **LAZY**
- везде `@Column` и `@Table`

## Примеры

### Инкремент id

```java
@Id
@SequenceGenerator(name = "generator", sequenceName = "user_id_seq")
@GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "generator")
@Column(name = "id")
private long id;
```

### Инициализация коллекций, двусторонние отношения, LAZY

{{< tabgroup >}}
{{< tab name="User entity" >}}

```java
@Builder.Default
@ElementCollection(fetch = FetchType.LAZY)
private Set<String> authorities = new HashSet<>();
@Builder.Default
@OneToMany(fetch = FetchType.LAZY, mappedBy = "author")
private Set<Task> createdTasks = new HashSet<>();
```

{{< /tab >}}

{{< tab name="Task entity" >}}

```java
@ManyToOne(fetch = FetchType.LAZY, optional = false)
@JoinColumn(name = "author_id")
private User author
```

{{< /tab >}}
{{< /tabgroup >}}

**Links**: https://habr.com/ru/articles/679216/
