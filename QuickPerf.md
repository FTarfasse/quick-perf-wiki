**QuickPerf** is a test Java open source library (Apache License, Version 2.0) that provides annotations to quickly evaluate some performance properties. <br><br>
QuickPerf works with a JDK 1.7+.

### [Twitter: @QuickPerf](https://twitter.com/quickperf)

# Why use QuickPerf

> Premature optimization is the root of all evil 

 Daniel Knuth

But...
>The real problem is that programmers have spent far too much time worrying about efficiency in the wrong places and at the wrong times; premature optimization is the root of all evil (or at least most of it) in programming.

Daniel Knuth 

> We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.

Daniel Knuth

<br>
<p><img src="https://github.com/quick-perf/doc/blob/master/doc/images/Tweet_tpierrain.PNG" width="60%" heigth="60%"></p>

<br>
<p>
<img src="https://github.com/quick-perf/doc/blob/master/doc/images/Tweet_mjpt777.PNG" width="60%" heigth="60%"></p>

Easy to use (based on annotations)
<br>
Works with existing tests
<br>
Works with Spring and Spring Boot tests

# Run QuickPerf
## [JUnit 4](https://github.com/quick-perf/doc/wiki/JUnit-4)
## [JUnit 4 & Spring](https://github.com/quick-perf/doc/wiki/JUnit-4--&-Spring)
## Have clickable links in your IDE
Sometimes, QuickPerf gives web or file ressources in console.
It is useful to have a plugin in your IDE allowing to open these links by a simple click.
In IntelliJ, you can for example use [Awesome console](https://plugins.jetbrains.com/plugin/7677-awesome-console).
# Use QuickPerf annotations
## Annotation scopes
An annotation can have three scopes:
* **Global scope** <br>
An annotation having global scope is applied on each test.<br>
You can define annotations with global scope by creating a class implementing SpecifiableAnnotations interface. This class has to be in org.quickperf package.
* **Test class scope** <br>
An annotation having a test class scope overrides the configuration of the same annotation with global scope.
* **Test method scope** <br>
An annotation having a test method scope overrides the configuration of the same annotation with test class and global scopes.

**[Example illustrating how annotation scopes work](https://github.com/quick-perf/doc/wiki/Example-to-understand-how-annotation-scopes-work)**

## [Base annotations](https://github.com/quick-perf/doc/wiki/base-annotations)
## [JVM annotations](https://github.com/quick-perf/doc/wiki/JVM-annotations)
[**Configure your test JVM**](JVM-annotations#Configure-your-test-JVM)<br><br>
[**Verify heap allocation**](JVM-annotations#Verify-heap-allocation)<br><br>
[**Profile or check your JVM**](JVM-annotations#Profile-or-check-your-JVM)
## [SQL annotations](https://github.com/quick-perf/doc/wiki/SQL-annotations)
Easily detect **N+1 select**, JDBC batching disabled and other things.

# Disable QuickPerf
To disable QuickPerf functionalities you can pass *-DdisableQuickPerf=true* to your JVM or use [annotations](https://github.com/quick-perf/doc/wiki/base-annotations) (@DisableQuickPerf, @FunctionalIteration, 
@DisableGlobalAnnotations).