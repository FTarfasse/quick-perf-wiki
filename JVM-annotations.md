# 🚩 Table of contents
[One JVM by test method](#One-JVM-by-test-method) <br>

[Configure your test JVM](#Configure-your-test-JVM) <br>
&nbsp;  &nbsp; [@HeapSize](#heapsize) &nbsp;|&nbsp; [@Xms](#xms) &nbsp;|&nbsp;[@Xmx](#xmx) &nbsp;|&nbsp; [@UseGC (Next release)](#usegc) &nbsp;|&nbsp; [@JvmOptions](#jvmoptions)

[Verify heap allocation](#Verify-heap-allocation) <br>
&nbsp;  &nbsp; [@MeasureHeapAllocation](#measureheapallocation) &nbsp;|&nbsp;[@ExpectMaxHeapAllocation](#expectmaxheapallocation) &nbsp;|&nbsp; [@ExpectNoHeapAllocation](#expectnoheapallocation)

[Verify RSS](#Verify-rss) <br>
&nbsp;  &nbsp; [@MeasureRSS (Next release)](#measurerss) &nbsp;|&nbsp; [@ExpectMaxRSS (Next release)](#expectmaxrss)

[Profile or check your JVM](#Profile-or-check-your-JVM) <br>
&nbsp;  &nbsp; [@ProfileJvm](#profilejvm) &nbsp;|&nbsp;[@ExpectNoJvmIssue](#expectnojvmissue)

[Test examples](#Test-examples)

# One JVM by test method
⚠️ *If you use one of the JVM annotations, the test method is executed in a dedicated JVM.*

# Configure your test JVM
## @HeapSize
With this annotation, the test is executed in a specific JVM having the given heap size.
### :wrench: Parameters 
|Parameter  |Type           | Meaning                  |
| -------- |:--------------:|:------------------------:|
| value    | long           |Heap size value (Xms=Xmx) |
| unit     | AllocationUnit |Allocation unit           |
### :mag_right: Example
 ```java
   @HeapSize(value = 20, unit = AllocationUnit.MEGA_BYTE)
  ```

### :bulb: [Fixing maximum heap size as a threshold test](https://github.com/quick-perf/doc/wiki/Fixing-maximum-heap-size-as-a-threshold-test)

## @Xms
With this annotation, the test is executed in a specific JVM having the given initial and minimum heap size value.
### :wrench: Parameters 
|Parameter  |Type           | Meaning                            |
| -------- |:--------------:|:----------------------------------:|
| value    | long           |Initial and minimum heap size value |
| unit     | AllocationUnit |Allocation unit                     |
### :mag_right: Example
  ```java
   @Xms(value = 20, unit = AllocationUnit.MEGA_BYTE)
  ```
## @Xmx
With this annotation, the test is executed in a specific JVM having the given maximum heap size value.
### :wrench: Parameters 
|Parameter  |Type           | Meaning                 |
| -------- |:--------------:|:-----------------------:|
| value    | long           |Maximum heap size value  |
| unit     | AllocationUnit |Allocation unit          |
### :mag_right: Example
  ```java
   @Xmx(value = 20, unit = AllocationUnit.MEGA_BYTE)
  ```

### :bulb [Fixing maximum heap size as a threshold test](https://github.com/quick-perf/doc/wiki/Fixing-maximum-heap-size-as-a-threshold-test)

## @UseGC 
_**Available in next QuickPerf release**_

To specify a GC type.

### :wrench: Parameters 
|Parameter |Type                       | Meaning    | Default value |
| -------- |:-------------------------:|:----------:|:-------------:|
| value    | org.quickperf.jvm.gc.GC   |GC type     | GC.DEFAULT    |


The following GC types are available:
* GC.EPSILON_GC:  [Epsilon GC - Doc 1](https://openjdk.java.net/jeps/318), [Epsilon GC - Doc 2](https://shipilev.net/jvm/diy-gc/#_epsilon_gc)
* GC.ZGC: [ZGC - Doc](https://wiki.openjdk.java.net/display/zgc/Main)
* GC.SHENANDOAH: [Shenandoah - Doc](https://wiki.openjdk.java.net/display/shenandoah/Main)

### :mag_right: Example
```java
@UseGC(GC.EPSILON_GC)
```

## @JvmOptions
With this annotation, the test is executed in a specific JVM having the given JVM options.

A [tool](https://chriswhocodes.com/vm-options-explorer.html) developed by [Chris Newland](https://github.com/chriswhocodes) can be used to explore the available JVM options.

### :wrench: Parameters 
|Parameter  |Type           | Meaning       |
| -------- |:--------------:|:-------------:|
| value    | String           |JVM options  |

# Verify heap allocation
The following annotations use ByteWatcher under the hood:
* https://github.com/danielshaya/ByteWatcher
* https://www.javaspecialists.eu/archive/Issue232.html

*They measure heap allocation of the thread running the method annotated @Test*.

You can  for example use @MeasureHeapAllocation and @ExpectMaxHeapAllocation to check the heap allocation cost of a large data structure (containing 1 000 000 elements for example) .<br>

@ExpectNoHeapAllocation can be used to verify that the tested code does not allocate on heap.


## @MeasureHeapAllocation
You can measure allocation using this annotation. <br><br>
The measured allocation is displayed in the console.

### :mag_right: Example
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

### :wrench: Parameters 
|Parameter  |Type           | Meaning          | 
| -------- |:--------------:|:----------------:|
| value    | long           |Allocation value  |   
| unit     | AllocationUnit |Allocation unit   |

### :mag_right: Example
 ```java
    @ExpectMaxHeapAllocation(value = 440, unit = AllocationUnit.BYTE)
    @Test
    public void array_list_with_size_100_should_allocate_440_bytes() {
        ArrayList<Object> data = new ArrayList<>(100);
    }
  ```
## @ExpectNoHeapAllocation
With this annotation, the test will fail if heap allocation is detected.


# Verify RSS

## @MeasureRSS
_**Available in next QuickPerf release**_

## @ExpectMaxRSS
_**Available in next QuickPerf release**_

### :wrench: Parameters 
|Parameter  |Type           | Meaning          | 
| -------- |:--------------:|:----------------:|
| value    | long           |Allocation value  |   
| unit     | AllocationUnit |Allocation unit   |

# Profile or check your JVM
The following annotations use *Java Flight Recorder* (JFR) under the hood. <br><br>
*JFR profiling works with Oracle JDK >= 1.7u40 and OpenJDK >= 11.*

## @ProfileJvm
To profile JVM with Java Flight Recorder (JFR).<br>

The JFR file location is shown in the console. You can open it with Java Mission Control.
<br>
### :mag_right: Example
```
[QUICK PERF] JVM was profiled with Java File Recorder (JFR).
The recording file can be found here: C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-46868616\jvm-profiling.jfr
You can open it with Java Mission Control (JMC).
```

### :bulb: Where to find Java Mission Control (JMC)?
A _jmc_ executable is available in the _bin _folder of some Oracle JDK (10   >= version >= 1.7u40).

The last JMC release can be found [here](https://adoptopenjdk.net/jmc.html).

## @ExpectNoJvmIssue

*Today we consider this annotation as experimental.*

With this annotation, JVM is profiled with Java Flight Recorder (JFR).<br>

Based on the profiling, some [JMC rules](http://hirt.se/blog/?p=920) are evaluated. For each rule a score is attributed. The maximum score value is 100. The test will fail if one rule has a score greater than this expected (by default 60)<br><br> 
Things like significant primitives to object conversions can be detected:
<p align="center">
<img src="https://github.com/quick-perf/doc/blob/master/doc/images/JMC-PrimitiveToObjectConversion.PNG" width="944" heigth="191"></p>

:bulb: With this annotation you can also detect that most of the time is spent to do garbage collection in your test.

:bulb: If you have the following message in the console
```
Rule: Stackdepth Setting
Severity: WARNING
Score: 97
Message: Some stack traces were truncated in this recording.
```
then you can increase the stack depth value in this way:
```java
@JvmOptions("-XX:FlightRecorderOptions=stackdepth=128")
```

### :wrench: Parameters 
|Parameter  |Type           | Meaning           | Default value |
| -------- |:--------------:|:-----------------:|:-------------:|
| score    | int            |Rule score (<=100) |      60       |

# Test examples
[With JUnit 4](https://github.com/quick-perf/quickperf-examples/tree/master/jvm-junit4)<br><br>
[With JUnit 5](https://github.com/quick-perf/quickperf-examples/tree/master/jvm-junit5)<br><br>
[With TestNG](https://github.com/quick-perf/quickperf-examples/tree/master/jvm-testng)