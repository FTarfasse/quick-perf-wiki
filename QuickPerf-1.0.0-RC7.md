
# 🚀 New features and improvements

## Core
* Add execution time annotations: [@MeasureExecutionTime](https://github.com/quick-perf/doc/wiki/core-annotations#measureexecutiontime) and [@ExpectMaxExecutionTime](https://github.com/quick-perf/doc/wiki/core-annotations#expectmaxexecutiontime) @jeanbisutti
* Add [`CoreAnnotationBuilder` to configure core annotations with a global scope](https://github.com/quick-perf/doc/wiki/core-annotations#configure-core-annotations-with-a-global-scope) @jeanbisutti

## JVM
* @ProfileJvm also displays some JVM profiling data (GC times, heap allocation estimation, exception numbers, ...) in standard output. @jeanbisutti & edwardrose946 (allocation rate) & UbaidurRehman1 (young and old gc counts)

Example:
```
------------------------------------------------------------------------------
 ALLOCATION (estimations)     |   GARBAGE COLLECTION           |  THROWABLE
 Total       : 3,68 GiB       |   Total pause     : 1,264 s    |  Exception: 0
 Inside TLAB : 3,67 GiB       |   Longest GC pause: 206,519 ms |  Error    : 36
 Outside TLAB: 12,7 MiB       |   Young: 13                    |  Throwable: 36
 Allocation rate: 108.1 MiB/s |   Old  : 3                     |
------------------------------------------------------------------------------
 COMPILATION                  |   CODE CACHE
 Number : 157                 |   The number of full code cache events: 0
 Longest: 1,615 s             |   
------------------------------------------------------------------------------
```
* Add [@UseGC](https://github.com/quick-perf/doc/wiki/JVM-annotations#usegc) @jeanbisutti
* Add [@EnableGcLogging](https://github.com/quick-perf/doc/wiki/JVM-annotations#enablegclogging) @jeanbisutti 
* Add [heap dumper](https://github.com/quick-perf/doc/wiki/JVM-annotations#heapdumper) @jeanbisutti
* Add Resident Set Size (RSS) annotations: [@MeasureRSS](https://github.com/quick-perf/doc/wiki/JVM-annotations#measurerss) and [@ExpectMaxRSS](https://github.com/quick-perf/doc/wiki/JVM-annotations#expectmaxrss) @loicmathieu 
* Improve allocation display @jeanbisutti  
* Improve reporting in case of JVM issue @jeanbisutti 
* Display information on where to find JMC in console @jeanbisutti 

## SQL

## JUnit 5
* Display an error message if JUnit 5 version is less than 5.6.0  @jeanbisutti 


## Build
* 


# Improvements


# 🐛 Bug fixes

## JVM
* QuickPerf did not work when a not serializable throwable was thrown and the test was executed in a dedicated  JVM @loicmathieu (report) &  @jeanbisutti (fix)

## SQL
*  Should not suspect N+1 select if less than two select statements are executed @jeanbisutti 

#  ⚠️ Breaking change