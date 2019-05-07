**QuickPerf** is a Java open source project (Apache License, Version 2.0) that provides annotations to quickly evaluate some performance properties. <br>

### [Twitter: @QuickPerf](https://twitter.com/quickperf)

# Run QuickPerf
## [JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)
## [JUnit 4 & Spring](https://github.com/quick-perf/doc/wiki/JUnit-4--&-Spring)

# Use QuickPerf annotations
## Annotation scopes
An annotation can have **three scopes** :
* **Global** <br>
An annotation defined at global level is applied on each test.<br>
You can define annotations with global levels by creating a class implementing SpecifiableAnnotations interface. This class has to be in org.quickperf package.
* **Class** <br>
An annotation defined at class level overrides the configuration of the same annotation defined at global level.
* **Method** <br>
An annotation defined at method level overrides the configuration of the same annotation defined at class and global levels.

**[Example to understand how annotation scopes work](https://github.com/quick-perf/doc/wiki/Example-to-understand-how-annotation-scopes-work)**

## [Base annotations](https://github.com/quick-perf/doc/wiki/base-annotations)
## [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations)
## [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations)

# Disable QuickPerf
To disable QuickPerf functionalities you can pass *-DdisableQuickPerf=true* to your JVM or use [annotations](https://github.com/quick-perf/doc/wiki/base-annotations).