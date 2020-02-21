*N+1* select antipattern can lead to many JDBC roundtrips in production.

With QuickPerf you can easily detect and fix this antipattern from your tests.

# What is an N+1 select?

## N+1 select coming from an eager fetch type

## N+1 select with a lazy fetch type...

# Easily detect N+1 selects with QuickPerf

You can detect N+1 select by adding ***[@ExpectSelect](./@ExpectSelect) annotation on a test method*** to check the number of executed SELECT statements.

The outcome of an N+1 select is to have the same SELECT statements with different values. You can systematically detect this by configuring ***@DisableSameSelectTypesWithDifferentParams annotation with a [global scope](https://github.com/quick-perf/doc/wiki/QuickPerf#annotation-scopes)***. In the previous examples, the outcome of the N+1 select was to have additional SELECT statements on Team table. These additional SELECT statements are the same apart from the id value of the Team table.

Hibernate code examples are available to play with these two ways of detecting N+1 selects: <br>
:point_right: &nbsp; [Hibernate JUnit 4 code example](https://github.com/quick-perf/quickperf-examples/blob/master/hibernate-junit4/src/test/java/org/quickperf/sql/HibernateJUnit4Test.java) <br>
:point_right: &nbsp; [Hibernate JUnit 5 code example](https://github.com/quick-perf/quickperf-examples/blob/master/hibernate-junit5/src/test/java/org/quickperf/sql/HibernateJUnit5Test.java) <br>
:point_right: &nbsp; [Hibernate TestNG code example](https://github.com/quick-perf/quickperf-examples/blob/master/hibernate-testng/src/test/java/org/quickperf/sql/HibernateTestNGTest.java) <br>

The use of QuickPerf to detect N+1 selects in a pring Boot, a Quarkus or a Micronaut application is demonstrated in [this repository](https://github.com/quick-perf/quickperf-examples).

# Easily fix N+1 selects with QuickPerf

QuickPerf automatically detects that the tested code uses _Hibernate_ or _Spring Data JPA_. When a test is failing possibly due to an N+1 select, QuickPerf proposes ways to fix an N+1 select with _Hibernate_ or _Spring Data JPA_:

```
Perhaps you are facing a N+1 select issue
	* With Hibernate, you may fix it by using JOIN FETCH
	                                       or LEFT JOIN FETCH
	                                       or FetchType.LAZY
	                                       or ...
```
```
	* With Spring Data JPA, you may fix it by adding
		@EntityGraph(attributePaths = { "..." }) on repository method.
```