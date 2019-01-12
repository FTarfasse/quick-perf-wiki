## @NoCrossJoin
## @MaxReturnedSqlColumns
### Parameters 
|Parameter  |Type| Meaning                             | Default value  |
| --------  |:---:|:----------------------------------:|:--------------:|
| value     | int |Maximum number of returned columns  |        0       |
## @MaxSqlSelect
### Parameters 
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of selects  |        0       |

### Example
```java
    @MaxSqlSelect(1)
    @Test
    public void should_retrieve_all_cars() {	
     //...
    }
```
## @MaxSqlInsert
### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of inserts  |        0       |
## @MaxSqlUpdate
### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of updates  |        0       |
## @MaxSqlDelete
### Parameters
|Parameter  |Type| Meaning                   | Default value  |
| -------- |:---:|:-------------------------:|:--------------:|
| value    | int |Maximum number of deletes  |        0       |