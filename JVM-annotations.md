With JVM annotations, the test method is executed in a dedicated JVM.
# Outline
[**Configure your test JVM**](#Configure-your-test-JVM) <br> @HeapSize, @Xms, @Xmx, @JvmOptions <br><br>
[**Accurately verify heap allocation**](#Accurately-verify-heap-allocation)<br> @MeasureAllocation, @MaxAllocation, @NoAllocation <br><br>
[**Profile or check your JVM**](#Profile-or-check-your-JVM) <br> @ProfileJvm, @CheckJvm

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
The following annotations use ByteWatcher under the hood:
* https://github.com/danielshaya/ByteWatcher
* https://www.javaspecialists.eu/archive/Issue232.html

## @MeasureAllocation
You can measure allocation using this annotation. <br><br>
The measured allocation is displayed in the console. <br><br>
The test will be executed in a specific JVM.

### Example
```java
@RunWith(QuickPerfJUnitRunner.class)
public class ClassWithMethodAnnotatedWithMeasureAllocation {

    @MeasureAllocation
    @JvmOptions("-XX:+UseCompressedOops -XX:+UseCompressedClassPointers")
    // Allocation value depends on UseCompressedOops and UseCompressedClassPointers.
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
Measured allocation: 440.0 bytes
```
## @MaxAllocation
With this annotation, the test will fail if allocation is greater than expected. <br><br>
The test will be executed in a specific JVM.

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

# Profile or check your JVM
The following annotations use *Java Flight Recorder* (JFR) under the hood. <br><br>
*JFR profiling works with Oracle JDK >= 1.7u40 and OpenJDK >=11.*

## @ProfileJvm
To profile JVM with Java Flight Recorder (JFR).<br>

The JFR file location is shown in the console. You can open it with Java Mission Control.
<br>

```
[QUICK PERF] JVM was profiled with Java File Recorder (JFR).
The recording file can be found here: C:\Users\JEANBI~1\AppData\Local\Temp\QuickPerf-46868616\jvm-profiling.jfr
You can open it with Java Mission Control (JMC).
```
## @CheckJvm
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