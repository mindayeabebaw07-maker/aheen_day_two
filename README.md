# aheen_day_two
*Student:* Mindaye Abebaw
*Title:* Setting Up a GitHub Repository from Scratch and Making a First Commit

*Audience:* First-semester computing student with basic computer literacy, no GitHub experience.

Prerequisites

Before you start, make sure you have all of the following:

1. A computer with Windows, macOS, or Linux
2. A stable internet connection
3. A GitHub account (mine is *mindayeabebaw07-maker*)
4. Git installed on your computer - check by opening your terminal and typing `git --version`
5. A code editor, e.g. VS Code
6. Your GitHub username and password / login access

Procedure

*1. Open your terminal / command prompt.*
Expected result: A black/white command window opens and shows a prompt.

*2. Create a new folder for your project.*
Type: `mkdir mindaye-first-project`
Expected result: No error appears, the cursor moves to the next line.

*3. Move into that folder.*
Type: `cd mindaye-first-project`
Expected result: The prompt now shows you are inside `mindaye-first-project`.

*4. Initialise Git in the folder.*
Type: `git init`
Expected result: You see: `Initialized empty Git repository...`

*5. Create a README file.*
Type: `echo "# Mindaye First Project" > README.md`
Expected result: A file called `README.md` is created in the folder.

*6. Tell Git to track the file.*
Type: `git add README.md`
Expected result: No error appears, the cursor moves to the next line.

*7. Save your first version.*
Type: `git commit -m "Add initial README by Mindaye"`
Expected result: You see: `[main (root-commit)...] Add initial README by Mindaye` and `1 file changed`.

*8. Go to http://github.com in your browser and log in.*
Expected result: Your GitHub dashboard/home page opens.

*9. Click the + icon at the top right.*
Expected result: A dropdown menu appears.

*10. Click "New repository".*
Expected result: The "Create a new repository" page opens.

*11. Type `mindaye-first-project` as the Repository name.*
Expected result: The name appears in the field with a green tick.

*12. Leave the other settings as default and click "Create repository".*
Expected result: A new empty repository page opens with setup instructions.

*13. Copy the remote URL shown on the page.*
Expected result: The URL, e.g. `https://github.com/mindayeabebaw07-maker/mindaye-first-project.git`, is copied.

*14. Return to your terminal.*
Expected result: Your terminal window is in focus again.

*15. Connect your local folder to GitHub.*
Type: `git remote add origin https://github.com/mindayeabebaw07-maker/mindaye-first-project.git`
Expected result: No error appears.

*16. Upload your commit to GitHub.*
Type: `git push -u origin main`
Expected result: You see: `Branch 'main' set up to track remote branch...` and your files appear on GitHub after refresh.

*17. Refresh your GitHub repository page in the browser.*
Expected result: You see your `README.md` file and the text "# Mindaye First Project".

Screenshot Description

*Screenshot to include after Step 12:*
A full-screen screenshot of my newly created GitHub repository page. It should show the repository name `mindaye-first-project` at the top, the owner `mindayeabebaw07-maker`, the message "...or push an existing repository from the command line", and the HTTPS URL in the code box highlighted. The purpose is to show the learner exactly where to find the remote URL they need for Step 13.

Troubleshooting Note

*Most Common Error: `fatal: not a git repository`*

*What it means:* You tried to run a Git command in a folder where Git was not initialised, or you are in the wrong folder.

*How to fix:*
1. Type `pwd` to see which folder you are in.
2. Type `ls` to see if your project folder is there.
3. Type `cd mindaye-first-project` to go into the correct folder.
4. Run `git init` again if you skipped Step 4, then repeat from Step 6.

---

B) API Reference Entry

*Student:* Mindaye Abebaw

Create a New Task

*Endpoint:*
`POST /api/v1/projects/{projectId}/tasks`

*Description:*
Creates a new task inside a specific project for the authenticated user. The task is assigned to a user, given a due date, and given a priority level. It returns the full task object with its unique ID and creation time.

*Authentication Required:* Yes

Request Headers
Header	Required	Description
Authorization	Yes	Bearer token. Format: `Bearer <your_access_token>`
Content-Type	Yes	Must be `application/json`
Accept	No	`application/json`
Path Parameters
Name	Type	Required	Description
projectId	string	Yes	The unique ID of the project the task belongs to. Example: `proj_mindaye_001`
Query Parameters
None.

Request Body Parameters
Name	Type	Required	Description
title	string	Yes	Short name of the task. Min 3, max 100 characters. Example: `"Create student dashboard"`
description	string	No	Longer details about the task. Max 1000 characters.
assigneeId	string	Yes	User ID of the person assigned to the task. Example: `"user_mindaye_07"`
dueDate	string (date)	Yes	Due date in ISO 8601 format: `YYYY-MM-DD`. Must be today or a future date.
priority	string (enum)	Yes	Priority level. Allowed values: `low`, `medium`, `high`
Example Request Body (JSON)
{
  "title": "Create student dashboard",
  "description": "Build a simple dashboard for students to view their courses, assignments and grades.",
  "assigneeId": "user_mindaye_07",
  "dueDate": "2026-11-25",
  "priority": "medium"
}
*Example Request:*
POST /api/v1/projects/proj_mindaye_001/tasks
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
Content-Type: application/json
Example Successful Response Body (JSON) - 201 Created
{
  "success": true,
  "message": "Task created successfully",
  "data": {
    "taskId": "task_mindaye_101",
    "projectId": "proj_mindaye_001",
    "title": "Create student dashboard",
    "description": "Build a simple dashboard for students to view their courses, assignments and grades.",
    "assigneeId": "user_mindaye_07",
    "assigneeName": "Mindaye Abebaw",
    "dueDate": "2026-11-25",
    "priority": "medium",
    "status": "todo",
    "createdBy": "user_mindaye_07",
    "createdAt": "2026-11-10T10:15:30Z",
    "updatedAt": "2026-11-10T10:15:30Z"
  }
}
Possible HTTP Response Codes
Code	Meaning	When It Occurs
201 Created	Success	Task was created successfully.
400 Bad Request	Invalid input	Missing required field, `priority` is not `low/medium/high`, or `dueDate` is in the past or wrong format.
401 Unauthorized	Not logged in	No token provided, or token is expired/invalid.
403 Forbidden	No permission	User is authenticated but is not a member of this project.
404 Not Found	Project not found	The `projectId` in the URL does not exist.
422 Unprocessable Entity	Validation failed	`assigneeId` does not belong to a real user.
500 Internal Server Error	Server error	Unexpected error on the server. Try again later
