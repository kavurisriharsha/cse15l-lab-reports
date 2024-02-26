# Lab Report - 3

The folder structure is as follows:
```bash
/home/gram/cse15l/docsearch/technical/:
911report  biomed  government  plos
```

> The files have been cloned from Github. Files contain text that is larely irrelevant to the scope of this document. Text has been mentioned in codeblocks where necessary

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
###### NOTE: The source for all the commands listed in this section is the `man grep` command. 

`grep` command is used to find files that contain a certain string pattern. This command has several options that can be enabled by using the respective "flag". 

### Ignore case
The `-i` flag can be used to enable case-insensitive search in grep. 
```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -i "america" ./technical/911report/chapter-5.txt
            AL QAEDA AIMS AT THE AMERICAN HOMELAND
                but his involvement with al Qaeda appears to have inspired him to pursue American
                group to join him in a "jihad against the Americans." Although all were urged to
                how Bin Ladin came to share his preoccupation with attacking America, Bin Ladin in
                November 2001 by an American air strike in Afghanistan.
                he collected Western aviation magazines; telephone directories for American cities
                anti-American opinions, ranging from condemnations of what he described as a global
                Saddam Hussein was an American stooge set up to give Washington an excuse to
                sessions at their Marienstrasse apartment that involved extremely anti-American
                on America. The 9/11 plotters eventually spent somewhere between $400,000 and
```

```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -i "conference" ./technical/911report/chapter-13.2.txt
            188. For the time of the teleconference, see FAA record, Chronology ADA-30, Sept. 11,
            189. For the times of the video teleconference, see White House record, Situation
                interview (May 17, 2004). The White House video teleconference was not connected
                information being shared on the video teleconference. See, e.g., Charles Leidig
                later participated in the White House video teleconference, they were necessarily
                Conference Call, Sept. 11, 2001.
            194. Charles Leidig interview (Apr. 29, 2004). Secure teleconferences are the NMCC's
                and "threat." Event conferences seek to gather information. If the situation
                escalates, a threat conference may be convened. On 9/11, there was no preset
                teleconference for a domestic terrorist attack. NMCC and National Military Joint
                conferences on 9/11, see DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
            195. See DOD transcript, Air Threat Conference Call, Sept. 11, 2001; see also White
                joining, see DOD transcript, Air Threat Conference Call, Sept. 11, 2001. For the FAA
            198. DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
            200. DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
                conference room, see White House record, PEOC Shelter Log, Sept. 11, 2001 (9:58);
                conference room; called headquarters immediately; call logged at 10:00); President
                conference call; and (4) Secret Service and White House Situation Room logs, as well
            217. DOD transcript, Air Threat Conference Call, Sept. 11, 2001. For one open line
                conference with FAA headquarters. Chuck Green interview (Mar. 10, 2004). For
                Service updates, see DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
            222. DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
                President's authorization, see ibid.; DOD transcript, AirThreat Conference Call,
                Sept. 11, 2001. For Hadley's statement, see DOD transcript, Air Threat Conference
            225. On the NMCC, see DOD transcript, Air Threat Conference Call, Sept. 11, 2001. On
                Conference Call, Sept. 11, 2001; Nelson Garabito interview (Mar. 11, 2004).
            226. DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
                of leaders in Washington, see DOD transcript, Air Threat Conference Call, Sept. 11,
            233. DOD note, transcript of Air Threat Conference Call, Sept. 11, 2001.
```

### Recursive 
The `-r` flag can be used to search for patterns recursively i.e. within sub-directories of the specified directory.
```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -r "america" ./technical/
./technical/biomed/1471-2148-3-7.txt:          Dermatocarpon americanum ,
./technical/biomed/1471-2148-3-7.txt:          Phialophora americana , X65199);
./technical/biomed/gb-2001-2-6-research0021.txt:          Reclinomonas americana - codes for
./technical/biomed/gb-2001-2-6-research0021.txt:          R. americana to initialize these
./technical/biomed/1472-6882-1-10.txt:        Mazama americana trinitatis ), lappe
./technical/biomed/1472-6882-1-10.txt:          Solanum americanum leaf decoction
./technical/biomed/1472-6882-1-10.txt:          Solanum americanum leaf extracts
./technical/plos/journal.pbio.0020001.txt:        well as data compiled by the Red Iberoamericana de Indicadores de Ciencia y Tecnología
./technical/plos/pmed.0020067.txt:        Necator americanus . Worldwide,
./technical/plos/pmed.0020067.txt:        N. americanus is the predominant etiology of human hookworm
./technical/plos/pmed.0020067.txt:        N. americanus were completed in 2004 (Figure 4). Pending Unite
```
> All the listed files are within sub-directories of `technical` but not the folder itself.

