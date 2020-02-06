With this annotation, the test will fail if the number of returned columns is greater than expected.

💡 **_[Why limit the number of selected columns?](https://github.com/quick-perf/doc/wiki/Why-limit-the-number-of-selected-columns)_**

### :wrench: Parameters 
|Parameter  |Type| Meaning                             | Default value  |
| --------  |:---:|:----------------------------------:|:--------------:|
| value     | int |Maximum number of returned columns  |        0       |
### :mag_right: Example
```java
    @ExpectMaxSelectedColumn(5)
```