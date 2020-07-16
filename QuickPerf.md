<div align="center">
<img src="https://pbs.twimg.com/profile_banners/926219963333038086/1518645789" alt="QuickPerf"/>
</div><br>

>***QuickPerf is a testing library for Java to quickly evaluate and improve some performance properties.***

---
<p align="center">
  <a href="https://search.maven.org/search?q=org.quickperf">
    <img src="https://maven-badges.herokuapp.com/maven-central/org.quickperf/quick-perf/badge.svg"
         alt="Maven Central">
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/quick-perf/quickperf/blob/master/LICENSE.txt">
    <img src="https://img.shields.io/badge/license-Apache2-blue.svg"
         alt = "License">
  </a>
  &nbsp;&nbsp;
  <a href="https://twitter.com/quickperf">
      <img src="https://img.shields.io/twitter/follow/QuickPerf.svg?label=Follow%20%40QuickPerf&style=social"
           alt = "Twitter Follow">
  </a>  
  &nbsp;&nbsp;
  <a href="https://gitter.im/quickperf">
     <img src="https://img.shields.io/gitter/room/quick-perf/quickperf?color=orange"
          alt = "Gitter">
  </a>
  &nbsp;&nbsp;
  <a href="https://github.com/quick-perf/quickperf">
  :octocat:
  </a>
<p align="center">

---

**QuickPerf works with JUnit 4, JUnit 5, TestNG and a JDK 1.7+.** <br>

# Execute QuickPerf
## [With JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)
## [With JUnit 5](https://github.com/quick-perf/doc/wiki/JUnit-5)
## [With TestNG](https://github.com/quick-perf/doc/wiki/TestNG)
## [With Spring](https://github.com/quick-perf/doc/wiki/Spring)
## [With Micronaut (*experimental*)](https://github.com/quick-perf/quickperf-examples#micronaut)
## [With Quarkus (*experimental*)](https://github.com/quick-perf/quickperf-examples#quarkus)
## [Have clickable links in your IDE](https://github.com/quick-perf/doc/wiki/Have-clickable-links-in-your-IDE)

# Use QuickPerf features
## Annotation scopes
An annotation can have three scopes:
* **Global scope** <br>
The annotation applies on each [QuickPerf test](https://github.com/quick-perf/doc/wiki/QuickPerf-test).<br>
You can configure global scope annotations with a class implementing `SpecifiableGlobalAnnotations`interface and located in `org.quickperf` package. `CoreAnnotationBuilder`, `SqlAnnotationBuilder` and `JvmAnnotationBuilder`are available to help to build global scope annotations.
* **Test scope** <br>
The annotation is used on the test class. It overrides the configuration of the same annotation with global scope.
* **Test method scope** <br>
The annotation is used on the test method. It overrides the configuration of the same annotation with test class and global scopes.

💡 **[Examples illustrating how annotation scopes work](https://github.com/quick-perf/doc/wiki/Example-illustrating-how-annotation-scopes-work)**

## [Core](https://github.com/quick-perf/doc/wiki/core-annotations)
Execution time, debugging, ...
## [JVM](https://github.com/quick-perf/doc/wiki/JVM-annotations)
Heap allocation, profiling, ...
## [SQL](https://github.com/quick-perf/doc/wiki/SQL-annotations)
Easily [**detect N+1 select**](https://github.com/quick-perf/doc/wiki/Easily-detect-and-fix-N-plus-One-SELECT-with-QuickPerf), JDBC batching disabled, ...

# Disable QuickPerf
To disable QuickPerf features you can pass *-DdisableQuickPerf=true* to your JVM or use [some core annotations](https://github.com/quick-perf/doc/wiki/core-annotations) (@DisableQuickPerf, @FunctionalIteration, 
@DisableGlobalAnnotations).

# Project examples
[Maven performance](https://github.com/quick-perf/maven-test-bench)<br><br>
[QuickPerf examples (*JUnit 4*, *JUnit 5*, *TestNG*, *Hibernate*, *Spring*, *Spring Boot*, *Micronaut*, *Quarkus*, *...*)](https://github.com/quick-perf/quickperf-examples)