```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -r "Redwood" ./technical/
./technical/biomed/1471-2350-3-7.txt:          Oracle database (Oracle Corp., Redwood Shores, CA).
./technical/plos/journal.pbio.0020394.txt:        dictate, founding the Redwood Neuroscience Institute and also funding various conferences
```
> All the listed files are within sub-directories of `technical` but not the folder itself.

### Line number
The `-n` flag prints the line number of the matched line for each file.
```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -n "America" ./technical/911report/chapter-5.txt
229:                but his involvement with al Qaeda appears to have inspired him to pursue American
266:                group to join him in a "jihad against the Americans." Although all were urged to
369:                how Bin Ladin came to share his preoccupation with attacking America, Bin Ladin in
374:                November 2001 by an American air strike in Afghanistan.
496:                he collected Western aviation magazines; telephone directories for American cities
651:                anti-American opinions, ranging from condemnations of what he described as a global
654:                Saddam Hussein was an American stooge set up to give Washington an excuse to
799:                sessions at their Marienstrasse apartment that involved extremely anti-American
1035:                on America. The 9/11 plotters eventually spent somewhere between $400,000 and
```
> The word `America` appears several times in the file `chapter-5.txt`. The line number for each of these occurances is printed along with the line.

```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -n "conference" ./technical/911report/chapter-13.2.txt
984:            188. For the time of the teleconference, see FAA record, Chronology ADA-30, Sept. 11,
988:            189. For the times of the video teleconference, see White House record, Situation
995:                interview (May 17, 2004). The White House video teleconference was not connected
998:                information being shared on the video teleconference. See, e.g., Charles Leidig
1001:                later participated in the White House video teleconference, they were necessarily
1012:            194. Charles Leidig interview (Apr. 29, 2004). Secure teleconferences are the NMCC's
1014:                and "threat." Event conferences seek to gather information. If the situation
1015:                escalates, a threat conference may be convened. On 9/11, there was no preset
1016:                teleconference for a domestic terrorist attack. NMCC and National Military Joint
1018:                conferences on 9/11, see DOD transcript, Air Threat Conference Call, Sept. 11, 2001.
1107:                conference room, see White House record, PEOC Shelter Log, Sept. 11, 2001 (9:58);
1116:                conference room; called headquarters immediately; call logged at 10:00); President
1134:                conference call; and (4) Secret Service and White House Situation Room logs, as well
1140:                conference with FAA headquarters. Chuck Green interview (Mar. 10, 2004). For
```
> The word `conference` appears several times in the file. The line numbers are printed alongside for each occurance.

### Context 
The `-C` flag followed by a positive integer `n`, prints `n` number of lines before and after the matched line.
```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -C 4 "Confer" ./technical/911report/chapter-1.txt
    At the White House, Vice President Dick Cheney had just sat down for a meeting when his assistant told him to turn on his television because a plane had struck the NorthTower of the World Trade Center. The Vice President was wondering "how the hell could a plane hit the World Trade Center" when he saw the second aircraft strike the South Tower.

    Elsewhere in the White House, a series of 9:00 meetings was about to begin. In the absence of information that the crash was anything other than an accident, the White House staff monitored the news as they went ahead with their regular schedules.

The Agencies Confer

    When they learned a second plane had struck the World Trade Center, nearly everyone in the White House told us, they immediately knew it was not an accident. The Secret Service initiated a number of security enhancements around the White House complex. The officials who issued these orders did not know that there were additional hijacked aircraft, or that one such aircraft was en route to Washington. These measures were precautionary steps taken because of the strikes in New York.

    The FAA and White House Teleconferences. The FAA, the White House, and the Defense Department each initiated a multiagency teleconference before 9:30. Because none of these teleconferences-at least before 10:00- included the right officials from both the FAA and Defense Department, none succeeded in meaningfully coordinating the military and FAA response to the hijackings.
```
> 4 lines before and after the matched phrase are printed

