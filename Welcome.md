# QuickPerf
QuickPerf is a Java open source project that provides annotations to quickly evaluate some performance properties. <br>
Each annotation can be applied at class level, at method level or for each test.
An annotation configured at method level overrides the configuration of this annotation defined at class level. An annotation configured for each test is overridden by the same annotation defined at class level or method level. <br>
A JUnit 4 version is going to be released in January 2019.<br>
A Spring version will be available around February 2019.<br>
A JUnit 5 version will be available around March 2019.<br>

## JVM annotations
### @MaxAllocation
This annotation is based on ByteWatcher (https://github.com/danielshaya/ByteWatcher, https://www.javaspecialists.eu/archive/Issue232.html).
#### Parameters 
|Parameter  |Type           | Meaning          | Default value  |
| -------- |:--------------:|:----------------:| :-------------:|
| value    | long           |Allocation value  |        -       |
| unit     | AllocationUnit |Allocation unit   |        -       |
#### Example
 ```java
    @MaxAllocation(value = 440, unit = AllocationUnit.BYTE)
    @Test
    public void array_list_with_size_100_should_allocate_440_bytes() {
        ArrayList<Object> data = new ArrayList<>(100);
    }
  ```
### @MeasureAllocation
This annotation is based on ByteWatcher (https://github.com/danielshaya/ByteWatcher, https://www.javaspecialists.eu/archive/Issue232.html).
#### Example
```java
@RunWith(QuickPerfJUnitRunner.class)
public class ClassWithMethodAnnotatedWithMeasureAllocation {

    @MeasureAllocation
    @JvmOptions("-XX:+UseCompressedOops -XX:+UseCompressedClassPointers")
    // JVM options written as documentation. Indeed, QuickPerf works
    // with JDK >= 7u40 where UseCompressedOops is enabled by default.
    // UseCompressedClassPointers was introduced in JDK 8 and is
    // enabled by default.
    // MaxAllocation depends on UseCompressedOops and UseCompressedClassPointers.
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
### @NoAllocation

### @HeapSize
#### Parameters 
|Parameter  |Type           | Meaning                  | Default value  |
| -------- |:--------------:|:------------------------:| :-------------:|
| value    | long           |Heap size value (Xms=Xmx) |        -       |
| unit     | AllocationUnit |Allocation unit           |        -       |
### @Xms
#### Parameters 
|Parameter  |Type           | Meaning                            | Default value |
| -------- |:--------------:|:----------------------------------:|:-------------:|
| value    | long           |Initial and minimum heap size value |        -      |
| unit     | AllocationUnit |Allocation unit                     |        -      |
### @Xmx
#### Parameters 
|Parameter  |Type           | Meaning                 | Default value |
| -------- |:--------------:|:-----------------------:|:-------------:|
| value    | long           |Maximum heap size value  |        -      |
| unit     | AllocationUnit |Allocation unit          |        -      |

### @JvmOptions
|Parameter  |Type           | Meaning       | Default value |
| -------- |:--------------:|:-------------:|:-------------:|
| value    | String           |JVM options  |      -        |

### @CheckJvm

## SQL annotations
### @NoCrossJoin
### @MaxReturnedSqlColumns
#### Parameters 
|Parameter  |Type| Meaning                             | Default value  |
| --------  |:---:|:----------------------------------:|:--------------:|
| value     | int |Maximum number of returned columns  |        0       |
### @MaxSqlSelect
#### Parameters 
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of selects  |        0       |

#### Example
```java
    @MaxSqlSelect(1)
    @Test
    public void should_retrieve_all_cars() {	
	}
```
### @MaxSqlInsert
#### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of inserts  |        0       |
### @MaxSqlUpdate
#### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of updates  |        0       |
### @MaxSqlDelete
#### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of deletes  |        0       |1. 1. ## ## 