+++ 
draft = false
date = 2024-02-20T01:07:27+03:00
title = "Spring Transaction Propagation"
description = ""
slug = ""
authors = ["Nikita Garmoza"]
tags = []
categories = []
externalLink = ""
series = []
+++

`org.springframework.transaction.annotation.Propagation`

- **REQUIRED**
  - If a transaction is in progress, the execution will continue within that transaction.
  - Otherwise, a new transaction will be created. REQUIRED is the **default propagation** for transactions in Spring.
- **SUPPORTS**
  - If a transaction is in progress, the execution will continue within that transaction.
  - Otherwise, no transaction will be created.
- **MANDATORY**
  - If a transaction is in progress, the execution will continue within that transaction.
  - Otherwise, a `TransactionRequiredException` will be thrown.
- **REQUIRES_NEW**
  - If a transaction is in progress, it will be suspended and a new transaction will be started.
  - Otherwise, a new transaction will be created anyway.
- **NOT_SUPPORTED**
  - If a transaction is in progress, it will be suspended and a non-transactional execution will continue.
  - Otherwise, the execution will simply continue.
- **NEVER**
  - If a transaction is in progress, an `IllegalTransactionStateException` will be thrown.
  - Otherwise, the execution will simply continue.
- **NESTED**
  - If a transaction is in progress, a subtransaction of this one will be created, and at the same time a savepoint will be created. If the subtransaction fails, the execution will rollback to this savepoint.
  - If no transaction was originally in progress, a new transaction will be created.
