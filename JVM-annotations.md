JVM annotations can help you to 
* *Design your code for performance* <br>
  Example: evaluate heap cost of a data structure
* *Investigate and improve performance with quick iterations on your computer*<br>
  Example: huge allocation in pre-production when a given functionality or a batch is called => reproduce the issue on your computer
            => apply quick performance iterations on your computer (investigate, apply code modification, assess code modification) until having acceptable allocation => re-test in pre-production
* *Add automatic tests verifying performance*<br>
Example: verify allocation of performance-sensitive functionnalities, check allocation to control financial cost of memory usage in cloud

*With JVM annotations, the test method is executed in a dedicated JVM.*

# Outline
[**Configure your test JVM**](#Configure-your-test-JVM) <br> @HeapSize, @Xms, @Xmx, @JvmOptions <br><br>
[**Verify heap allocation**](#Verify-heap-allocation)<br> @MeasureHeapAllocation, @ExpectMaxHeapAllocation, @ExpectNoHeapAllocation <br><br>
[**Profile or check your JVM**](#Profile-or-check-your-JVM) <br> @ProfileJvm, @ExpectNoJvmIssue

# Configure your test JVM
## @HeapSize
With this annotation, the test is executed in a specific JVM having the given heap size.
### Parameters 
|Parameter  |Type           | Meaning                  | Default value  |
| -------- |:--------------:|:------------------------:| :-------------:|
| value    | long           |Heap size value (Xms=Xmx) |        -       |
| unit     | AllocationUnit |Allocation unit           |        -       |
### Example
 ```java
   @HeapSize(value = 20, unit = AllocationUnit.MEGA_BYTE)
  ```
### Fixing maximum heap size as a threshold test
See [@Xmx](#Xmx).

## @Xms
### Parameters 
|Parameter  |Type           | Meaning                            | Default value |
| -------- |:--------------:|:----------------------------------:|:-------------:|
| value    | long           |Initial and minimum heap size value |        -      |
| unit     | AllocationUnit |Allocation unit                     |        -      |
### Example
With this annotation, the test is executed in a specific JVM having the given initial and minimum heap size value.
  ```java
   @Xms(value = 20, unit = AllocationUnit.MEGA_BYTE)
  ```
## @Xmx
With this annotation, the test is executed in a specific JVM having the given maximum heap size value.
### Parameters 
|Parameter  |Type           | Meaning                 | Default value |
| -------- |:--------------:|:-----------------------:|:-------------:|
| value    | long           |Maximum heap size value  |        -      |
| unit     | AllocationUnit |Allocation unit          |        -      |
### Example
  ```java
   @Xmx(value = 20, unit = AllocationUnit.MEGA_BYTE)
  ```
### Fixing maximum heap size as a threshold test
The principle of a *threshold test* is to compare a value in the current build to a threshold. If this value exceeds the threshold, the test will fail and we are alerted of a possible performance degradation. In this [paper](https://martinfowler.com/bliki/ThresholdTest.html), Martin Fowler gives the example of the amount of time taken by a test. When you set maximum heap size for a test, the test may fail one day because of an OutOfMemoryError. You may consider this as a performance issue or if not increase maximum heap size. So, fixing maximum heap size could be seen as a threshold test. A significant increase of heap allocation may also lead to a significant increase of the test time length because of garbage collection activity. So your build duration could increase, possibly alerting you of a significant increase of heap allocation for a test. [@ExpectNoJvmIssue](#ExpectNoJvmIssue) can help you to check that most of the test time is spent to garbage collect objects.

## @JvmOptions
With this annotation, the test is executed in a specific JVM having the given JVM options.
### Parameters 
|Parameter  |Type           | Meaning       | Default value |
| -------- |:--------------:|:-------------:|:-------------:|
| value    | String           |JVM options  |      -        |

# Verify heap allocation
The following annotations use ByteWatcher under the hood:
* https://github.com/danielshaya/ByteWatcher
* https://www.javaspecialists.eu/archive/Issue232.html

You can  for example use @MeasureHeapAllocation and @ExpectMaxHeapAllocation to check the heap allocation cost of a large data structure (containing 1 000 000 elements for example) .<br>

@ExpectNoHeapAllocation can be used to verify that the tested code does not allocate on heap.


## @MeasureHeapAllocation
You can measure allocation using this annotation. <br><br>
The measured allocation is displayed in the console.

### Example
```java
@RunWith(QuickPerfJUnitRunner.class)
public class ClassWithMethodAnnotatedWithMeasureAllocation {

    @MeasureHeapAllocation
    @JvmOptions("-XX:+UseCompressedOops -XX:+UseCompressedClassPointers")
    // Heap allocation value depends on UseCompressedOops and UseCompressedClassPointers.
    // QuickPerf works with JDK >= 7u40 where UseCompressedOops is enabled by default.
    // UseCompressedClassPointers was introduced in JDK 8 and is enabled by default.
    @Test
    public void array_list_with_size_100_should_allocate_440_bytes() {
        // java.util.ArrayList: 24 bytes
        //            +
        //  Object[]: 16 + 100 x 4 = 416
        //       = 440 bytes
        ArrayList<Object> data = new ArrayList<>(100);
    }

}
```
In console:
```
Measured heap allocation: 440.0 bytes
```
## @ExpectMaxHeapAllocation
With this annotation, the test will fail if heap allocation is greater than expected.

### Parameters 
|Parameter  |Type           | Meaning          | Default value  |
| -------- |:--------------:|:----------------:| :-------------:|
| value    | long           |Allocation value  |        -       |
| unit     | AllocationUnit |Allocation unit   |        -       |
### Example
 ```java
    @ExpectMaxHeapAllocation(value = 440, unit = AllocationUnit.BYTE)
    @Test
    public void array_list_with_size_100_should_allocate_440_bytes() {
        ArrayList<Object> data = new ArrayList<>(100);
    }
  ```
## @ExpectNoHeapAllocation
With this annotation, the test will fail if heap allocation is detected.

# Profile or check your JVM
The following annotations use *Java Flight Recorder* (JFR) under the hood. <br><br>
*JFR profiling works with Oracle JDK >= 1.7u40 and OpenJDK >= 11.*

## @ProfileJvm
To profile JVM with Java Flight Recorder (JFR).<br>

The JFR file location is shown in the console. You can open it with Java Mission Control.
<br>

```
[QUICK PERF] JVM was profiled with Java File Recorder (JFR).
The recording file can be found here: C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-46868616\jvm-profiling.jfr
You can open it with Java Mission Control (JMC).
```
## @ExpectNoJvmIssue

*Today we considrer this annotation as experimental.*

With this annotation, JVM is profiled with Java Flight Recorder (JFR).<br>

Based on the profiling, some [JMC rules](http://hirt.se/blog/?p=920) are evaluated. For each rule a score is attributed. The maximum score value is 100. The test will fail if one rule has a score greater than this expected (by default 60)<br><br> 
Things like significant primitives to object conversions can be detected:
<p align="center">
<img src="https://github.com/quick-perf/doc/blob/master/doc/images/JMC-PrimitiveToObjectConversion.PNG" width="944" heigth="191"></p>
With this annotation you can also detect that most of the time is spent to do garbage collection in your test.

### Parameters 
|Parameter  |Type           | Meaning           | Default value |
| -------- |:--------------:|:-----------------:|:-------------:|
| score    | int            |Rule score (<=100) |      60       |