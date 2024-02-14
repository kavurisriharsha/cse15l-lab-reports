# Lab Report - 3

The folder structure is as follows:
```bash
/home/gram/cse15l:
f1  f10  f2

/home/gram/cse15l/folder1:
f3  f4  f5  f6  f7  f8  f9
```

> Files contain a few lines of text that are irrelevant for the sake of this report. Relevant text is displayed in codeblocks when necessary.

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
```bash
gram@Harsha-Gram:~/cse15l/lr3$ grep -i "text" ./*
./f1:Text
./f10:text
./f2:tExT
```

### Recursive 
The `-r` flag can be used to search for patterns recursively i.e. within sub-directories of the specified directory.
```bash
gram@Harsha-Gram:~/cse15l/lr3$ grep -r "text" ./
./f10:text
./folder1/f3:text
./folder1/f5:text
./folder1/f4:text
```
> `f10` is a file in the current directory. `f3`, `f4`, `f5` are in the sub-directory `folder1`. All of these files contain the word `text`.

### Line number
The `-n` flag prints the line number of the matched line for each file.
```bash
gram@Harsha-Gram:~/cse15l/lr3/folder1$ grep -n "text" ./*
./f3:1:text
./f4:2:text
./f5:3:text
```
> `f3` contains the word `text` on line 1, `f4` contains the word on line 2 and `f5` contains the word on line 3.

### Context 
The `-C` flag followed by a positive integer `n`, prints `n` number of lines before and after the matched line.
```bash
gram@Harsha-Gram:~/cse15l/lr3/folder1$ grep -C 2 "context" ./f6
l2
l3
context
l4
l5
```
> 2 lines before and after the matched line are printed

### Invert match
The `-v` flag outputs all the lines that do not match the specified pattern.
```bash
gram@Harsha-Gram:~/cse15l/lr3/folder1$ grep -v "pattern" ./f7
This line
does not
match the
that is
specified.
```
> Line that contains the word `pattern` has been omitted.

### Multiple flags
These options can be chained together in a single command.
```bash
gram@Harsha-Gram:~/cse15l/lr3$ grep -rnC 2 "text" ./
./f10:1:text
--
./folder1/f3:1:text
--
./folder1/f6-2-l2
./folder1/f6-3-l3
./folder1/f6:4:context
./folder1/f6-5-l4
./folder1/f6-6-l5
--
./folder1/f5-1-
./folder1/f5-2-
./folder1/f5:3:text
--
./folder1/f4-1-
./folder1/f4:2:text
```

> Here, we can see that the command recursively searched all the files within the working directory and its sub-directory. It also printed 2 lines above and below where applicable along with the line numbers.




