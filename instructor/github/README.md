## Overview
This guide will detail the intended usage of the SourceAcademy website for GitHub-hosted courses.

It will detail the use of both SourceAcademy and GitHub in order to create a lightweight deployment of a SourceAcademy course.

## Content
- [Creating a GitHub classroom organization](#creating-a-github-classroom-organization)
- [Authorizing our application for your classroom organization](#authorizing-our-application-for-your-classroom-organization)
- [Adding students to your classroom organization](#adding-students-to-your-classroom-organization)
- [Authoring new assignments](#authoring-new-assignments)
    - [Editing starter code](#editing-starter-code)
    - [Editing an MCQ question](#editing-an-MCQ-question)
    - [Editing the assignment briefing and question descriptions](#Editing-the-assignment-briefing-and-question-descriptions)
    - [Adding and deleting questions](#Adding-and-deleting-questions)
    - [Writing testcases](#writing-testcases)
    - [Editing the Assignment Metadata](#editing-the-assignment-metadatas)
    - [Saving your changes](#saving-your-changes)
    - [Using old XMLs](#using-old-xmls)
-  [Publishing new assignments](#publishing-new-assignments)
    - [Creating template repositories](#creating-template-repositories)
    - [Editing the Course Information](#editing-the-course-information)

### Creating a GitHub classroom organization
1. Navigate to the (GitHub classroom website)[https://classroom.github.com/].
2. Sign in to GitHub.
3. Click on "New Classroom" and select "Create a New Organization".
4. Fill in the details of your classroom and confirm the creation of the Organization.

### Authorizing our application for your classroom organization
Source Academy might not be able to see your assignments if it is not given permission to access your organization's repositories. Please follow the steps below to set the permissions.

1. Log in with GitHub through our website. By approving SourceAcademy, you add it to your list of personally approved apps.
2. On your account, navigate to Settings > Applications > Authorized OAuth Apps.
3. Click on the Source Academy app, then grant permission for your classroom application.

### Adding students to your classroom organization
1. Navigate to the GitHub page of your organization. Take note - this is not the GitHub classroom page for the same organization.
2. Navigate to People > Invite Member and invite your student to the organization (your student will have to provide you with their username, and then accept your invitation to the organization).
3. Thereafter, your students will be able to use view their course through the SourceAcademy website.

### Authoring new assignments

#### Opening the Assignment Creator
1. After logging in on the classrooms page, click on the "Create A New Assignment!" button.
2. This will bring you to the editor workspace. The following sections will detail how the functionality of this page.

#### Editing starter code
1. The code editor on the left panel displays the starter code. This is the code that students will see when they first accept the assignment. 
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

2. To view the question from your student's point of view, click the toggle button in the control bar. If the view does not change, the JSON might be malformed.

#### Editing the assignment briefing and question descriptions
1. Click on the text to open a markdown editor
2. Edit the text as necessary.
3. Clicking away from the editor will cause the text to render.

#### Adding and deleting questions
1. Click on the add button to add another question
2. You can navigate between questions with the Next and Previous buttons.
3. You can click on the delete button to get rid of a question.

#### Writing test cases
1. Each test case consists of three parts. An 
2. You may add a new test case by clicking on the "Add a new test case" button.
3. You may run a test case by clicking the Play button on the right. You may also run all the test cases at once by running the program with the test cases tab open.
4. You may delete a test case by clicking the the X button on the right.
5. You may also edit the Test Prepend and Test Postpend. These are programs that will be appended to the student's code. You can use this to set up variables or define helper functions for your tests.

#### Editing the Assignment Metadata
Take note that this information is not reflected in the course information. For example, when creating a course, the link to the cover-image will not be recovered from the assignment repository, but rather from the course-information file. However, you can use this section to record these details.

The following information can be saved:
| Property | Description |
| --- | --- |
| Title | The display title of the Assignment. This is typically different from the repository name.
| Cover Image Link | A link to the Assignment's display image.
| Summary | A one-line summary of the Assignment.
| Type | The classification of the Assignment. For example, an assigment could be "Homework" or "Extra Credit".
| ID | An ID number for the Assignment.
| Source Version | The Source Version that the program should be ran with.
| Reading | The pages of the textbook the assignment is in reference to.
| Due Date | The date by which the assignment should be completed to be accepted for marking.

#### Saving your changes
1. Click on the "Save" button.
2. If you are creating a new assignment, you will be prompted to name the new repository.
3. Click "Confirm" to continue. You can check corresponding GitHub repository to ascertain that the changes were saved.

#### Using old XMLs
There is currently no automated conversion from an XML-file to an assignment repository. Old XMLs need to be manually converted into assignment repositories, whether through the website interface or through committing to a GitHub repository.

In order to accomplish this, you may copy-and-paste the relevant sections of the XML (such as the briefings and task descriptions) into the workspace, and save to GitHub.

### Publishing new assignments

#### Creating template repositories
1. Navigate to the GitHub page of the repository you would like to use as a template for your assignment.
2. Navigate to settings and tick the box for "Template repository".

#### Creating an GitHub classroom assignment
1. In the GitHub classroom page, navigate to your organization.
2. Click on "New assignment".
3. Fill in the details for your organization. Make sure to use the [template repository](#creating-template-repositories) as the template repository for the new assignment under the Starter Code section.
4. After your assignment has been created, a link to accept the assignment will be created.

#### Editing the Course Information
