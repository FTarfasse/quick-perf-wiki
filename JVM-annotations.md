# 🚩 Table of contents
[One JVM by test method](#One-JVM-by-test-method) <br>

[How to get the JVM options added by QuickPerf?](#how-to-get-the-jvm-options-added-by-quickperf)

[Configure your test JVM](#Configure-your-test-JVM) <br>
&nbsp;  &nbsp; [@HeapSize](#heapsize) &nbsp;|&nbsp; [@Xms](#xms) &nbsp;|&nbsp;[@Xmx](#xmx) &nbsp;|&nbsp; [@UseGC (*Next release*)](#usegc)  &nbsp;|&nbsp; [@EnableGcLogging (*Next release*)](#enablegclogging) &nbsp;|&nbsp; [@JvmOptions](#jvmoptions)

[Verify heap allocation](#Verify-heap-allocation) <br>
&nbsp;  &nbsp; [@MeasureHeapAllocation](#measureheapallocation) &nbsp;|&nbsp;[@ExpectMaxHeapAllocation](#expectmaxheapallocation) &nbsp;|&nbsp; [@ExpectNoHeapAllocation](#expectnoheapallocation)

[Verify resident set size (RSS)](#verify-resident-set-size-rss) <br>
&nbsp;  &nbsp; [@MeasureRSS (*Next release*)](#measurerss) &nbsp;|&nbsp; [@ExpectMaxRSS (*Next release*)](#expectmaxrss)

[Profile or check your JVM](#Profile-or-check-your-JVM) <br>
&nbsp;  &nbsp; [@ProfileJvm](#profilejvm) &nbsp;|&nbsp; [@DisplayJvmProfilingValue (*Next release*)](#displayjvmprofilingvalue) &nbsp;|&nbsp;[@ExpectNoJvmIssue](#expectnojvmissue)

[Test examples](#Test-examples)

# One JVM by test method
⚠️ *If you use one of the JVM annotations, the test method is executed in a dedicated JVM.*

# How to get the JVM options added by QuickPerf?
Some JVM annotations configure JVM options.

You can use *@DebugQuickPerf* to get the JVM options added by QuickPerf.

### :mag_right: Example
```java
 @DebugQuickPerf
```

```
JVM OPTIONS
-Xms20m
-Xmx20m
-XX:+UnlockExperimentalVMOptions
-XX:+AlwaysPreTouch
-XX:+UseEpsilonGC
-XX:+HeapDumpOnOutOfMemoryError
-XX:HeapDumpPath=C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-16618400885811126911\heapDump.hprof

```

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

### :bulb: [Fixing maximum heap size as a threshold test](https://github.com/quick-perf/doc/wiki/Fixing-maximum-heap-size-as-a-threshold-test)

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

## @EnableGcLogging
_**Available in next QuickPerf release**_

To enable GC logging.

The path of the GC log file is displayed in the console:
```
GC log file: C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-1127741195724299919\gc.log
```

This file can be analysed with the help of a GC log analyzer:
* [GCViewer](https://github.com/chewiebug/GCViewer), [GCViewer Download 1](https://mvnrepository.com/artifact/com.github.chewiebug/gcviewer), [GCViewer Download 2](https://sourceforge.net/projects/gcviewer/)
* [Censum](https://www.jclarity.com/censum/)
* [GCeasy](https://gceasy.io/gc-index.jsp), the GC log file can be uploaded
* ...

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

*They measure heap allocation of the test method thread*.

You can  for example use @MeasureHeapAllocation and @ExpectMaxHeapAllocation to check the heap allocation cost of a large data structure (containing 1 000 000 elements for example) .<br>

@ExpectNoHeapAllocation can be used to verify that the tested code does not allocate on heap.


## @MeasureHeapAllocation
You can measure heap allocation with this annotation. <br><br>
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


# Verify resident set size (RSS)

## @MeasureRSS
_**Available in next QuickPerf release**_

You can measure the [Resident Set Size (RSS)](https://en.wikipedia.org/wiki/Resident_set_size) with this annotation. <br><br>
The measured RSS is displayed in the console.


⚠️ _Today this annotation only woks on Linux. You can work on this [issue](https://github.com/quick-perf/quickperf/issues/56) to make the RSS annotations work on MacOS._

## @ExpectMaxRSS
_**Available in next QuickPerf release**_

With this annotation, the test will fail if the [Resident Set Size (RSS)](https://en.wikipedia.org/wiki/Resident_set_size)   is greater than expected.

⚠️ _Today this annotation only woks on Linux. You can work on this [issue](https://github.com/quick-perf/quickperf/issues/56) to make the RSS annotations work on MacOS._

### :wrench: Parameters 
|Parameter  |Type           | Meaning   | 
| -------- |:--------------:|:---------:|
| value    | long           |value      |   
| unit     | AllocationUnit |RAM unit   |

# Profile or check your JVM
The following annotations use *Java Flight Recorder* (JFR) under the hood. <br>

⚠️ JFR profiling works with 
* Oracle JDK >= 1.7u40 
* OpenJDK >= 11
* [OpenJDK 8 with version >= 8u262](https://bugs.openjdk.java.net/browse/JDK-8239140)

## @ProfileJvm
To profile JVM with Java Flight Recorder (JFR).<br>

The JFR file location is shown in the console. You can open it with Java Mission Control.
<br>
### :mag_right: Example
```
[QUICK PERF] JVM was profiled with Java Flight Recorder (JFR).
The recording file is available here: C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-9292511997956298899\jvm-profiling.jfr
You can open it with Java Mission Control (JMC).
Where to find Java Mission Control? 👉 https://tinyurl.com/find-jmc
```

### :bulb: Where to find Java Mission Control (JMC)?

There are several ways to get JMC:
* A _jmc_ executable is available in the _bin_ folder of some Oracle JDK (10   >= version >= 1.7u40)
* With [Oracle](https://jdk.java.net/jmc/)
* With Azul [Zulu Mission Control](https://www.azul.com/products/zulu-mission-control/)
* With [Liberica Mission Control](http://www.google.com/search?q=Download+Liberica+Mission+Control)
* With Red Hat,
```
 yum install rh-jmc
```
* The last JMC release can be found [here (AdoptOpenJDK)](https://adoptopenjdk.net/jmc.html) 

## @DisplayJvmProfilingValue

### :mag_right: Example 1

```java
@DisplayJvmProfilingValue
```

```
[QUICK PERF] JVM profiling values
	 * The total duration of all GC pauses: 1,082 s
	 * The duration of the longest GC pause: 169,547 ms
	 * An estimate of the total allocation. This is not an exact value.: 3,68 GiB
	 * The estimated allocation size in TLABs: 3,67 GiB
	 * The total size of allocations outside TLABs: 12,4 MiB
	 * The number of non-error throwables created: 0
	 * The number of created errors: 36
	 * The number of created throwables: 36
	 * The number of compilations: 131
	 * Longest compilation: 1,264 s
	 * The number of full code cache events: 0
	 * JVM Name: OpenJDK 64-Bit Server VM
	 * JVM Version: OpenJDK 64-Bit Server VM (11.0.1+13) for windows-amd64 JRE (11.0.1+13), built on Oct  6 2018 13:18:13 by "mach5one" with MS VC++ 15.5 (VS2017)
	 * JVM Arguments: -XX:+FlightRecorder -XX:+UnlockDiagnosticVMOptions -XX:+DebugNonSafepoints -XX:FlightRecorderOptions=stackdepth=128 -Xms6g -Xmx6g -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-13786478919350756048\heapDump.hprof -DquickPerfToExecInASpecificJvm=true -DquickPerfWorkingFolder=C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-13786478919350756048
	 * Number of Hardware Threads: 8
	 * Number of Cores: 4
	 * Number of Sockets: 1
	 * CPU Description
		Brand: Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz, Vendor: GenuineIntel
		Family: <unknown> (0x6), Model: <unknown> (0x8e), Stepping: 0xa
		Ext. family: 0x0, Ext. model: 0x8, Type: 0x0, Signature: 0x000806ea
		Features: ebx: 0x03100800, ecx: 0x7ffafbbf, edx: 0xbfebfbff
		Ext. features: eax: 0x00000000, ebx: 0x00000000, ecx: 0x00000121, edx: 0x2c100800
		Supports: On-Chip FPU, Virtual Mode Extensions, Debugging Extensions, Page Size Extensions, Time Stamp Counter, Model Specific Registers, Physical Address Extension, Machine Check Exceptions, CMPXCHG8B Instruction, On-Chip APIC, Fast System Call, Memory Type Range Registers, Page Global Enable, Machine Check Architecture, Conditional Mov Instruction, Page Attribute Table, 36-bit Page Size Extension, CLFLUSH Instruction, Debug Trace Store feature, ACPI registers in MSR space, Intel Architecture MMX Technology, Fast Float Point Save and Restore, Streaming SIMD extensions, Streaming SIMD extensions 2, Self-Snoop, Hyper Threading, Thermal Monitor, Streaming SIMD Extensions 3, PCLMULQDQ, 64-bit DS Area, MONITOR/MWAIT instructions, CPL Qualified Debug Store, Virtual Machine Extensions, Enhanced Intel SpeedStep technology, Thermal Monitor 2, Supplemental Streaming SIMD Extensions 3, Fused Multiply-Add, CMPXCHG16B, xTPR Update Control, Perfmon and Debug Capability, Process-context identifiers, Streaming SIMD extensions 4.1, Streaming SIMD extensions 4.2, x2APIC, MOVBE, Popcount instruction, TSC-Deadline, AESNI, XSAVE, OSXSAVE, AVX, F16C, LAHF/SAHF instruction support, Advanced Bit Manipulations: LZCNT, SYSCALL/SYSRET, Execute Disable Bit, RDTSCP, Intel 64 Architecture, Invariant TSC
	 * OS Version: OS: Windows 10 , 64 bit Build 18362 (10.0.18362.329)
```

### :mag_right: Example 2

```java
@DisplayJvmProfilingValue(valueType = ProfilingValueType.TOTAL_GC_PAUSE)
```

```
[QUICK PERF] JVM profiling values
	 * The total duration of all GC pauses: 946,397 ms
```

### :mag_right: Example 3
```java
    @DisplayJvmProfilingValue(valueType = {  ProfilingValueType.ALLOCATION_TOTAL
                                           , ProfilingValueType.ALLOC_INSIDE_TLAB_SUM
                                           , ProfilingValueType.ALLOC_OUTSIDE_TLAB_SUM
                                          }
                              )
```

```
[QUICK PERF] JVM profiling values
	 * An estimate of the total allocation. This is not an exact value.: 3,74 GiB
	 * The estimated allocation size in TLABs: 3,73 GiB
	 * The total size of allocations outside TLABs: 11,3 MiB

```

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