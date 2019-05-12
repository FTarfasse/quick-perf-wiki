**QuickPerf** is a Java open source project (Apache License, Version 2.0) that provides annotations to quickly evaluate some performance properties. <br>

### [Twitter: @QuickPerf](https://twitter.com/quickperf)

# Run QuickPerf
## [JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)
## [JUnit 4 & Spring](https://github.com/quick-perf/doc/wiki/JUnit-4--&-Spring)

# Use QuickPerf annotations
## Annotation scopes
An annotation can have three **scopes**:
* **Global** <br>
An annotation having global scope is applied on each test.<br>
You can define annotations with global scope by creating a class implementing SpecifiableAnnotations interface. This class has to be in org.quickperf package.
* **Test class** <br>
An annotation having a test class scope overrides the configuration of the same annotation with global scope.
* **Test method** <br>
An annotation having a test method scope overrides the configuration of the same annotation with test class and global scopes.

**[Example illustrating how annotation scopes work](https://github.com/quick-perf/doc/wiki/Example-to-understand-how-annotation-scopes-work)**

## [Base annotations](https://github.com/quick-perf/doc/wiki/base-annotations)
## [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations)
[**Configure your test JVM**](JVM-annotations#Configure-your-test-JVM)<br><br>
[**Accurately verify heap allocation**](JVM-annotations#Accurately-verify-heap-allocation)<br><br>
[**Profile or check your JVM**](JVM-annotations#Profile-or-check-your-JVM)
## [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations)
Easily detect **N+1 select**, bad use of Hibernate session and other things.

# Disable QuickPerf
To disable QuickPerf functionalities you can pass *-DdisableQuickPerf=true* to your JVM or use [annotations](https://github.com/quick-perf/doc/wiki/base-annotations).