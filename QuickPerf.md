<div align="center">
<img src="https://pbs.twimg.com/profile_banners/926219963333038086/1518645789" alt="QuickPerf"/>
</div><br>

>***QuickPerf is a testing library for Java providing annotations to quickly evaluate some performance properties.***

<br>

**QuickPerf works with JUnit 4, JUnit 5 and a JDK 1.7+.** <br>

:octocat: Github repository [here](https://github.com/quick-perf/quickperf)

# Execute QuickPerf
## [With JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)
## [With JUnit 5](https://github.com/quick-perf/doc/wiki/JUnit-5)
## [With TestNG](https://github.com/quick-perf/doc/wiki/TestNG)
## [With Spring](https://github.com/quick-perf/doc/wiki/Spring)
## Have clickable links in your IDE
Sometimes, QuickPerf can display a web url in the console. It is useful to have a clickable web url in your IDE, to be able to open it by a simple click.

_Example of clickable web url_
<p align="center"><img src="https://github.com/quick-perf/doc/blob/master/doc/images/Web_ressource.PNG"></p>

### IntelliJ
Web URL are clickable since IntelliJ 2019.

For oldest versions, you can use [Awesome console](https://plugins.jetbrains.com/plugin/7677-awesome-console) plugin.

💡 If links are displayed on more one line, uncheck _Use soft wraps in console_ (Editor -> General -> Console).

# Use QuickPerf annotations
## Annotation scopes
An annotation can have three scopes:
* **Global scope** <br>
An annotation having a global scope applies on each test.<br>
You can define annotations with global scope by creating a class implementing SpecifiableGlobalAnnotations interface. This class has to be in org.quickperf package.
* **Test class scope** <br>
An annotation having a test class scope overrides the configuration of the same annotation with global scope.
* **Test method scope** <br>
An annotation having a test method scope overrides the configuration of the same annotation with test class and global scopes.

**[Example illustrating how annotation scopes work](https://github.com/quick-perf/doc/wiki/Example-illustrating-how-annotation-scopes-work)**

## [Core annotations](https://github.com/quick-perf/doc/wiki/core-annotations)
## [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations)
[**Configure your test JVM**](JVM-annotations#Configure-your-test-JVM)<br><br>
[**Verify heap allocation**](JVM-annotations#Verify-heap-allocation)<br><br>
[**Profile or check your JVM**](JVM-annotations#Profile-or-check-your-JVM)
## [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations)
Easily detect **N+1 select**, JDBC batching disabled and other things.

# Disable QuickPerf
To disable QuickPerf features you can pass *-DdisableQuickPerf=true* to your JVM or use [some core annotations](https://github.com/quick-perf/doc/wiki/core-annotations) (@DisableQuickPerf, @FunctionalIteration, 
@DisableGlobalAnnotations).

# Project examples
[Maven performance](https://github.com/quick-perf/maven-test-bench)<br><br>
[Spring Boot - JUnit 4](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit4)<br><br>
[Spring Boot - JUnit 5](https://github.com/quick-perf/quickperf-examples/tree/master/springboot-junit5)<br><br>
[Micronaut - JUnit 5](https://github.com/quick-perf/quickperf-examples/tree/master/micronaut-hibernate-jpa)