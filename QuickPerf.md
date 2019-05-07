**QuickPerf** is a Java open source project (Apache License, Version 2.0) that provides annotations to quickly evaluate some performance properties. <br>

### [Twitter: @QuickPerf](https://twitter.com/quickperf)

# Run QuickPerf
## [JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)
## [JUnit 4 & Spring](https://github.com/quick-perf/doc/wiki/JUnit-4--&-Spring)

# Use QuickPerf annotations
## Annotation scopes
An annotation can have three scopes :
* Global
* Class
* Method

Each annotation can be applied at *method level*, at *class level* or *for each test*. <br>
An annotation configured at method level overrides the configuration of this annotation defined at class level. <br>
An annotation configured for each test is overridden by the same annotation defined at class level or method level.

[Example to understand how annotation scopes work](https://github.com/quick-perf/doc/wiki/Example-to-understand-how-annotation-scopes-work) 

## [Base annotations](https://github.com/quick-perf/doc/wiki/base-annotations)
## [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations)
## [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations)