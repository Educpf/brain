---
title: "Databases"
tags: ["database"]
draft: false
---

# Server(less) database


Serverless were created in order to handle serverless compute, which are stateless.

### Comparation

| Traditional                 | Serverless                                |
| :-------------------------- | :---------------------------------------- |
| One long running process    | Computes spin ups only when query arrives |
| Always consumes CPU and RAM | Can scale to zero                         |
| Fixed hardware size         | Can scale to zero                         |
| Limited connection count    | Uses proxy + pooling                      |

## Polling

Because serverless functions are short-lived, each query could open a new database connection instead of reusing a fixed set like in traditional apps. Too many simultaneous connections can overload the database. A connection pool(pooler) manages and limits the number of active connections, distributing queries efficiently and ensuring the database stays stable.


