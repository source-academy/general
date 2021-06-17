## Overview
This guide will detail the intended usage of the Source Academy website for GitHub-hosted courses.

It will detail the use of both Source Academy and GitHub in order to create a lightweight deployment of a Source Academy course.

## Content
- [Creating a GitHub classroom organization](#creating-a-github-classroom-organization)
- [Authorizing our application for your classroom organization](#authorizing-our-application-for-your-classroom-organization)
- [Adding students to your classroom organization](#adding-students-to-your-classroom-organization)
- [Authoring new assessments](#authoring-new-assessments)
    - [Editing starter code](#editing-starter-code)
    - [Editing an MCQ question](#editing-an-MCQ-question)
    - [Editing the assessment briefing and question descriptions](#Editing-the-assessment-briefing-and-question-descriptions)
    - [Adding and deleting questions](#Adding-and-deleting-questions)
    - [Writing testcases](#writing-testcases)
    - [Editing the Assessment Metadata](#editing-the-assessment-metadatas)
    - [Saving your changes](#saving-your-changes)
    - [Using old XMLs](#using-old-xmls)
-  [Publishing new assessments](#publishing-new-assessments)
    - [Creating template repositories](#creating-template-repositories)
    - [Editing the Course Information](#editing-the-course-information)

### Creating a GitHub classroom organization
1. Navigate to the GitHub classroom website at https://classroom.github.com/.
2. Sign in to GitHub.
3. Click on "New Classroom" and select "Create a New Organization".
4. Fill in the details of your classroom and confirm the creation of the Organization.

### Authorizing our application for your classroom organization
Source Academy might not be able to see your assessments if it is not given permission to access your organization's repositories. Please follow the steps below to set the permissions.

1. Go to https://source-academy.github.io/ and click on "Classrooms" at the top of the control bar. Click on "Log In" to login with GitHub through our website. By approving Source Academy, you add it to your list of personally approved apps.
2. On your account, navigate to Settings > Applications > Authorized OAuth Apps.
3. Click on the Source Academy app, then grant permission for your classroom application.

### Adding students to your classroom organization
1. Ask your students for their GitHub usernames.
2. Navigate to the GitHub page of your organization. Take note - this is not the GitHub classroom page for the same organization.
3. Navigate to People > Invite Member and invite your student to the organization.
4. Your student will receive an invitation to join the organization in their email, which they should accept.
5. Your students will then be able to go to https://source-academy.github.io/, click on "Classroom" and "Log In" with their GitHub account. They will be able to select your course and see the assessments you have created. Note that the assessment area will be empty before you have created any assessments.

### Authoring new assessments

#### Opening the Assessment Creator
1. After logging in on the classrooms page, click on the "Create A New Assessment!" button.
2. This will bring you to the editor workspace. The following sections will detail how the functionality of this page.

#### Editing starter code
1. The code editor on the left panel displays the starter code. This is the code that students will see when they first accept the assessment. 
2. Any changes you make as a teacher will be reflected in the starter code.

#### Editing an MCQ question
1. The website is only able to recognize multiple-choice questions if the starter code is written in a specific format. This format should include the prefix "MCQ", followed by a JSON object. An example is given below:

```json
MCQ
{
  "choices":
    [
      { "option": "Alice", "hint":"Not this one" },
      { "option": "Bob", "hint":"Not this one" },
      { "option": "Charlie", "hint":"" }
    ],
  "answer": -1,
  "solution": 2
}
```

The possible choices for the question should be contained in an array. This array should be populated with objects containing the following properties:
 - option: The contents of the choice.
 - hint: A message that will pop up upon clicking the choice. This can be left as an empty string.

It is recommended that the "answer" is left as -1. This variable will be used to track the students' answers.

If a solution is not provided, all choices will be highlighted in red when selected. If a solution is provided, the choice will be highlighted in green when selected. The choices are zero-indexed. Therefore, the correct solution amongst the choices above is "Charlie".

If you do not want to provide the solution with the question (since students have access to their own GitHub repositories) the solution can be set to -1.

2. To view the question from your student's point of view, click the toggle button in the control bar. If the view does not change, the JSON might be malformed.\
![Text and Student View](https://user-images.githubusercontent.com/47176493/122351880-2b5a4880-cf81-11eb-8a0c-529155cd3e2a.png)


#### Editing the assessment briefing and question descriptions
1. Click on the text to open a markdown editor
2. Edit the text as necessary.
3. Clicking away from the editor will cause the text to render.

#### Adding and deleting questions
1. Click on the add button to add another question
2. You can navigate between questions with the Next and Previous buttons.
3. You can click on the delete button to get rid of a question.

#### Writing test cases
![Testcase example](https://user-images.githubusercontent.com/47176493/122352810-087c6400-cf82-11eb-8058-222d671229a6.png)
1. You may add a new test case by clicking on the "Add a new test case" button. You may edit the test program and the edited result. The actual result is generated by the website when the test case is ran.
2. You may run a test case by clicking the Play button on the right. You may also run all the test cases at once by running the program with the test cases tab open.
3. You may delete a test case by clicking the the X button on the right.
4. You may also edit the Test Prepend and Test Postpend. These are programs that will be appended to the student's code. You can use this to set up variables or define helper functions for your tests.

#### Editing the Assessment Metadata
Take note that this information is not reflected in the course information. For example, when creating a course, the link to the cover-image will not be recovered from the assessment repository, but rather from the course-information file. However, you can use this section to record these details.

The following information can be saved:
| Property | Description |
| --- | --- |
| Title | The display title of the Assessment. This is typically different from the repository name.
| Cover Image Link | A link to the Assessment's display image.
| Summary | A one-line summary of the Assessment.
| Type | The classification of the Assessment. For example, an assigment could be "Homework" or "Extra Credit".
| ID | An ID number for the Assessment.
| Source Version | The Source Version that the program should be ran with.
| Reading | The pages of the textbook the assessment is in reference to.
| Due Date | The date by which the assessment should be completed to be accepted for marking.

#### Saving your changes
1. Click on the "Save" button.
2. If you are creating a new assessment, you will be prompted to name the new repository.
3. Click "Confirm" to continue. You can check corresponding GitHub repository to ascertain that the changes were saved.

#### Using old XMLs
There is currently no automated conversion from an XML-file to an assessment repository. Old XMLs need to be manually converted into assessment repositories, whether through the website interface or through committing to a GitHub repository.

In order to accomplish this, you may copy-and-paste the relevant sections of the XML (such as the briefings and task descriptions) into the workspace, and save to GitHub.

### Publishing new assessments

#### Creating template repositories
1. Navigate to the GitHub page of the repository you would like to use as a template for your assessment.
2. Navigate to settings and tick the box for "Template repository".

#### Creating an GitHub classroom assessment
1. In the GitHub classroom page, navigate to your organization.
2. Click on "New assessment".
3. Fill in the details for your organization. Make sure to use the [template repository](#creating-template-repositories) as the template repository for the new assessment under the Starter Code section.
4. After your assessment has been created, a link to accept the assessment will be created.

#### Editing the Course Information
In order for Source Academy to recognise the course information, the organsation has to own a repository named 'course-info', with a file named 'course-info.json' placed in the root folder of that repository. The repository and file can be created using any tool and pushed to GitHub as long as ownership belongs to the organisation.

The format for course-info.json will be given below:
```json
{
  "CourseName": "CS1101S",
  "assessmentCategories":
  [
    {
      "categoryDisplayName": "Mission",
      "assessments":
      [
        {
          "id": "M6A_v07",
          "title": "Curve Introduction",
          "openAt": "2020-01-01T-00:00+00",
          "closeAt": "2021-12-31T-23:59+00",
          "published": "yes",
          "coverImage": "https://imgur.com/r/cats/MHXp1kt",
          "shortSummary": "In this mission, you get introduced to visible functions, called Curves!",
          "acceptLink": "https://classroom.github.com/a/PyAUhdfe",
          "repoPrefix": "sa-mission-curves"
        },
        {
          "id": "M6B",
          "title": "Sorting Things Out",
          "openAt": "2020-01-01T-00:00+00",
          "closeAt": "2021-12-31T-23:59+00",
          "published": "yes",
          "coverImage": "",
          "shortSummary": "Quicksort assessment description!",
          "acceptLink": "",
          "repoPrefix": "sa-mission-quicksort"
        }
      ]
    },
    {
      "categoryDisplayName": "Quest",
      "assessments":
      [
        {
          "id": "P6_v03",
          "title": "Curves",
          "openAt": "2020-01-01T-00:00+00",
          "closeAt": "2021-12-31T-23:59+00",
          "published": "yes",
          "coverImage": "",
          "shortSummary": "The Path P6 covers Lecture L6 of SICP1101 Unit 1.",
          "acceptLink": "",
          "repoPrefix": "sa-quest-curves"
        }
      ]
    },
    {
      "categoryDisplayName":"Paths",
      "assessments":[]
    },
    {
      "categoryDisplayName":"Contests",
      "assessments":[]
    },
    {
      "categoryDisplayName":"Others",
      "assessments":[]
    }
  ]
}
```
The above example will display 2 Missions and 1 Quest on the student's Source Academy frontend.

| Property | Description |
| --- | --- |
| id | A string value with the unique identifier of the assessment.
| title | A string value with the display name of the assessment.
| openAt and closeAt | start and due dates of the assessment in ISO 8601 standard for Date and Time.
| published | "yes" or "no" value determines if the assessment is visible on Source Academy (not yet implemented).
| coverImage | A string value with URL leading to cover image address for the assessment.
| shortSummary | A string value that summarises the assessment.
| acceptLink | A string value of the URL to accept the assessment on GitHub Classroom.
| repoPrefix | A string value given to all repositories generated from the assessment on GitHub Classroom.

In addition, the value to the right of "categoryDisplayName" can be changed to rename the headers for each of the assessments.
e.g. instead of "Missions", instructors may choose "Assessment" or any other suitable names.
