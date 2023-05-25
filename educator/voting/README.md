## Overview

Instructors can setup a voting question for students to vote for some popular solutions of specific assessments using a ranked tierlist.

## Content

- [Voting Workflow](#voting-workflow)
   - [Instructor](#instructor)
   - [Students](#students)
- [Voting XML](#contest-voting-xml)
- [Voting Guide](#voting-guide)
  - [Different Contest Types](#different-contest-types)
  - [Random Assignment of Entries](#random-assignment-of-entries)
  - [Valid vs Invalid Entries](#valid-vs-invalid-entries)
- [Leaderboard](#leaderboard)
## Voting Workflow

### Instructor

1. Upload a contest.xml in ground control

   Note that the backend **does not anonymize** students' answers, hence students should submit a standardized function header without their names for anonymity.
2. Once the contest has ended, upload the contest-voting.xml in ground control

   The vote assignment will be generated at the point of time of upload of contest-voting.xml (this is a possible future improvement that can be made)
3. Students can vote once the contest voting assessment is open, and the rolling leaderboard will compute the scores

   The rolling leaderboard will compute the scores of each student based on votes recieved and number of tokens in the student's answer. More information can be found in [Leaderboard](#leaderboard).

### Students

1. Students access the side contents to vote for their favorite entries.
Students can then execute the program and view the contest entries to vote for.
Students vote by dragging and dropping the card corresponding to the entry they are voting for into the tierlist.
![image](https://github.com/source-academy/general/assets/122250318/12f69841-6e95-4510-b1a2-8da2229aba01)


2. Once the student finalises the submission, the drag-and-drop function is disabled and scores are submitted based on the tier the student has placed each entry in.

## Contest Voting XML

Similar to the creating an [assessment xml](../assessment/README.md) for programming and mcq questions, except that except that you need to declare the [PROBLEM](../assessment/README.md#problem) type as “voting” and include a VOTING tag `<VOTING assessment_number='??' />`.

### VOTING Attributes

| attribute | details |
| --------- | ------- |
| assessment_number | The assessment number of the **contest** (i.e. If a contest was uploaded with the number "C4", the assessment_number will be "C4"). Required. |

### Example of a contest voting xml:

```xml
<?xml version="1.0"?>
<CONTENT xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://128.199.210.247">

<!-- ***********************************************************************
**** Contest Voting #D7
************************************************************************ -->
<TASK
  kind="practical"
  coverimage="http://via.placeholder.com/350x150"
  number="D7"
  title="Beautiful Runes Contest Voting"
>
  <READING>Textbook Chapter 1</READING>

  <TEXT>
Now is the time to rank the most beautiful runes made by your peers in a tierlist!

You will be randomly assigned 10 runes to rank, from S to D tier!
  </TEXT>

  <PROBLEMS>
    <PROBLEM type="voting" maxgrade="0" maxxp="100"> <!-- problem type is "voting"-->
      <TEXT>
You can now vote on the most beautiful runes made by your fellow students!
      </TEXT>
      <SNIPPET>
        <TEMPLATE>
Rank the beautiful runes!
        </TEMPLATE>
      </SNIPPET>
      <VOTING assessment_number="C4"/> <!-- addditional Voting tag -->
    </PROBLEM>
  </PROBLEMS>

  <DEPLOYMENT interpreter="1">
  </DEPLOYMENT>

</TASK>
</CONTENT>
```

## Voting Guide

### Different Contest Types

Students can vote on contests involving images (runes/curves) or sounds. The relevant modules will be automatically imported from Source Academy in the assessment.

### Random Assignment of Entries

Up to 10 entries from the contest entry bank will be randomly assigned to each student to vote on.

## Leaderboard

When the contest is ongoing, instructors will be able to view the rolling leaderboard by opening the contest voting assessment in their own account and navigating to the leaderboard pane.
![image](https://github.com/source-academy/general/assets/122250318/86775e8e-982f-44fb-80bd-d7372807bcbd)
After the contest has finished, both instructors and students will be able to view the final leaderboard by opening the contest voting assessment in their own account and navigating to the leaderboard pane.
![image](https://github.com/source-academy/general/assets/122250318/88d43007-949e-4d68-ab8e-d2b33d1e90c9)
