# Overview
This guide will detail the intended usage of the Source Academy website for GitHub-hosted courses.

It will detail the use of both Source Academy and GitHub in order to create a lightweight deployment of a Source Academy course.

# Content
- [Setting up course](#setting-up-course)
    - [Creating GitHub Classroom organization](#creating-github-classroom-organization)
    - [Authorizing Source Academy to access GitHub Classroom organization](https://github.com/source-academy/general/blob/master/instructor/github/README.md#authorizing-source-academy-to-access-github-classroom-organization)
    - [Adding learner to GitHub Classroom organization](#adding-learner-to-github-classroom-organization)
    - [Creating course repository](#creating-course-repository)
- [Creating assessment](#creating-assessment)
    - [Authoring assessment](#authoring-assessment)
        - [Opening Assessment Creator](#opening-assessment-creator)
        - [Editing starter code](#editing-starter-code)
        - [Editing multiple choice question](#editing-multiple-choice-question)
        - [Editing assessment briefing and task description](#editing-assessment-briefing-and-task-description)
        - [Adding and deleting task](#adding-and-deleting-task)
        - [Writing test case](#writing-test-case)
        - [**Editing assessment metadata**](#editing-assessment-metadata)
        - [Saving changes](#saving-changes)
        - [Using old XMLs](#using-old-xmls)
    -  [Publishing assessment](#publishing-assessment)
        - [Turning assessment repository into template repository](#turning-assessment-repository-into-template-repository)
        - [Creating GitHub Classroom assessment](#creating-github-classroom-assessment)
        - [Adding assessment to Source Academy course](#adding-assessment-to-source-academy-course)
- [Grading assessment](#grading-assessment)
- [Closing course](#closing-course)

# Setting up course

## Creating GitHub Classroom organization
1. Navigate to the GitHub Classroom website at https://classroom.github.com/.
2. Sign in to GitHub.
3. Click on "New Classroom" and select "Create a New Organization".
4. Fill in the details of your classroom and confirm the creation of the Organization. Note that the organization name has to be prefixed with 'source-academy-course' for Source Academy to identify it.

## Authorizing Source Academy to access GitHub Classroom organization
Source Academy might not be able to see your assessments if it is not given permission to access your organization's repositories. Please follow the steps below to set the permissions.

1. Go to https://source-academy.github.io/ and click on "Classrooms" at the top of the control bar. Click on "Log In" to login with GitHub through our website. By approving Source Academy, you add it to your list of personally approved apps.
2. On your account, navigate to Settings > Applications > Authorized OAuth Apps.
3. Click on the Source Academy app, then grant permission for your classroom application.

## Adding learner to GitHub Classroom organization
Learners can currently be added only one at a time.
1. Ask your learner for their GitHub username.
2. Navigate to the GitHub page of your organization. Take note - this is not the GitHub Classroom page for the same organization.
3. Navigate to People > Invite Member and invite the learner to the organization. Make sure to invite them as member:\
![Invite As Member](https://user-images.githubusercontent.com/47176493/122375134-8ac25380-cf95-11eb-9f0f-3ce067b981b1.png)
You can control member permissions by navigation to Settings > Member privileges.
4. The learner will receive an invitation to join the organization in their email, which they should accept.
5. The learner will then be able to go to https://source-academy.github.io/, click on "Classroom" and "Log In" with their GitHub account. They will be able to select your course and see the assessments you have created. Note that the assessment area will be empty before you have created any assessments.

## Creating course repository
1. Navigate to the Organization's GitHub page.
2. Create a new repository named 'course-info'.
3. In this repository, create a new file 'course-info.json'.
A sample format for this file is detailed below:
```json
course-info
{
  "CourseName": "ReplaceWithCourseName",
  "types":
  [
    {
      "typeName": "Missions",
      "assessments":
      [
        {
          "title": "Mission1",
          "openAt": "2020-12-01T00:00:00+08:00",
          "closeAt": "2021-12-31T23:59:59+08:00",
          "published": "yes",
          "coverImage": "https://i.imgur.com/q2O4iwa.png",
          "shortSummary": "In this mission, you get introduced to visible functions, called Curves!",
          "acceptLink": "https://classroom.github.com/a/PyAUhdfe",
          "repoPrefix": "somePrefix"
        },
        {
          "title": "Mission2",
          "openAt": "2020-12-01T00:00:00+08:00",
          "closeAt": "2021-05-30T23:59:59+08:00",
          "published": "yes",
          "coverImage": "https://avatars.githubusercontent.com/u/35620705?s=400&u=32f72fd1d65a0d6877ad1d5870ffa327dda754f1&v=4",
          "shortSummary": "This is a demo mission!",
          "acceptLink": "https://classroom.github.com/a/CxlqjLaP",
          "repoPrefix": "somePrefix2"
        }
      ]
    },
    {
      "typeName": "Quests",
      "assessments":
      [
        {
          "title": "Quest1",
          "openAt": "2020-12-01T00:00:00+08:00",
          "closeAt": "2021-12-31T23:59:59+08:00",
          "published": "yes",
          "coverImage": "URL_TO_IMAGE",
          "shortSummary": "A sample assessment that should show up.",
          "acceptLink": "URL_TO_GITHUB_CLASSROOM",
          "repoPrefix": "somePrefix3"
        }
      ]
    }
  ]
}
```
The above example will display 2 Missions and 1 Quest on the learner's Source Academy frontend.

| Property | Description |
| --- | --- |
| title | A string value with the display name of the assessment.
| openAt and closeAt | start and due dates of the assessment in ISO 8601 standard for Date and Time.
| published | "yes" or "no" value determines if the assessment is visible for learners.
| coverImage | A string value of image URL for cover image of the assessment.
| shortSummary | A string value that summarises the assessment.
| acceptLink | A string value of the assignment accept link on GitHub Classroom.
| repoPrefix | A string value given to all repositories generated from GitHub Classroom.

The value of "categoryDisplayName" can be changed to replace the assessment headers in the navbar.  
e.g. instead of "Missions", instructors may choose "Assessment" or any other suitable names.

4. The course repository is now ready for use.

# Creating assessment

## Authoring assessment
Take note that any changes made in the Assessment Creator will be accessible for learners to view after they have accepted the assessment. They will be present in the repository created for the learners.

This includes all test cases and solutions to multiple-choice questions. If it is important that this information is not exposed, it is highly advised that they are not input into the Assessment Creator.

### Opening Assessment Creator
1. After logging in on the classrooms page, click on the "Create A New Assessment!" button.
2. This will bring you to the editor workspace. The following sections will explain how to edit assessments on this page.

### Editing starter code
1. The code editor on the left panel displays the starter code. This is the code that learners will see when they first accept the assessment. 
2. Any changes you make as a teacher will be reflected in the starter code.

### Editing multiple choice question
Take note that the solution will be available to the learner through their GitHub repository.

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

It is recommended that the "answer" is left as -1. This variable will be used to track the learners' answers.

If a solution is not provided, all choices will be highlighted in red when selected. If a solution is provided, the choice will be highlighted in green when selected. The choices are zero-indexed. Therefore, the correct solution amongst the choices above is "Charlie".

If you do not want to provide the solution with the question (since learners have access to their own GitHub repositories) the solution can be set to -1.

2. To view the question from your learner's point of view, click the toggle button in the control bar. If the view does not change, the JSON might be malformed.\
![Text and Learner View](https://user-images.githubusercontent.com/47176493/122369178-8c3d4d00-cf90-11eb-9022-efcdcc1d4b1c.png)


### Editing assessment briefing and task description
1. Click on the text to open a markdown editor
2. Edit the text as necessary.
3. Clicking away from the editor will cause the text to render.

### Adding and deleting task
1. Click on the add button to add another task. This will add a task numbered greater than the current task by 1. 
2. You can navigate between tasks with the Next and Previous buttons.
3. You can click on the delete button to get rid of a task.

### Writing test case
Take note that any test cases will also appear in the learner's side of the website.

![Testcase example](https://user-images.githubusercontent.com/47176493/122352810-087c6400-cf82-11eb-8058-222d671229a6.png)
1. You may add a new test case by clicking on the "Add a new test case" button. You may edit the test program and the edited result. The actual result is generated by the website when the test case is ran.
2. You may run a test case by clicking the Play button on the right. You may also run all the test cases at once by running the program with the test cases tab open.
3. You may delete a test case by clicking the the X button on the right.
4. You may also edit the Test Prepend and Test Postpend. The Test Prepend will be appended in front of the learner's program, while the Test Postpend will be appended to end of the learner's program. You can use this to set up variables or define helper functions for your tests.

### Editing assessment metadata
Currently, the Assessment Metadata only contains a single variable - the Source Version.

The Source Version will dictate which capabilities and libraries the programs will have access to. Please follow this link for more details: https://source-academy.github.io/source/

### Saving changes
1. Click on the "Save" button.
2. If you are creating a new assessment, you will be prompted to name the new repository.
3. Click "Confirm" to continue. You can check corresponding GitHub repository to ascertain that the changes were saved.

### Using old XMLs
There is currently no automated conversion from an XML-file to an assessment repository. Old XMLs need to be manually converted into assessment repositories, whether through the website interface or through committing to a GitHub repository.

In order to accomplish this, you may copy-and-paste the relevant sections of the XML (such as the briefings and task descriptions) into the workspace, and save to GitHub.

## Publishing assessment

### Turning repository into template repository
1. Navigate to the GitHub page of the repository you would like to use as a template for your assessment.
2. Navigate to settings and tick the box for "Template repository".

### Creating GitHub Classroom assessment
1. In the GitHub Classroom page, navigate to your organization.
2. Click on "New assignment".
3. In the new page, copy down the repository prefix for use in the next section. This will be used to locate student repositories for this assessment.
![Setting repository prefix](https://user-images.githubusercontent.com/42378805/122539209-ed7d2300-d059-11eb-9dd1-8c417f8d3736.png)
4. Fill in the details for your organization. Make sure to use the [template repository](#creating-template-repositories) as the template repository for the new assessment under the Starter Code section.
5. After your assessment has been created, an invitation link to accept the assessment will be created. Copy down this link for use in the next section. This link can be found on the GitHub Classroom page of the assessment.\
![Invitation Link](https://user-images.githubusercontent.com/47176493/122371718-a7a95780-cf92-11eb-93c9-fd73f37b52cb.png)

### Adding assessment to the Source Academy Course
1. Navigate to the course-info repository in your organization on GitHub.
2. Add an entry for the new assessment into the relevant category in the course-info.json file.  
For example, if this assessment belongs under "Missions", add it to the end of the "assessments:" array for that category.
![Add Entry](https://user-images.githubusercontent.com/42378805/122541401-2ddda080-d05c-11eb-9e19-072708acbe15.png)  
The entry should look something like this:
```
{
  "title": "insert-title-here",
  "openAt": "2020-12-01T00:00:00+08:00",
  "closeAt": "2021-12-31T23:59:59+08:00",
  "published": "yes",
  "coverImage": "insert-image-URL-here",
  "shortSummary": "insert-summary-here",
  "acceptLink": "paste-accept-link-here",
  "repoPrefix": "paste-repository-prefix-here"
}
```




# Grading assessment

We currrently do not have specific support for auto-testing assessments. Instructors will have to develop their own test scripts, download student submissions using GitHub Classroom, and communicate the results to learners.

Downloading submissions requires the use of GitHub Classroom Assistant, which can be downloaded at https://classroom.github.com/assistant.

A link to download repositories can be accessed from an assignment's page on GitHub Classroom:\
![Download Repositories](https://user-images.githubusercontent.com/47176493/122371252-500aec00-cf92-11eb-8cd0-af32c379f22e.png)

# Closing course