```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -v "As we" ./technical/911report/preface.txt



            PREFACE
            We present the narrative of this report and the recommendations that flow from it to
                the President of the United States, the United States Congress, and the American
                people for their consideration. Ten Commissioners-five Republicans and five
                Democrats chosen by elected leaders from our nation's capital at a time of great
                partisan division-have come together to present this report without dissent.
            We have come together with a unity of purpose because our nation demands it.
                September 11, 2001, was a day of unprecedented shock and suffering in the history of
                the United States. The nation was unprepared. How did this happen, and how can we
                avoid such tragedy again?
            To answer these questions, the Congress and the President created the National
                Commission on Terrorist Attacks Upon the United States (Public Law 107-306, November
                27, 2002).
            Our mandate was sweeping. The law directed us to investigate "facts and circumstances
                relating to the terrorist attacks of September 11, 2001," including those relating
                to intelligence agencies, law enforcement agencies, diplomacy, immigration issues
                and border control, the flow of assets to terrorist organizations, commercial
                aviation, the role of congressional oversight and resource allocation, and other
                areas determined relevant by the Commission. In pursuing our mandate, we have
                reviewed more than 2.5 million pages of documents and interviewed more than 1,200
                individuals in ten countries. This included nearly every senior official from the
                current and previous administrations who had responsibility for topics covered in
                our mandate. We have sought to be independent, impartial, thorough, and nonpartisan.
                From the outset, we have been committed to share as much of our investigation as we
                can with the American people. To that end, we held 19 days of hearings and took
                public testimony from 160 witnesses.
            Our aim has not been to assign individual blame. Our aim has been to provide the
                fullest possible account of the events surrounding 9/11 and to identify lessons
                learned.
            We learned about an enemy who is sophisticated, patient, disciplined, and lethal. The
                enemy rallies broad support in the Arab and Muslim world by demanding redress of
                political grievances, but its hostility toward us and our values is limitless. Its
                purpose is to rid the world of religious and political pluralism, the plebiscite,
                and equal rights for women. It makes no distinction between military and civilian
                targets. Collateral damage is not in its lexicon.
            We learned that the institutions charged with protecting our borders, civil aviation,
                and national security did not understand how grave this threat could be, and did not
                adjust their policies, plans, and practices to deter or defeat it. We learned of
                fault lines within our government-between foreign and domestic intelligence, and
                between and within agencies. We learned of the pervasive problems of managing and
                sharing information across a large and unwieldy government that had been built in a
                different era to confront different dangers.
            At the outset of our work, we said we were looking backward in order to look forward.
                We hope that the terrible losses chronicled in this report can create something
                positive-an America that is safer, stronger, and wiser. That September day, we came
                together as a nation. The test before us is to sustain that unity of purpose and
                meet the challenges now confronting us. We need to design a balanced strategy for
                the long haul, to attack terrorists and prevent their ranks from swelling while at
                the same time protecting our country against future attacks. We have been forced to
                think about the way our government is organized. The massive departments and
                agencies that prevailed in the great struggles of the twentieth century must work
                together in new ways, so that all the instruments of national power can be combined.
                Congress needs dramatic change as well to strengthen oversight and focus
                accountability.
                Commissioners, whose dedication to this task has been profound. We have reasoned
                together over every page, and the report has benefited from this remarkable
                dialogue. We want to express our considerable respect for the intellect and judgment
                of our colleagues, as well as our great affection for them.
            We want to thank the Commission staff. The dedicated professional staff, headed by
                Philip Zelikow, has contributed innumerable hours to the completion of this report,
                setting aside other important endeavors to take on this all-consuming assignment.
                They have conducted the exacting investigative work upon which the Commission has
                built. They have given good advice, and faithfully carried out our guidance. They
                have been superb. We thank the Congress and the President. Executive branch agencies
                have searched records and produced a multitude of documents for us. We thank
                officials, past and present, who were generous with their time and provided us with
                insight. The PENTTBOM team at the FBI, the Director's Review Group at the CIA, and
                Inspectors General at the Department of Justice and the CIA provided great
                assistance. We owe a huge debt to their investigative labors, painstaking attention
                to detail, and readiness to share what they have learned. We have built on the work
                of several previous Commissions, and we thank the Congressional Joint Inquiry, whose
                fine work helped us get started. We thank the City of New York for assistance with
                documents and witnesses, and the Government Printing Office and W.W. Norton
                & Company for helping to get this report to the broad public.
            We conclude this list of thanks by coming full circle: We thank the families of 9/11,
                whose persistence and dedication helped create the Commission. They have been with
                us each step of the way, as partners and witnesses. They know better than any of us
                the importance of the work we have undertaken.
            We want to note what we have done, and not done. We have endeavored to provide the
                most complete account we can of the events of September 11, what happened and why.
                This final report is only a summary of what we have done, citing only a fraction of
                the sources we have consulted. But in an event of this scale, touching so many
                issues and organizations, we are conscious of our limits. We have not interviewed
                every knowledgeable person or found every relevant piece of paper. New information
                inevitably will come to light. We present this report as a foundation for a better
                understanding of a landmark in the history of our nation.
            We have listened to scores of overwhelming personal tragedies and astounding acts of
                heroism and bravery. We have examined the staggering impact of the events of 9/11 on
                the American people and their amazing resilience and courage as they fought back. We
                have admired their determination to do their best to prevent another tragedy while
                preparing to respond if it becomes necessary. We emerge from this investigation with
                enormous sympathy for the victims and their loved ones, and with enhanced respect
                for the American people. We recognize the formidable challenges that lie ahead.
            We also approach the task of recommendations with humility. We have made a limited
                number of them. We decided consciously to focus on recommendations we believe to be
                most important, whose implementation can make the greatest difference. We came into
                this process with strong opinions about what would work. All of us have had to
                pause, reflect, and sometimes change our minds as we studied these problems and
                considered the views of others. We hope our report will encourage our fellow
                citizens to study, reflect-and act.
            Thomas H. Kean, chair
            Lee H. Hamilton, vice chair
```
> 2 lines before and after the word `Dick` have been printed.

