With this annotation the SQL statements are diplayed in the console during the test execution.<br>

Compared to [@DisplaySqlOfTestMethodBody](./DisplaySqlOfTestMethodBody), this annotation also diplays SQL statements before (JUnit 4: @Before, @BeforeClass) and after (JUnit 4: @After, @AfterClass) the execution of the test method body. <br>

⚠️ *It is not recommended to commit your test with this annotation. Indeed, the SQL statements would pollute the logs and may slow down the continuous integration build.*