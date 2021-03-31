# INTERVIEW BOARD

## Project Description

Interview Board is an SaaS solution that provides tools for training companies and associates. Internal users can schedule interview sessions with individual associates or entire batches, view scheduled and completed interviews, and manage a pool of mock interview questions. Associates can, as external users, add questions to the pool of mock interview questions, see upcoming interviews, read client feedback from recent interviews, and construct mock interviews for practice. Associates can also take a mock panel from an administrator created pool of panel questions.

Please make sure that you check out the wiki for more information regarding this project.


## Technologies Used

* The Salesforce Tech Stack:
> - Salesforce Orgs
> - Aura Components
> - Apex (Triggers, controllers, test classes)
> - Declarative tools
* VS Code
* Github

# Setting up your Repo

### Initializing the local project.
1. Go to the directory where you want your project directory to be created.
2. Use the SFDX: Create Project with Manifest command
   * An empty project is fine since all the code is on the GitHub repository
3. In a terminal window, enter the repository folder and use the git init command to create the local repository
4. Navigate to the GitHub repository and click the 'Code' button to copy the URL for the repo.
5. Continue using terminal and use the git remote add origin and paste <paste URL from step 4>
6. Use the git fetch origin command
7. Use the git checkout 2009-dev command to move into the code base and watch your VScode download the branch.

### Deploying to an org
1. Clone repo down to local. (use the VS code button on the welcome screen)
2. Create new playground, get login info
3. Authorize the playground with VS code. (copy down the admin email!)

(Alternatively for 2 & 3 you can create a scratch org)

4. Switch to 2009-dev branch. (already done if you followed the above!)
5. Change email to your admin email for the playground in force-app/main/default/sites/InterviewForce......xml
   * SiteGuest & SiteAdmin
