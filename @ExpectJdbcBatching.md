Verify that inserts, deletes and updates are processed in JDBC batches having *batchSize* elements.
You may sometimes think that you are using JDBC batching but in fact not ([Paper 1](https://abramsm.wordpress.com/2008/04/23/hibernate-batch-processing-why-you-may-not-be-using-it-even-if-you-think-you-are/), [Paper 2](https://stackoverflow.com/questions/27697810/hibernate-disabled-insert-batching-when-using-an-identity-identifier))!
*Batching  of inserts, updates and deletes allows to reduce the number of [roundtrips to the database which can dramatically impact application performance](https://blog.jooq.org/2017/12/18/the-cost-of-jdbc-server-roundtrips/).*
You can decide to batch all inserts, updates, delete. Prior to Hibernate 5.2, batching, when enabled with a hibernate.jdbc.batch_size property stricly positive, was applied to all inserts, updates and deletes (from Hibernate 5.2 it is also possible to override the batch size value for a given session).
### :wrench: Parameters 
|Parameter  |Type| Meaning           | Default value  |
| -------- |:---:|:-----------------:|:--------------:|
| batchSize| int |JDBC batch size   |      -         |

_batchSize is optional._

A 0 batch size means that JDBC batching is disabled.

### :mag_right: Example
```java
    @ExpectJdbcBatching(batchSize = 30)
```
