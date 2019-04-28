[**Configure your JVM**](#Configure-your-JVM) <br> @HeapSize, @Xms, @Xmx, @JvmOptions <br><br>
[**Accurately verify heap allocation**](#Accurately-verify-heap-allocation)<br> @MeasureAllocation, @MaxAllocation, @NoAllocation <br><br>
[**Profile or check your JVM**](#Profile-or-check-your-JVM) <br> @ProfileJvmWithJavaFlightRecorder, @CheckJvm

# Configure your JVM
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
## @JvmOptions
With this annotation, the test is executed in a specific JVM having the given JVM options.
### Parameters 
|Parameter  |Type           | Meaning       | Default value |
| -------- |:--------------:|:-------------:|:-------------:|
| value    | String           |JVM options  |      -        |

# Accurately verify heap allocation
## @MeasureAllocation
You can measure allocation using this annotation. <br><br>
The test will be executed in a specific JVM.
<br><br>
Under the hood, QuickPerf uses ByteWatcher:
* https://github.com/danielshaya/ByteWatcher
* https://www.javaspecialists.eu/archive/Issue232.html

### Example
```java
@RunWith(QuickPerfJUnitRunner.class)
public class ClassWithMethodAnnotatedWithMeasureAllocation {

    @MeasureAllocation
    @JvmOptions("-XX:+UseCompressedOops -XX:+UseCompressedClassPointers")
    // Allocation value depends on UseCompressedOops and UseCompressedClassPointers.
    // QuickPerf works with JDK >= 7u40 where UseCompressedOops is enabled by default.
    // UseCompressedClassPointers was introduced in JDK 8 and is
    // enabled by default.
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
## @MaxAllocation
With this annotation, the test will fail if allocation is greater than expected. <br><br>
The test will be executed in a specific JVM.
<br><br>
Under the hood, QuickPerf uses ByteWatcher:
* https://github.com/danielshaya/ByteWatcher
* https://www.javaspecialists.eu/archive/Issue232.html
### Parameters 
|Parameter  |Type           | Meaning          | Default value  |
| -------- |:--------------:|:----------------:| :-------------:|
| value    | long           |Allocation value  |        -       |
| unit     | AllocationUnit |Allocation unit   |        -       |
### Example
 ```java
    @MaxAllocation(value = 440, unit = AllocationUnit.BYTE)
    @Test
    public void array_list_with_size_100_should_allocate_440_bytes() {
        ArrayList<Object> data = new ArrayList<>(100);
    }
  ```
## @NoAllocation
With this annotation, the test will fail if allocation is detected. <br><br>
The test will be executed in a specific JVM.
<br><br>
Under the hood, QuickPerf uses ByteWatcher:
* https://github.com/danielshaya/ByteWatcher
* https://www.javaspecialists.eu/archive/Issue232.html

# Profile or check your JVM
## @ProfileJvmWithJavaFlightRecorder

## @CheckJvm
With this annotation, JVM is profiled with Java Flight Recorder (JFR).<br><br>
The JFR file location is shown in the console:
```java
JFR file: C:\Users\UserName~1\AppData\Local\Temp\QuickPerf-1969922557\profiling.jfr
```
So, you can open the produced JFR file with Java Mission Control.<br><br>
Based on the profiling, some [JMC rules](http://hirt.se/blog/?p=920) are evaluated. For each rule a score is attributed. The maximum score value is 100.<br><br>
Things like significant primitives to object conversions can be detected:
<p align="center">
<img src="https://github.com/quick-perf/doc/blob/master/doc/images/JMC-PrimitiveToObjectConversion.PNG" width="944" heigth="191"></p>
With this annotation you can also detect that most of the time is spent to do garbage collection in your test.

### Parameters 
|Parameter  |Type           | Meaning           | Default value |
| -------- |:--------------:|:-----------------:|:-------------:|
| score    | int            |Rule score (<=100) |      60       |