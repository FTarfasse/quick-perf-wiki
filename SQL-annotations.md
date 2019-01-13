
## @MaxReturnedSqlColumns
### Parameters 
|Parameter  |Type| Meaning                             | Default value  |
| --------  |:---:|:----------------------------------:|:--------------:|
| value     | int |Maximum number of returned columns  |        0       |
### Example
```java
    @MaxReturnedSqlColumns(5)
```
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

## @NoCrossJoin
 The [cartesian product induced by a cross join can be very inefficient](https://vladmihalcea.com/hibernate-facts-always-check-criteria-api-sql-queries/). Although most database engines will try to remove a cross join, we can decide to remove cross join to not have to check if a database engine version will really remove it.
 
## @EnableSqlCrossJoin
To decide to enable a cross join in a specific case if you add @NoSqlCrossJoin check for every test or at test class level.