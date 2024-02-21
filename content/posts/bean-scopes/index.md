+++ 
draft = false
date = 2024-02-21T10:13:19+03:00
title = "Bean Scopes"
description = ""
slug = ""
authors = ["Nikita Garmoza"]
tags = []
categories = []
externalLink = ""
series = []
+++

Scope can be specified in the `xml` configuration as an attribute of bean (`scope`), or using the `@Scope` annotation.

- **singleton** - Scopes a single bean definition to a single object in instance per Spring ApplicationContext.
- **prototype** - Scopes a single bean definition to any number of object instances.
- **request** - Scopes a single bean definition to the lifecycle of a single HTTP request; that is, each and every HTTP request will have its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring ApplicationContext.
- **session** - Scopes a single bean definition to the lifecycle of a HTTP session. Only valid in the context of a web-aware Spring ApplicationContext.
- **global session** - Scopes a single bean definition to the lifecycle of a global HTTP session. Typically only valid when used in a portlet context. Only valid in the context of a web-aware Spring ApplicationContext.

{{< notice note >}}
`globalSession` doesn't have any special effect different from the `session` scope in Servlet based application.
{{< /notice >}}