### Invert match
The `-v` flag outputs all the lines that do not match the specified pattern.

```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -v "We" ./technical/911report/preface.txt



            PREFACE
                the President of the United States, the United States Congress, and the American
                people for their consideration. Ten Commissioners-five Republicans and five
                Democrats chosen by elected leaders from our nation's capital at a time of great
                partisan division-have come together to present this report without dissent.
                September 11, 2001, was a day of unprecedented shock and suffering in the history of
                the United States. The nation was unprepared. How did this happen, and how can we
                avoid such tragedy again?
            To answer these questions, the Congress and the President created the National
                Commission on Terrorist Attacks Upon the United States (Public Law 107-306, November
                27, 2002).
            Our mandate was sweeping. The law directed us to investigate "facts and circumstances
                relating to the terrorist attacks of September 11, 2001," including those relating
                to intelligence agencies, law enforcement agencies, diplomacy, immigration issues
                and border control, the flow of assets to terrorist organizations, commercial
                aviation, the role of congressional oversight and resource allocation, and other
                areas determined relevant by the Commission. In pursuing our mandate, we have
                reviewed more than 2.5 million pages of documents and interviewed more than 1,200
                individuals in ten countries. This included nearly every senior official from the
                current and previous administrations who had responsibility for topics covered in
                From the outset, we have been committed to share as much of our investigation as we
                can with the American people. To that end, we held 19 days of hearings and took
                public testimony from 160 witnesses.
            Our aim has not been to assign individual blame. Our aim has been to provide the
                fullest possible account of the events surrounding 9/11 and to identify lessons
                learned.
                enemy rallies broad support in the Arab and Muslim world by demanding redress of
                political grievances, but its hostility toward us and our values is limitless. Its
                purpose is to rid the world of religious and political pluralism, the plebiscite,
                and equal rights for women. It makes no distinction between military and civilian
                targets. Collateral damage is not in its lexicon.
                and national security did not understand how grave this threat could be, and did not
                fault lines within our government-between foreign and domestic intelligence, and
                sharing information across a large and unwieldy government that had been built in a
                different era to confront different dangers.
            At the outset of our work, we said we were looking backward in order to look forward.
                positive-an America that is safer, stronger, and wiser. That September day, we came
                together as a nation. The test before us is to sustain that unity of purpose and
                the long haul, to attack terrorists and prevent their ranks from swelling while at
                think about the way our government is organized. The massive departments and
                agencies that prevailed in the great struggles of the twentieth century must work
                together in new ways, so that all the instruments of national power can be combined.
                Congress needs dramatic change as well to strengthen oversight and focus
                accountability.
            As we complete our final report, we want to begin by thanking our fellow
                together over every page, and the report has benefited from this remarkable
                of our colleagues, as well as our great affection for them.
                Philip Zelikow, has contributed innumerable hours to the completion of this report,
                setting aside other important endeavors to take on this all-consuming assignment.
                They have conducted the exacting investigative work upon which the Commission has
                built. They have given good advice, and faithfully carried out our guidance. They
                officials, past and present, who were generous with their time and provided us with
                insight. The PENTTBOM team at the FBI, the Director's Review Group at the CIA, and
                Inspectors General at the Department of Justice and the CIA provided great
                of several previous Commissions, and we thank the Congressional Joint Inquiry, whose
                documents and witnesses, and the Government Printing Office and W.W. Norton
                & Company for helping to get this report to the broad public.
                whose persistence and dedication helped create the Commission. They have been with
                us each step of the way, as partners and witnesses. They know better than any of us
                the importance of the work we have undertaken.
                most complete account we can of the events of September 11, what happened and why.
                This final report is only a summary of what we have done, citing only a fraction of
                the sources we have consulted. But in an event of this scale, touching so many
                every knowledgeable person or found every relevant piece of paper. New information
                understanding of a landmark in the history of our nation.
                have admired their determination to do their best to prevent another tragedy while
                enormous sympathy for the victims and their loved ones, and with enhanced respect
                this process with strong opinions about what would work. All of us have had to
                pause, reflect, and sometimes change our minds as we studied these problems and
                citizens to study, reflect-and act.
            Thomas H. Kean, chair
            Lee H. Hamilton, vice chair
```
> Line that contains the word `We` has been omitted.

