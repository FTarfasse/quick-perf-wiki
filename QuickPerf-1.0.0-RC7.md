
# 🚀 New features and improvements

## Core
* Add execution time annotations: [@MeasureExecutionTime](https://github.com/quick-perf/doc/wiki/core-annotations#measureexecutiontime) and [@ExpectMaxExecutionTime](https://github.com/quick-perf/doc/wiki/core-annotations#expectmaxexecutiontime) @jeanbisutti
* Add [`CoreAnnotationBuilder` to configure core annotations with a global scope](https://github.com/quick-perf/doc/wiki/core-annotations#configure-core-annotations-with-a-global-scope) @jeanbisutti

## JVM

* Add [@UseGC](https://github.com/quick-perf/doc/wiki/JVM-annotations#usegc) @jeanbisutti
* Add [@EnableGcLogging](https://github.com/quick-perf/doc/wiki/JVM-annotations#enablegclogging) @jeanbisutti 
* Add [heap dumper](https://github.com/quick-perf/doc/wiki/JVM-annotations#heapdumper) @jeanbisutti
* Add Resident Set Size (RSS) annotations: [@MeasureRSS](https://github.com/quick-perf/doc/wiki/JVM-annotations#measurerss) and [@ExpectMaxRSS](https://github.com/quick-perf/doc/wiki/JVM-annotations#expectmaxrss) @loicmathieu 
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