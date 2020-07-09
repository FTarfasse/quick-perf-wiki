**What is a QuickPerf test?**

A _QuickPerf test_ executes QuickPerf annotations.

**How to create a QuickPerf test?**

* **JUnit 4:** Add `@RunWith(QuickPerfJUnitRunner.class)` on the class

* **JUnit 4 + Spring:** Replace `@RunWith(SpringRunner.class)` by `@RunWith(QuickPerfSpringRunner.class)`

* **JUnit 5:** Add `@QuickPerfTest` on the test class

* **TestNG:** No annotation to add