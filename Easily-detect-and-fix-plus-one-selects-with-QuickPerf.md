*N+1* select antipattern can lead to many JDBC roundtrips in production.

With QuickPerf you can easily detect and fix this antipattern from your tests.

# What is an N+1 select?

##First example##

##Second example##

# Easily detect N+1 selects with QuickPerf

You can detect N+1 select by adding [@ExpectSelect](./@ExpectSelect) annotation on a test method to check the number of executed SELECT statements.



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