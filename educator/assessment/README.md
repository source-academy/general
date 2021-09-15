## Guide for Assessment Configuration

### Bonus XP calculation

The follwing diagram explains the two configurable numbers(`max bonus xp` and `early hours before xp decay`) in our system for calculating the bonus xp for assessments.

![bonus_xp](https://user-images.githubusercontent.com/39845424/125188426-537a5600-e266-11eb-972f-7113a2d31201.png)

## Guide for authoring and uploading assessments

Pieces of work that are to be submitted by students and the can be graded are called _assessments_ in the Source Academy.

Instructors can specify assessments in an XML format that is documented in this page. They upload assessment files using the "Ground Control" feature of the Source Academy frontend.

Your Source Academy deployment comes with an authoring tool for assessments. Visit
https://your-source-academy-url/mission-control
to access the tool. This page describes the XML format, in case you want to or need to manually edit the assessment XML files.

## General XML file format

Each node may have `attributes`. Each node has either `children` nodes or a `value`. There are no empty elements.

If a property is not documented for an element, then that element does not specify that property. For example, a [TASK](#task) has `attributes` and `children` nodes, and no `value`; and a [READING](#reading) has a `value` and neither `attributes` nor `children`.

Each element is marked as `required` or `optional`. A `required` element is compulsory only if its parent element exists. **All attributes are required (i.e. compulsory) unless explicitly stated "optional"**.

You may assume that each element must be unique (i.e. only one exists per parent), unless specified as "can have many".

## XML details

- [TASK](#task)
  - [PASSWORD](#password)
  - [READING](#reading)
  - [WEBSUMMARY](#websummary)
  - [TEXT](#text)
  - [PROBLEMS](#problems)
    - [PROBLEM](#problem)
      - [programming](#programming)
      	- [TEXT](#text)
       	- [SNIPPET](#snippet)
          - [PREPEND](#prepend)
          - [TEMPLATE](#template)
          - [POSTPEND](#postpend)
          - [TESTCASES](#testcases)
            - [PUBLIC](#public)
            - [OPAQUE](#opaque)
            - [SECRET](#secret)
        - [SOLUTION](#solution)
      - [mcq](#mcq)
      	- [TEXT](#text)
      	- [CHOICE](#choice)
      - [voting](#voting)
      	- [TEXT](#text)
      	- [VOTING](#voting)
      	- [SNIPPET](#snippet)
          - [PREPEND](#prepend)
          - [TEMPLATE](#template)
      
  - [DEPLOYMENT](#deployment) / [GRADERDEPLOYMENT](#graderdeployment)
    - [IMPORT](#import)
      - [SYMBOL](#symbol)
    - [EXTERNAL](#external)
      - [SYMBOL](#symbol)
    - [GLOBAL](#global)
      - [IDENTIFIER](#identifier)
      - [VALUE](#value)
- [EXAMPLE ASSESSMENTS XML](#example-assessments-xml)

## TASK

Represents an assessment. **Required**.

### Attributes

| attribute  | details                                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------------------ |
| coverimage | The thumbnail to show for this assessment. Must be a URL, i.e. not a file path. e.g. "https://http.cat/200". Optional.   |
| number     | The string identifier the relative position of this assessment. Must be unique among assessments.                        |
| title      | The title of this assessment.                                                                                            |
| story      | The story XML to load. Must correspond to the filename of a story XML file, e.g. "mission-2", "sidequest-2.1". Optional. |
| access     | Access control for the assessment. Can eitherbe set to "public" or "private". Defaults to "public".                      |

With AY2018/19, there is a change in the way that assessments are named and ordered. E.g. from "mission-2", "mission-19", "sidequest-10.3" to "M2A", "Q10A". This change is reflected in the number attribute.

The story XML to load in the game used to refer to this number attribute However, since the story XML filenames have not been updated to reflect this change in number, we need a new attribute story to point to the right story XML files.

This is not necessarily a bad change, since a module staff may now explicitly refer to a story XML file to be loaded for the assessment, rather than have it be implicitly inferred from the number attribute. Additionally, module staff writing story XML files will now have greater flexibility in how to name their files.

### Children

[READING](#reading), [WEBSUMMARY](#websummary), [TEXT](#text), [PROBLEMS](#problems), [DEPLOYMENT](#deployment), [GRADERDEPLOYMENT](#graderdeployment), [PASSWORD](#password)

Example

```xml
<TASK
  access="public"
  coverimage="http://via.placeholder.com/350x150"
  number="M2A"
  title="Beyond the Second Dimension"
  story="mission-2"
>
  ...
</TASK>
```

## PASSWORD

Only appiles to "private" assessments. Please choose a strong a complex password. If no password is given, an empty string (`""`) will be used.

### Value

A String that represents the assessment password.

### Example

```xml
<PASSWORD>MySuperSecretPassword1!23%$#@%</PASSWORD>
```

## READING

Represents a recommended reading from the textbook. Optional.

### Value

Text to be shown to the student, representing a recommended reading from the textbook.

### Example

```xml
<READING>Textbook Sections 1.1.1 to 1.1.4</READING>
```

## WEBSUMMARY

Represents a summary of the assessment to be shown in the list of assessments. Relevant only for the cadet website. Optional.

WEBSUMMARY is a new element defined for the cadet website. It is currently optional in order to minimise the amount of work required to migrate to this new API specification, but it is highly recommended that every [TASK](#task) have a WEBSUMMARY.

### Value

Text to be shown to the student, representing a summary of the assessment.

### Example

```xml
<WEBSUMMARY>A small scare broke out in the server room of the spaceship...</WEBSUMMARY>
```

## TEXT

Represents text to be shown to the student. Uses github-flavoured markdown. Can have many.

Required: at least one TEXT element as a direct child of each TASK, PROBLEM, and CHOICE.

### Value

Text to be shown to the student. The TEXT element may render differently based on its position and order in the XML tree.

- As a child of [TASK](#task), the first TEXT element is taken to be the assessment briefing. This provides a general overview of the assessment.
- As a child of [PROBLEM](#problem), the first TEXT element is taken to be the task description for "programming" questions, otherwise the MCQ question for "mcq" questions.
- As a child of [CHOICE](#choice), the first TEXT element is taken to be the content of said choice in the PROBLEM.
  Other TASK elements may be rendered in the assessment pdf, but are otherwise ignored by cadet.

Note that XML preserves whitespaces, so the indentations of the string in this value property will be exactly what is passed to the markdown parser; i.e. the text you write here should start with an absolute indentation level of 0. In addition, you must escape predefined XML entities quot ("), amp (&), apos ('), lt (<), and gt (>).

Note that the current backend implementation supports only absolute URLs for embedded images, i.e. not file paths. You can consider using an online image hosting service, e.g. Imgur.

### Example

````xml
<TEXT>
Write a function `steps` that takes four runes as arguments and arranges
them in a *2 x 2* square, starting with the top-right corner, going
clockwise, just like `mosaic` in Mission 1.

```
steps(rcross_bb, sail_bb, corner_bb, nova_bb);
```

![](http://via.placeholder.com/350x150)
</TEXT>
````

## PROBLEMS

Represents a set of PROBLEM elements. Required.

### Children

[PROBLEM](#problem)

### Example

```xml
<PROBLEMS>
  <PROBLEM type="...">...</PROBLEM>
  <PROBLEM type="...">...</PROBLEM>
  <PROBLEM type="..." showsolution="true">...</PROBLEM>
</PROBLEMS>
```

## PROBLEM

Represents a task within an assessment. Can have many.

Required: at least one PROBLEM in the PROBLEMS element.

### Attributes

| attribute    | details                                                                                                                          |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------- |
| maxxp        | The maximum xp achievable for thie PROBLEM. XP earned by students will be proportional to the grade as graded by the autograder. |
| type         | The type of this question. Can be "programming", "mcq" or "voting".                                                              |
| showsolution | Default false; If value is string "true", the solution string is shipped to web client. <br><sub> In a PROBLEM of type "mcq', the solution is used on the frontend to determine if the student's answer is correct (and to provide immediate visual feedback to the student by highlighting the selection green or red). This can be used together with the attribute `blocking` to ensure students answer MCQ questions correctly before proceeding to the next question. In the event `showsolution` is "false" and `blocking` is "true", the frontend is unable to verify the student's MCQ answer, and will allow him/ her to proceed after making any selection.</sub> <br><sub> In a PROBLEM of type "programming", the solution string, if sent, is currently NOT displayed to the student on the frontend. However, this might be subject to changes in the future, and the documentation will be updated accordingly here. Do note that more astute students might open the browser's network tab to view the solution string directly if it is sent. </sub> |
| blocking     | Default false; If value is string "true", the question is blocking; must be answered correctly to proceed to next question       |

### Children

[TEXT](#text), [SNIPPET](#snippet), [CHOICE](#choice), [VOTING](#voting), [DEPLOYMENT](#deployment), [GRADERDEPLOYMENT](#graderdeployment)

#### programming
[TEXT](#text), [SNIPPET](#snippet), [DEPLOYMENT](#deployment), [GRADERDEPLOYMENT](#graderdeployment)
#### mcq
[TEXT](#text), [CHOICE](#choice)
#### voting
[TEXT](#text), [SNIPPET](#snippet), [VOTING](#voting), [DEPLOYMENT](#deployment), [GRADERDEPLOYMENT](#graderdeployment)

### Example

```xml
<PROBLEM maxxp="100" type="programming">
  <TEXT>Your first task shall be to practise writing comments.</TEXT>
  <SNIPPET>...</SNIPPET>
</PROBLEM>

<PROBLEM maxxp="20" type="mcq">
  <CHOICE correct="..."></CHOICE>
  <CHOICE correct="..."></CHOICE>
  <CHOICE correct="..."></CHOICE>
  <CHOICE correct="..."></CHOICE>
</PROBLEM>

<PROBLEM maxxp="100" type="voting">
  <TEXT> You can now vote on the most beautiful runes made by your fellow students! </TEXT>
  <VOTING assessment_number="C5"/>
  <SNIPPET>...</SNIPPET>
</PROBLEM>
```

## CHOICE

Represents an option in a [PROBLEM](#problem) of type "mcq". Required for PROBLEM elements of type "mcq". Can have many.

### Attributes

| attribute | details                                                                                                   |
| --------- | --------------------------------------------------------------------------------------------------------- |
| correct   | If this is the correct option for the PROBLEM. Can be "true" or "false".                                  |
| hint      | The hint to show to the student should they select this CHOICE, and this CHOICE is not correct. Optional. |

### Example

```xml
<CHOICE correct="false"><TEXT>...</TEXT></CHOICE>
<CHOICE correct="true"><TEXT>...</TEXT></CHOICE>
<CHOICE correct="false"><TEXT>...</TEXT></CHOICE>
<CHOICE correct="false"><TEXT>...</TEXT></CHOICE>
```

## VOTING

Represents the configuration in a [PROBLEM](#problem) of type "voting". Required for PROBLEM elements of type "voting". Can only have one.

### Attributes

| attribute | details                                                                                                   |
| --------- | --------------------------------------------------------------------------------------------------------- |
| assessment_number | The assessment number that refers to the contest assessment for this voting assessment |

### Example

```xml
<VOTING assessment_number="C5"/>
```

## SNIPPET

Represents snippets of source programs in a [PROBLEM](#problem) of type "programming". Required for PROBLEM elements of type "programming".

In a [PROBLEM](#problem) of type "voting", valid children are [PREPEND](#prepend) and [TEMPLATE](#template). Optional for PROBLEM elements of type "voting". 

### Children

[PREPEND](#prepend), [TEMPLATE](#template), [POSTPEND](#postpend), [TESTCASES](#testcases), [SOLUTION](#solution)

### Example

```xml
<SNIPPET>
  <PREPEND>...</PREPEND>
  <TEMPLATE>...</TEMPLATE>
  <POSTPEND>...</POSTPEND>
  <TESTCASES>...</TESTCASES>
  <SOLUTION>...</SOLUTION>
</SNIPPET>
```

## PREPEND

Represents a program string to be prepended to student's code. Optional.

### VALUE

A program string to be prepended to the student's code.

Note that XML preserves whitespaces, so the indentations of the string in this value property will be exactly what is shown to the student; i.e. the programs you write here should start with an absolute indentation level of 0. In addition, you must escape predefined XML entities quot ("), amp (&), apos ('), li (<), and gt (>).

### EXAMPLE

```xml
<PREPEND>
function predeclared_function() {
    do_something();
}
</PREPEND>
```

## TEMPLATE

Represents an answer template that will be provided to the student when they first load up an assessment or when they reset an assessment. Required.

### Value

Text to be shown to the student, representing an answer template that will be shown when they first load up an assessment.

### Example

```xml
 <TEMPLATE>
function sum(a,b) {
    // your solution here
}
</TEMPLATE>
```

## POSTPEND

Represents a program string that is appended to the student code. Note that the postpend program string is only appended to student code during grading. Optional.

### VALUE

A program string that is appended to the student code.

### EXAMPLE

```xml
<POSTPEND>
function postdeclared_function() {
    do_something_else();
}
</POSTPEND>
```

## TESTCASES

Represents the test cases that are used to judge a student's code. Typically used together with serverside autograding (if configured). Optional.

### CHILDREN

[PUBLIC](#public), [OPAQUE](#opaque), [SECRET](#secret)

### EXAMPLE

```xml
<TESTCASES>
    <PUBLIC answer="1" score="1">...</PUBLIC>
    <PUBLIC answer="1" score="1">...</PUBLIC>
    <PUBLIC answer="1" score="1">...</PUBLIC>
    <OPAQUE answer="1" score="1">...</OPAQUE>
    <OPAQUE answer="1" score="1">...</OPAQUE>
    <SECRET answer="1" score="1">...</SECRET>
</TESTCASES>
```

## PUBLIC

Represents a public test case. Public test cases will be sent to the client. Students can see the testcase program and run them to see the result. Optional. Can have many.

### Attributes

| attribute | details                                                                  |
| --------- | ------------------------------------------------------------------------ |
| answer    | The answer to the particular test case. String format. Required.         |
| score     | The weighted score to the particular test case. String format. Required. |

### VALUE

The program string that calls the student-declared function.

### EXAMPLE

```xml
<PUBLIC answer="3" score="1">sum(1,2);</PUBLIC>
```

## OPAQUE

Represents an opaque test case. Opaque test cases will be sent to the client. The test case programs will be hidden from the students, but they can still run them to see if they pass the test. Grader can view the test case program in the grading workspace. Optional. Can have many.

### Attributes

| attribute | details                                                                  |
| --------- | ------------------------------------------------------------------------ |
| answer    | The answer to the particular test case. String format. Required.         |
| score     | The weighted score to the particular test case. String format. Required. |

### VALUE

The program string that calls the student-declared function.

### EXAMPLE

```xml
<OPAQUE answer="0" score="1">sum(-2,2);</OPAQUE>
```

## SECRET

Represents a secret test case. Secret test cases will not be sent to students but will be sent to the grading workspace. Primarily used to add more testcases for serverside autograding. Optional. Can have many.

### Attributes

| attribute | details                                                                  |
| --------- | ------------------------------------------------------------------------ |
| answer    | The answer to the particular test case. String format. Required.         |
| score     | The weighted score to the particular test case. String format. Required. |

### VALUE

The program string that calls the student-declared function.

### EXAMPLE

```xml
<SECRET answer="0" score="1">sum(-2,2);</SECRET>
```

## SOLUTION

Represents a solution to the PROBLEM that may be provided to the module staff during manual grading, in the grading interface. Required.

### Value

Working solution to be shown to the grading staff.

This is also a good place to insert any messages (as comments) that you may wish to convey to module staff viewing this assessment.

Note that XML preserves whitespaces, so the indentations of the string in this value property will be exactly what is shown to the student; i.e. the programs you write here should start with an absolute indentation level of 0. In addition, you must escape predefined XML entities quot ("), amp (&), apos ('), li (<), and gt (>).

### Example

```xml
<SOLUTION>
// [Marking Scheme]
// 0 marks

// flipping a picture vertically can be achieved
// by first flipping it horizontally and then
// turning the result upside_down (why?) (discuss
// the usefulness of this comment! Focus: Does
// it help the audience understand the program?)
function flipvert(picture) {
    return turn_upside_down(flip_horiz(picture));
    }
// example usage
show(flipvert(sail_bb));
</SOLUTION>
```

## DEPLOYMENT

Represents interpreter settings for the source interpreter of cadet.

**Required** for each [TASK](#task), optional for each [PROBLEM](#problem).

For a given PROBLEM, the following checks are performed to determine what interpreter settings should be used for the PROBLEM:

If the PROBLEM has a child DEPLOYMENT, use that
Otherwise, use the DEPLOYMENT which is a child of the TASK
The missionType attribute was previously used by source-academy2, but is unused by cadet.

### Attributes

| attributes  | details                    |
| ----------- | -------------------------- |
| interpreter | The source chapter to use. |

### Children

[IMPORT](#import), [EXTERNAL](#external), [GLOBAL](#global)

### Example

```xml
<DEPLOYMENT interpreter="2">
  <IMPORT module="cs1101s_1920/two_dim_runes">
      <SYMBOL>beside</SYMBOL>
      <SYMBOL>make_cross</SYMBOL>
  </IMPORT>
  <EXTERNAL name="CURVES">
      <SYMBOL>draw_points_on</SYMBOL>
      <SYMBOL>draw_connected</SYMBOL>
  </EXTERNAL>
  <GLOBAL>
      <IDENTIFIER>student_names</IDENTIFIER>
      <VALUE>list("Alpha Centauri", "007", "Gilgamesh");</VALUE>
  </GLOBAL>
</DEPLOYMENT>
```

## GRADERDEPLOYMENT

Represents interpreter settings which override that of DEPLOYMENT, for the grader. Optional for both TASK and PROBLEM.

For a given PROBLEM, the following checks are performed to determine what interpreter settings should be used for the grader:

If the PROBLEM has a child GRADERDEPLOYMENT, use that
Otherwise, if the PROBLEM has a child DEPLOYMENT, use that
Otherwise, if the TASK has a child GRADERDEPLOYMENT, use that
Otherwise, use the DEPLOYMENT which is a child of the TASK
This element is useful for assessments that utilise imported modules which use browser based APIs such as WebGL or HTML5 Audio. Since the grader runs on a node environment, these APIs are not available to the grader. Hence, it may be useful to 'overwrite' these functions with mock node-friendly alternative functions.

Examples include `show` from the runes module, `draw_connected` from the curves module, or `play` from the sound module.

### Attributes

| attributes  | details                    |
| ----------- | -------------------------- |
| interpreter | The source chapter to use. |

### Children

[IMPORT](#import), [EXTERNAL](#external), [GLOBAL](#global)

### Example

```xml
<GRADERDEPLOYMENT interpreter="2">
  <IMPORT module="cs1101s_1920/two_dim_runes">
      <SYMBOL>beside</SYMBOL>
      <SYMBOL>make_cross</SYMBOL>
  </IMPORT>
  <GLOBAL>...</GLOBAL>
  <GLOBAL>...</GLOBAL>
  <GLOBAL>...</GLOBAL>
</GRADERDEPLOYMENT>
```

## IMPORT

Represents a module to be exposed to the student. Optional. Can have many. The IMPORT mechanism is slated to replace the EXTERNAL mechanism in AY 20/21.

### Attributes

| attributes | details                                                                                            |
| ---------- | -------------------------------------------------------------------------------------------------- |
| module     | Path of the library, relative to `https://github.com/source-academy/assessments/tree/master/lib/`. |

### Children

[SYMBOL](#symbol)

### Example

```xml
<IMPORT module="cs1101s_1920/two_dim_runes">
  <SYMBOL>beside</SYMBOL>
  <SYMBOL>make_cross</SYMBOL>
</IMPORT>
```

## EXTERNAL

Represents an "external" library to be exposed to the student. Optional.

### Attributes

| attributes | details                                                                                                  |
| ---------- | -------------------------------------------------------------------------------------------------------- |
| name       | Name of the "external" library. Can be "NONE", "TWO_DIM_RUNES", "THREE_DIM_RUNES", "CURVES", or "SOUND". |

### Children

[SYMBOL](#symbol)

### Example

```xml
<EXTERNAL name="TWO_DIM_RUNES">
  <SYMBOL>beside</SYMBOL>
  <SYMBOL>make_cross</SYMBOL>
</EXTERNAL>
```

## SYMBOL

Represents an identifier from the [IMPORT](#import)ed module, or [EXTERNAL](#external) library or [GLOBAL](#global) variables to be exposed through the Source implementation. Put simply, this declares a variable in the source context that points to an identically named variable in the window namespace. Optional. Can have many.

The identifier exposed by the source interpreter will take the same name as the identifier it points to in the IMPORTed module. For example, the function `show` in Source will execute the function `show` in the specified IMPORTed module.

### Value

Text representing a valid JavaScript identifier name for the symbol.

### Example

```xml
<SYMBOL>beside</SYMBOL>
<SYMBOL>make_cross</SYMBOL>
```

## GLOBAL

Represents a variable to be dumped into the window namespace. Optional. Can have many.

This is useful to define functions or constants for the student's use, which are not provided by imported modules. For example, you may wish to expose a variable containing a list of names for the student to sort through in sorting missions.

You may also use this to override some property of the window, perhaps originating from an IMPORTed module.

### Children

[IDENTIFIER](#identifier), [VALUE](#value)

### Example

```xml
<GLOBAL>
  <IDENTIFIER>student_names</IDENTIFIER>
  <VALUE>list("Alpha Centauri", "007", "Gilgamesh");</VALUE>
</GLOBAL>
```

## IDENTIFIER

Represents the identifier name of a GLOBAL variable. Required.

### Value

Text representing a valid javascript identifier name for the GLOBAL variable.

### Example

```xml
<IDENTIFIER>student_names</IDENTIFIER>
```

## VALUE

Represents the javascript value of a GLOBAL variable. Required.

### Value

Text representing a valid JavaScript program to be eval'd. The return value of this evaluation is then taking to be the value of the GLOBAL variable.

### Example

```xml
<VALUE>list("Alpha Centauri", "007", "Gilgamesh");</VALUE>
```

## Example Assessments XML

```xml
<?xml version="1.0"?>
<CONTENT xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://128.199.210.247">

<!-- ***********************************************************************
**** MISSION #M99
************************************************************************ -->
<TASK
  coverimage="http://via.placeholder.com/350x150"
  number="M99"
  title="Simple Stuff"
  story="mission-99"
>

  <READING>Textbook Sections 1.1.1 to 1.1.2</READING>
  <WEBSUMMARY>
Welcome to your 99th mission!
  </WEBSUMMARY>
  <TEXT>
Welcome to this assessment! This is the *briefing*.

This mission consists of **four tasks**.

1. A simple PROBLEM that uses the default DEPLOYMENT and GRADERDEPLOYMENT
   for its GRADER programs (source 1).
2. A sorting PROBLEM that uses it's own DEPLOYMENT AND GRADERDEPLOYMENT.
   Here, GLOBALs and IMPORT are used to pre-define lists for the students
   as well as the grader to test the functions with.
3. An MCQ question.
4. A question which makes use of an imported module, `cs1101s_1920/two_dim_runes`.
   The functions from the module are exposed to the Source
   interpreter's global namespace according to what is specified in the
   SYMBOL elements.
  </TEXT>

  <PROBLEMS>
    <PROBLEM maxxp="200" type="programming">
      <TEXT>
Your first task is to define a function `sum` that adds two numbers together.
      </TEXT>
        <SNIPPET>
          <PREPEND/>
          <TEMPLATE>
const sum = () => 0; // Replace with your solution

// test your program!
sum(3, 5); // returns 8
          </TEMPLATE>
          <POSTPEND/>
          <TESTCASES>
            <PUBLIC answer="20" score="1">sum(10, 10);</PUBLIC>
            <PUBLIC answer="0" score="1">sum(0, 0);</PUBLIC>
            <PUBLIC answer="100" score="1">sum(99, 1);</PUBLIC>
            <PRIVATE answer="0" score="1">sum(-10, 10);</PRIVATE>
            <PRIVATE answer="-2" score="1">sum(-1, -1);</PRIVATE>
            <PRIVATE answer="-98" score="1">sum(-99, 1);</PRIVATE>
          </TESTCASES>
          <SOLUTION>
// [Marking Scheme]
// You may subtract 1 from grade if the student does not use arrow functions.
          </SOLUTION>
        </SNIPPET>
      </PROBLEM>

      <PROBLEM maxxp="100" type="programming">
        <TEXT>
Now, sort a list by any means. You are provided a list of numbers to test
your sort function out with.
        </TEXT>
        <SNIPPET>
          <PREPEND>
const numbers = list(7, 5, 3, 1);
const bigNumbers = list(15, 14, 13, 12, 11, 10, 1, 2, 3, 4, 5, 9, 8, 7, 6);
          </PREPEND>
          <TEMPLATE>
function sort() {
  // your solution here
}

// test your program!
sort(numbers); // should be [1, 3, 5, 7]
          </TEMPLATE>
          <POSTPEND/>
          <TESTCASES>
            <PUBLIC answer="[1,[3,[5,[7,[]]]]]" score="1">sort(numbers);</PUBLIC>
            <PRIVATE answer="[1, [2, [3, [4, [5, [6, [7, [8, [9, [10, [11, [12, [13, [14, [15, []]]]]]]]]]]]]]]]" score="1">sort(list(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15);</PRIVATE>
          </TESTCASES>
        </SNIPPET>
        <DEPLOYMENT interpreter="2">
          <IMPORT module="cs1101s_1920/two_dim_runes">
            <SYMBOL>show</SYMBOL>
          </EXTERNAL>
        </DEPLOYMENT>
        <GRADERDEPLOYMENT interpreter="2">
          <GLOBAL>
	    <IDENTIFIER>show</IDENTIFIER>
	      <VALUE>x => x;</VALUE>
	  </GLOBAL>
        </GRADERDEPLOYMENT>
      </PROBLEM>

      <PROBLEM maxxp="20" type="mcq">
        <TEXT>
What is the air-speed velocity of an unladen swallow?
        </TEXT>
        <CHOICE correct="false"><TEXT>5 meters per second</TEXT></CHOICE>
        <CHOICE correct="false"><TEXT>8 meters per second</TEXT></CHOICE>
        <CHOICE correct="true"><TEXT>11 meters per second</TEXT></CHOICE>
        <CHOICE correct="false"><TEXT>24 meters per second</TEXT></CHOICE>
      </PROBLEM>

      <PROBLEM maxxp="0" type="programming">
        <TEXT>
You'll use runes in your next mission. Why don't you give it a try?

`show(heart_bb)` should give you something like this:

![](http://via.placeholder.com/350x150)
![](https://http.cat/200)
        </TEXT>
        <SNIPPET>
          <TEMPLATE>
// shows the rune heart_bb
show(heart_bb);
          </TEMPLATE>
        </SNIPPET>
        <DEPLOYMENT interpreter="1">
          <IMPORT module="cs1101s_1920/two_dim_runes">
            <SYMBOL>beside</SYMBOL>
            <SYMBOL>make_cross</SYMBOL>
          </IMPORT>
          <EXTERNAL name="TWO_DIM_RUNES">
            <SYMBOL>show</SYMBOL>
            <SYMBOL>heart_bb</SYMBOL>
          </EXTERNAL>
        </DEPLOYMENT>
      </PROBLEM>

    </PROBLEMS>

    <TEXT>
## Submission

Make sure that everything for your programs to work is on the left hand side
and **not** in the REPL on the right! This is because only that program is
used to assess your solution.

This TEXT will not be shown in cadet, but may be useful in the generated pdf.
    </TEXT>

    <DEPLOYMENT interpreter="1">
    </DEPLOYMENT>

    <GRADERDEPLOYMENT interpreter="1">
    </GRADERDEPLOYMENT>

</TASK>
</CONTENT>
```
