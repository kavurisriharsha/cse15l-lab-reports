# Lab Report - 3

The folder structure is as follows:
```bash
.:
DocSearchServer.class  README.md                TestDocSearch.class  count-txts.sh~    start.sh
DocSearchServer.java   Server.class             TestDocSearch.java   find-results.txt  technical
FileHelpers.class      Server.java              URLHandler.class     grep-results.txt  test.sh
Handler.class          ServerHttpHandler.class  count-txts.sh        lib               test.sh~

./lib:
hamcrest-core-1.3.jar  junit-4.13.2.jar

./technical:
911report  biomed  government  plos

./technical/911report:
chapter-1.txt   chapter-12.txt    chapter-13.3.txt  chapter-2.txt  chapter-6.txt  chapter-9.txt
...
chapter-11.txt  chapter-13.2.txt  chapter-13.5.txt  chapter-5.txt  chapter-8.txt

./technical/biomed:
1468-6708-3-1.txt   1471-2164-3-34.txt  1471-230X-3-3.txt   1472-684X-1-5.txt   bcr588.txt
...
1471-2164-3-33.txt  1471-230X-2-23.txt  1472-6831-3-1.txt   bcr583.txt

./technical/government:
About_LSC  Alcohol_Problems  Env_Prot_Agen  Gen_Account_Office  Media  Post_Rate_Comm

./technical/government/About_LSC:
CONFIG_STANDARDS.txt                   ONTARIO_LEGAL_AID_SERIES.txt       Strategic_report.txt
...
ODonnell_et_al_v_LSCdecision.txt       State_Planning_Special_Report.txt

./technical/government/Alcohol_Problems:
DraftRecom-PDF.txt  Session2-PDF.txt  Session3-PDF.txt  Session4-PDF.txt

./technical/government/Env_Prot_Agen:
1-3_meth_901.txt  ctf1-6.txt   final.txt            nov1.txt                        tech_adden.txt
atx1-6.txt        ctf7-10.txt  jeffordslieberm.txt  ro_clear_skies_book.txt         tech_sectiong.txt
bill.txt          ctm4-10.txt  multi102902.txt      section-by-section_summary.txt

./technical/government/Gen_Account_Office:
GovernmentAuditingStandards_yb2002ed.txt  Testimony_d01609t.txt  im814.txt    og96034.txt  og97023.txt  og98026.txt
...
Testimony_cg00010t.txt                    gg96118.txt            og96033.txt  og97020.txt  og98024.txt

./technical/government/Media:
5_Legal_Groups.txt               Funding_cuts_force.txt              Pro_Bono_Services.txt
...
Funding_May_Limit.txt            Pro-bono_road_show.txt

./technical/government/Post_Rate_Comm:
Cohenetal_Cost_Function.txt  Cohenetal_Scale.txt       Mitchell_6-17-Mit.txt            ReportToCongress2002WEB.txt
...
Cohenetal_RuralDelivery.txt  Gleiman_gca2000.txt       Redacted_Study.txt

./technical/plos:
journal.pbio.0020001.txt  journal.pbio.0020224.txt  pmed.0010008.txt  pmed.0020027.txt  pmed.0020155.txt
...
journal.pbio.0020223.txt  journal.pbio.0030137.txt  pmed.0020024.txt  pmed.0020150.txt
```

> File names have been redacted for the sake of conserving space. Redacted files have been cloned from https://github.com/kavurisriharsha/docsearch.

## Bugs

Let us consider the method:
```java
// Returns a *new* array with all the elements of the input array in reversed
// order
static int[] reversed(int[] arr) {
  int[] newArray = new int[arr.length];
  for(int i = 0; i < arr.length; i += 1) {
    arr[i] = newArray[arr.length - i - 1];
  }
  return arr;
}
```

This program is known to contain a few errors. To pinpoint these errors, we use JUnit tests.
```java
@Test
public void testReversed() {
  int[] input1 = { };
  assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
}
```

```bash
gram@Harsha-Gram:~/$ java -cp .:./lib/hamcrest-core-1.3.jar:./lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
.
Time: 0.016

OK (1 test)
```
While the program is faulty, this test does not return an error. When we test the reverse method, we expect that the method returns a new array that contains all the same elements as the original array but in reverse order. In this case, we are testing on an empty array. Since it contains no elements, we are not able to observe any errors and the test passes.

To fix this, we can make the following changes:
```diff
-- int[] input1 = { };
++ int[] input1 = {1, 2, 3};
-- assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
++ assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));
```

Once we make these changes, we can see that the test now fails.
```bash
gram@Harsha-Gram:~/$ java -cp .:./lib/hamcrest-core-1.3.jar:./lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
.E
Time: 0.027
There was 1 failure:
1) testReversed(ArrayTests)
arrays first differed at element [0]; expected:<3> but was:<0>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReversed(ArrayTests.java:8)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<3> but was:<0>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more

FAILURES!!!
Tests run: 1,  Failures: 1
```

From the output, we can see that the first element was supposed to be 3 but was 0. To trace this error, we can look at the line of code where we assign elements to the new array. `arr[i] = newArray[arr.length - i - 1];` here, we are assigning the elements of the new array to the existing array when we want to do the opposite. 
So, we can fix this line by changing it to `newArray[arr.length - i - 1] = arr[i];`.

However, even after we make this change, we can see that an error is still thrown. 
```bash
gram@Harsha-Gram:~/$ java -cp .:./lib/hamcrest-core-1.3.jar:./lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
.E
Time: 0.02
There was 1 failure:
1) testReversed(ArrayTests)
arrays first differed at element [0]; expected:<3> but was:<1>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReversed(ArrayTests.java:8)
        ... 30 trimmed
Caused by: java.lang.AssertionError: expected:<3> but was:<1>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 36 more

FAILURES!!!
Tests run: 1,  Failures: 1
```
This time, the expected element is 3 but was 1. This aligns with the elements in the original array. So, we check if we are returning the right array. In the return statement, we can see that we are returning `arr` and not `newArray`. So, we change the line to `return newArray;`. After making this change, we can see that all the tests pass.
```bash
gram@Harsha-Gram:~/$ java -cp .:./lib/hamcrest-core-1.3.jar:./lib/junit-4.13.2.jar org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
.
Time: 0.014

OK (1 test)
```
The fixed code looks as follows:
```java
// Returns a *new* array with all the elements of the input array in reversed
// order
static int[] reversed(int[] arr) {
  int[] newArray = new int[arr.length];
  for(int i = 0; i < arr.length; i += 1) {
    newArray[arr.length - i - 1] = arr[i];
  }
  return newArray;
}
```

## `grep`

`grep` command is used to find files that contain a certain string pattern. This command has several options that can be enabled by using the respective "flag". 

### Ignore case
The `-i` flag can be used to enable case-insensitive search in grep. 