```bash
gram@Harsha-Gram:~/cse15l/docsearch$ grep -v "As we" ./technical/911report/preface.txt



            PREFACE
            We present the narrative of this report and the recommendations that flow from it to
                the President of the United States, the United States Congress, and the American
                people for their consideration. Ten Commissioners-five Republicans and five
                Democrats chosen by elected leaders from our nation's capital at a time of great
                partisan division-have come together to present this report without dissent.
            We have come together with a unity of purpose because our nation demands it.
                September 11, 2001, was a day of unprecedented shock and suffering in the history of
                the United States. The nation was unprepared. How did this happen, and how can we
                avoid such tragedy again?
            To answer these questions, the Congress and the President created the National
                Commission on Terrorist Attacks Upon the United States (Public Law 107-306, November
                27, 2002).
            Our mandate was sweeping. The law directed us to investigate "facts and circumstances
                relating to the terrorist attacks of September 11, 2001," including those relating
                to intelligence agencies, law enforcement agencies, diplomacy, immigration issues
                and border control, the flow of assets to terrorist organizations, commercial
                aviation, the role of congressional oversight and resource allocation, and other
                areas determined relevant by the Commission. In pursuing our mandate, we have
                reviewed more than 2.5 million pages of documents and interviewed more than 1,200
                individuals in ten countries. This included nearly every senior official from the
                current and previous administrations who had responsibility for topics covered in
                our mandate. We have sought to be independent, impartial, thorough, and nonpartisan.
                From the outset, we have been committed to share as much of our investigation as we
                can with the American people. To that end, we held 19 days of hearings and took
                public testimony from 160 witnesses.
            Our aim has not been to assign individual blame. Our aim has been to provide the
                fullest possible account of the events surrounding 9/11 and to identify lessons
                learned.
            We learned about an enemy who is sophisticated, patient, disciplined, and lethal. The
                enemy rallies broad support in the Arab and Muslim world by demanding redress of
                political grievances, but its hostility toward us and our values is limitless. Its
                purpose is to rid the world of religious and political pluralism, the plebiscite,
                and equal rights for women. It makes no distinction between military and civilian
                targets. Collateral damage is not in its lexicon.
            We learned that the institutions charged with protecting our borders, civil aviation,
                and national security did not understand how grave this threat could be, and did not
                adjust their policies, plans, and practices to deter or defeat it. We learned of
                fault lines within our government-between foreign and domestic intelligence, and
                between and within agencies. We learned of the pervasive problems of managing and
                sharing information across a large and unwieldy government that had been built in a
                different era to confront different dangers.
            At the outset of our work, we said we were looking backward in order to look forward.
                We hope that the terrible losses chronicled in this report can create something
                positive-an America that is safer, stronger, and wiser. That September day, we came
                together as a nation. The test before us is to sustain that unity of purpose and
                meet the challenges now confronting us. We need to design a balanced strategy for
                the long haul, to attack terrorists and prevent their ranks from swelling while at
                the same time protecting our country against future attacks. We have been forced to
                think about the way our government is organized. The massive departments and
                agencies that prevailed in the great struggles of the twentieth century must work
                together in new ways, so that all the instruments of national power can be combined.
                Congress needs dramatic change as well to strengthen oversight and focus
                accountability.
                Commissioners, whose dedication to this task has been profound. We have reasoned
                together over every page, and the report has benefited from this remarkable
                dialogue. We want to express our considerable respect for the intellect and judgment
                of our colleagues, as well as our great affection for them.
            We want to thank the Commission staff. The dedicated professional staff, headed by
                Philip Zelikow, has contributed innumerable hours to the completion of this report,
                setting aside other important endeavors to take on this all-consuming assignment.
                They have conducted the exacting investigative work upon which the Commission has
                built. They have given good advice, and faithfully carried out our guidance. They
                have been superb. We thank the Congress and the President. Executive branch agencies
                have searched records and produced a multitude of documents for us. We thank
                officials, past and present, who were generous with their time and provided us with
                insight. The PENTTBOM team at the FBI, the Director's Review Group at the CIA, and
                Inspectors General at the Department of Justice and the CIA provided great
                assistance. We owe a huge debt to their investigative labors, painstaking attention
                to detail, and readiness to share what they have learned. We have built on the work
                of several previous Commissions, and we thank the Congressional Joint Inquiry, whose
                fine work helped us get started. We thank the City of New York for assistance with
                documents and witnesses, and the Government Printing Office and W.W. Norton
                & Company for helping to get this report to the broad public.
            We conclude this list of thanks by coming full circle: We thank the families of 9/11,
                whose persistence and dedication helped create the Commission. They have been with
                us each step of the way, as partners and witnesses. They know better than any of us
                the importance of the work we have undertaken.
            We want to note what we have done, and not done. We have endeavored to provide the
                most complete account we can of the events of September 11, what happened and why.
                This final report is only a summary of what we have done, citing only a fraction of
                the sources we have consulted. But in an event of this scale, touching so many
                issues and organizations, we are conscious of our limits. We have not interviewed
                every knowledgeable person or found every relevant piece of paper. New information
                inevitably will come to light. We present this report as a foundation for a better
                understanding of a landmark in the history of our nation.
            We have listened to scores of overwhelming personal tragedies and astounding acts of
                heroism and bravery. We have examined the staggering impact of the events of 9/11 on
                the American people and their amazing resilience and courage as they fought back. We
                have admired their determination to do their best to prevent another tragedy while
                preparing to respond if it becomes necessary. We emerge from this investigation with
                enormous sympathy for the victims and their loved ones, and with enhanced respect
                for the American people. We recognize the formidable challenges that lie ahead.
            We also approach the task of recommendations with humility. We have made a limited
                number of them. We decided consciously to focus on recommendations we believe to be
                most important, whose implementation can make the greatest difference. We came into
                this process with strong opinions about what would work. All of us have had to
                pause, reflect, and sometimes change our minds as we studied these problems and
                considered the views of others. We hope our report will encourage our fellow
                citizens to study, reflect-and act.
            Thomas H. Kean, chair
            Lee H. Hamilton, vice chair
```
> Lines that contain the words `As we` have been omitted.