6. Save changes.
7. In the playground, enable digital experiences/communities. (the domain name doesn't matter.)
8. Create a new digital experience/site.
   * Use the "Build Your Own" template. (NOT the "Build your own (LWR)")
   * Make sure to put something in the URL!!
   * The site name must NOT be "InterviewForce"
9. Use the right-click menu to deploy source from manifest to the playground.    (DO NOT use sfdx force:source:push -f to push to the scratch org)
   * Or you can use sfdx force:source:deploy -x .\manifest\package.xml
10. If deployed unsuccessfully, make sure you look at problems below.
11. When you deployed successfully, go to "All Sites" in Setup, open the "Interview Force" Builder, and publish the page on the top right. (If you don't do this, the Interview Force App will not work).
12. Make sure that interviewers and associates have populated emails OR go into email alert and add the admin as a recipient.

### Problems setting up your org?
Deployment: some people have to use SOAP instead of rest API. If you are having problems deploying, try:
   * in VS code terminal, write:
   * sfdx config:set restDeploy=false

Make sure you didn't miss step 5 or step 8. (they can be tricky)

If you change metadata files in ways that you weren't supposed to, you may get weird errors like Network is PicassoNetwork and is not ChatterPicassoNetwork.

### Loading Sample Data

1. Deactivate "Send Email when Meeting is Created" process.
2. Go into the InterviewBoard Administration App, and then the sync tab.
3. Click "Test Data Import"
4. The data is now loaded.

### Loading Production Data

1. Go into the InterviewBoard Administration App.
2. Click ManualSync.
* Note that Caliber has more than 5mb of data to import, so the import is too large.


# Main functionality
There are three Apps (accessable from the nine circles app button found on the top left of the org page, then clicking 'view all', then scrolling down to the bottom), the Interview Board Administration app, the Quiz app, and Interview Force, the community site.

## Interview Board Administration (Internal App)

This App is where administrators go to perform their duties. Deals with Meetings, Questions, Associates, and more! It has two tabs: The Interview Questions Tab and the Administration Tab.

### Interview Questions Tab
You can view the responses to a mock interview in the standard salesforce way in this tab.

### Administration Tab
This tab contains many more sub tabs to help in administrative duties.
#### Meetings
Here you can create mock interviews and assign them to associates. You can also look at all mock interviews assigned to an associate. You can assign a meeting to an entire batch, and you can now have meetings with specific time frames. 
#### Associates
Here you can create associates, search associates, and view meetings (upcoming and completed).
#### Questions
You can create questions in this tab.
#### Interests
Allows you to create relationships between batches and clients, and look at clients interested in particular batches
#### Question Pool
In this tab, you can view all the questions, look at the responses, delete questions, add questions to meetings, and lock/unlock questions. 
#### Sync
This tab allows you to load mass data of various types into the org. You can load example meetings, associates, batches, questions, question responses quickly for UAT, or you can load data from the mock caliber API to simulate the production environment. You can change the API endpoint for Caliber in the SyncUtilityClass, (and probably add some credentials somewhere) and then it will load production data instead.
#### Assign Stack
This tab allows you to assign tech stacks to associates. You can assign multiple stacks to one associate, or a stack to an entire batch.
#### Mock Interview Launcher
Once a mock interview is created, this tab will display questions from that interview and allow you to enter responses. Once all the questions are answered, and the user associated with the interview has an account, it will send an email to them.
#### Congratulations Board
This tab allows you to add an associate to the board. When a contact is selected, they're associated with a particular client and the date is set to today. It only displays associates for a rolling 7 days once they become selected. 

This tab provides some fun and congratulation to the trainer for doing a great job and getting associates selected while also tracking who gets selected.

## Quiz (Internal App)
This app is visible to associates, and is pretty similar to Kahoot. It allows you to answer questions and records your question answering speed.

### Home
This is the registration page, pulls all the information for the game. This is the only page you'll see while playing. 
Once you create a session, this page is populated. What you see changes based on the Phase field of the session. Only expects one Quiz Session record to exist.

### Quiz Answers
Typical Object tab. Create new, import, or change owner.

### Quiz Players
Saves the players that have registered for a certain session. You can create or import players, but you don't need to as they are created automatically.

### Quiz Questions
Allows you to create questions for different tech stacks.
* Typical Object tab. Create new, import, or change owner.
* The Quiz Question object has fields containing the text of four possible answers and a field that identifies the correct answer. The actual body of the question is contained in the Label field, but it doesn’t seem to be accessible from the record detail page or when you manually create a new record.
Once you navigate to a record page, there is a Question Creation wizard. You can use it to make quiz questions. This wizard *does* allow you to fill out the Label field.

### Quiz Sessions
A session hosts a list of questions, and creates the QR code. Only one session at a time, and it is the container for the game.
* Once you navigate to a quiz session record page, there’s a component where you can select a question stack and select & sort questions for this session.

### Agenda
Search for sessions.

### Interview Questions
Typical Object tab. Create new or import.

## InterviewForce
This is the community for external users (associates/trainees)

### Home
This tab allows you to see their upcoming meetings and their upcoming mock interviews.

### Prepare
This tab allows you to do mock panels in a specific skillset with specific subskills with a user-chosen number of questions. Once you create a mock panel, you can do questions and get scores on those questions.

### Review
This tab allows you to look at previous interviews that have completed. You can add questions to each interview. 

### Calendar
Create time slots for mock interviews, and associates can click on time slots to request mock interviews. 

### Profile
This tab allows you to see stacks, techs, and subtechs you've been assigned.

### Challenges
This tab allows you to see challenges.

### Forum
This tab allows you to look at various questions by category, answer those questions, rate question answers, and view related questions.

### Quiz
This tab is pretty similar to Kahoot. After you register for quizzing, it lists the players you are quizzing with, and the leaderboard will display and rank you based on your points. The tab also allows you to answer questions and records your question answering speed.


# To-do list:
* Packaging needs to be completed.
* Old code needs to be removed (some has been marked for removal in the wiki).


# Contributors

Batch January 11th 2021:

Keilah Cherelus - Project Leader

### Subteam One (Documentation, UAT, and Packaging)
Tobias Yasutake - Lead
Donny Buckman - Scrum Master
John Carrigan
Forest Sweeney
Naji Rhodes
Brandon Nyugen
 
### Subteam Two (internal initiatives) 
James Lopez - Lead
Joshua Clougherty - Scrum Master
Jordan McClellan
Ranveer Singh
Julio Lino
Erich Lewis
William Hatto

### Subteam Three (Research Spike Tasks – Quiz and GSuite)  
Michael Johnston - Lead
Kevin Luo - Scrum Master
Joseph Pappas
San 
Asija Watson
Joseph Tate 
Mena Soliman

### Subteam Four (Mock Interview and Panel)
Allan Duong - Lead
Charles Kosoy - Scrum Master
Clara Mahenzi
Tristan Wentworth
Charaf Boulafaa
