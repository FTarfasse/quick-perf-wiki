The principle of a *threshold test* is to compare a value in the current build to a threshold. If this value exceeds the threshold, the test will fail and we are alerted of a possible performance degradation.<br>

In this [paper](https://martinfowler.com/bliki/ThresholdTest.html), Martin Fowler gives the example of the amount of time taken by a test.<br> 

When you set maximum heap size for a test with the help of [@HeapSize](https://github.com/quick-perf/doc/wiki/JVM-annotations#heapsize) or [@Xmx](https://github.com/quick-perf/doc/wiki/JVM-annotations#xmx), the test may fail one day because of an OutOfMemoryError. You may consider this as a performance issue or if not increase maximum heap size. So, fixing maximum heap size could be seen as a threshold test. A significant increase of heap allocation may also lead to a significant increase of the test time length because of garbage collection activity. So your build duration could increase, possibly alerting you of a significant increase of heap allocation for a test. [@ExpectNoJvmIssue](https://github.com/quick-perf/doc/wiki/JVM-annotations#ExpectNoJvmIssue) can help you to check that most of the test time is spent to garbage collect objects.