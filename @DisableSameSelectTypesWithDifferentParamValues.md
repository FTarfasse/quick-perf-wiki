The test will fail if at least two SELECT statements have the same structure but have different parameter values.

### :mag_right: Example
       @DisableSameSelectTypesWithDifferentParamValues
       public void execute() {
           SELECT 1 with parameter A;
           SELECT 1 with parameter B;
       }