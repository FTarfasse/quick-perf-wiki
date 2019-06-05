Selected columns that you don't need can impact performances, particularly when you select all columns.<br>

Unwanted columns can prevent some [index-only scans](https://use-the-index-luke.com/sql/clustering/index-only-scan-covering-index) that avoid table access, and so saves a lot of IO. [This can seriously degrade performances](https://use-the-index-luke.com/blog/2013-08/its-not-about-the-star-stupid).<br>

In addition, [according to Markus Winand](https://use-the-index-luke.com/blog/2013-04/the-two-top-performance-problems-caused-by-ORM-tools): "Besides Index-Only Scans, not selecting everything can also improve sorting, grouping and join performance because the database can save memory that way."<br>

We can add that reducing selected columns to what you need can reduce the memory presssure on JVM side together with the IO between the JVM and the database.<br>

So, when you need some specific read-only data, as it could be the case with DTOs, it is recommended to project the needed columns. Examples can be found [here](https://github.com/AnghelLeonard/Hibernate-SpringBoot) to do this with JPA, Hibernate or Spring.