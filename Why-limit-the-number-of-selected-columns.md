Selected columns that you don't need can impact performances, particularly when you select all columns.<br>

Not selected evertyting can save memory on database and JVM sides and reduce IO between the JVM and the database.

In addition, unwanted columns can prevent some [index-only scans](https://use-the-index-luke.com/sql/clustering/index-only-scan-covering-index) that avoid table access, and so saves a lot of IO. [This can seriously degrade performances](https://use-the-index-luke.com/blog/2013-08/its-not-about-the-star-stupid).<br>

So, when you need some specific read-only data, as it could be the case with DTOs, it is recommended to project the needed columns. Examples can be found [here](https://github.com/AnghelLeonard/Hibernate-SpringBoot) to do this with JPA, Hibernate or Spring.