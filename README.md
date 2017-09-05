# HackerNewsClone

## Authors
This group consists of the following people:
- Marco Blum
- Christian Lind
- Kasper Pagh
- David Blum
- Alexander Kraunsøe

## 1. Introduction

### A. Purpose of the system
To create a platform where people can share links and stories, much like reddit. The users can then discuss those stories and links on he website

### B. Scope of the system
The System has to be deployed to a publicly visible server, it has to have an uptime of more than 95% and may not “lose” content sent to it from a simulator program.

### C. Objectives and success criteria of the project
- Users should be able to create an account and login.
- Users should be able to create stories or links on the website.
- Users should be able to create comments to other Users stories and/or comments
- Users should be able to upvote other Users content (stories, links and comments alike)
- The downvote option will not be available until the user has achieved a minimum of 500 “karma” points.
- Users should be able to Flag all content regardless of karma.
- The system needs to have an uptime of at least 95% between 2.11 and 14.12
- All features should be accessible through a REST api so a simulator can use the webapp.

### D. Definitions, acronyms, and abbreviations
- Karma
  - The combined sum of up-votes and down-votes. 
- Flag
  - This is a feature so that users can report a story and/or comment for being either inappropriate or offensive
- Down-vote 
  - The feature for the user to disagree/dislike the content of a story/comment.
- Up-vote 
  - This is a feature where a user can show their approval of a comment or story, whether this is based on likeability, agreement  or amusement is for the user to decide.
- Stories
  - This feature is when a user makes a new post/thread on the webapp for other users to vote or comment on
- Comments
  - Viewing a story, the user will have the feature to discuss the content by making a comment to the story itself or other users.

### E. References
Reddit and Hacker news. The webapp will draw a lot of inspirations from these sites, such as design and features.
Other forum related websites.

## 2. Current system
There is no current system, meaning we will have to develop the system from scratch.

## 3. Proposed system

### A. Overview
Forum based, Story driven social webapp. where users can share stories from all over the web or post their own personal experiences.

### B. Functional requirements
Users should be able to post links and/or stories, post comments, flag user's/comments/stories for inappropriate content or harassment.
Guests should be able to signup, login and view stories.

### C. Nonfunctional requirements

#### a. Usability
The webapp must be simple to look at, such that it is clear what links lead to. 
- It must be clear at a glance a certain story leads to an external site or the post resides in Hacker news clone itself. 
- It must be clear how many comments are attached to a given post/story and whether you are viewing a comment or the post itself.
- A menu must be available at all times from the top of the page. The menu must contain the following items:
  - A username so you can make sure you are logged on correctly.
  - A link to your profile page, where you can edit your personal information, screen name etc.
  - The name and logo of our site.

#### b. Reliability
The service must have an uptime of at least 95%, in case of downtime the server must cache incoming requests, for immediate processing hen the service is available again,  such that no data is lost.  
The site should be running on https, for maximum security, so licensing and certification should be in order. This will allow for encrypted communication between users and our servers, such that user information can not be sniffed by a third party. 
All user passwords must be hashed and salted before storage, such that our users data is secure in case of a breach of our database. 

#### c. Performance
The responsiveness of the site should be as fast as possible, and the site should be able to handle at least 100 connections at a time, both should be able to scaled with better hardware.

#### d. Supportability
Preferably all browsers and operating systems, but the major ones will be focused on. 
- Windows 7/8/10, Linux and iOS.
- Chrome, Firefox and Safari.
- Mobile and Desktop renditions.
It should also be able to run even while updating the site.

#### e. Implementation & Constraints
The server has to have a REST API for accessing functions such as publishing stories and comments. This REST API must also be accessible to a simulator program use these functions.

Additionally the system may never lose any content, even when down during maintenance.

#### f. Interface
One user page, one story page, one comment thread.
Buttons and features should be intuitive and easy to both find and use.

#### g. Packaging
The Design should be easy to use and light on the colours to make a clear cut looking site.

#### h. Legal
The site uses cookies which means we will have to get consent from ALL new users, before they can use the site.

### D. Systemmodels

#### a. Scenarios
| **Scenario name** | Bob becomes user and Comments on a story |
| --- | --- |
| **Participating actor instances** | Bob:Guest |
| **Flow of events** |1. Bob goes to the site HaxorNewsClone123.org, and sees a hilarious story, he laughs wholeheartedly and becomes infatuated with idea of acknowledging the creator of said story, for his magnificent work.<br>2. Bob goes to the signup form and fills it out. He then gets notified that a confirmation email has been sent to his email address.<br>3. Bob checks his email and waits impatiently to confirm it.<br>4. Bob then goes back to the hilarious story, and writes a Heartfelt poem about the hilarious story.<br>5. Bob then contemplates about his life and draws many comparisons to the story and his own experiences.|

| **Scenario name** | User creates a post |
| --- | --- |
| **Participating actors instances** | **Bob** :Registered User |
| **Flow of events** |1. Bob navigates to the url: HaxorNewsClone123.org to post a story.<br>2. Bob logs in by pressing the login button in the top of the screen.<br>3. Bobs gains login access by typing in his userName and password.<br>4. Bob login attempt is successful and he is redirected back to the front page.<br>5. Bob presses the write post button.<br>6. Bob is presented with a screen containing an online text editor, wherein he writes his post.<br>7. Bob clicks the &quot;post&quot; button when he is done writing the post.<br>8. Bob is redirected to his post (there same one other user would see when clicking it).<br>9. Bob collects karma and there is much rejoicing.|

| **Scenario name** | Bob becomes offended |
| --- | --- |
| **Participating actors instances** | **Bob** :Registered User |
| **Flow of events** |1. Bob navigates happily to the url: HaxorNewsClone123.org to post a story.<br>2. Bob reads the daily stories that have been posted and finds something disturbing.<br>3. Bob becomes horribly upset that someone would post such vile things on his favorit forum.<br>4. Bob uses the little flag button to complain, and writes a very strongly<br> worded complaint to the faculty at the website.<br>5. Bob then sits back in his chair, feeling very satisfied with his own work.|

#### b. Use case model

| **Use Case Name** | Guest Transitions to user and Comments on a story |
| --- | --- |
| **Participating actor instances** | Bob:Guest |
| **Flow of events** |1. The Guest sees a story.<br>2. The Guest uses the signup form to become a user.<br>3. The Guest confirms the signup through email confirmation.<br>4. The new user then goes back to the story and posts a comment.|

| **Use Case Name** | User creates a post |
| --- | --- |
| **Participating actor instances** | **Bob** :Registered User |
| **Flow of events** |1. The user logs in. <br>   a. The server validates the given data against the database. <br>   b.If a returns true, the user is logged in the HNC.<br>2. The user access the post functionality<br>3. The user is redirected to our online post/text editor.<br>4. The user submits the post.<br>5. The úser is redirected to their new post<br>   a. HNC is updated with the new post, such that it is accessible to all users and guests.|

| **Use Case Name** | Registered user reports a story |
| --- | --- |
| **Participating actors instances** | **Bob** :Registered User |
| **Flow of events** |1. A registered user sees a story that violates the terms and conditions of the site.<br>2. The Registered user then uses the report function and gives a description of the abuse.<br>3. A faculty member then reviews the story, and if deemed appropriate removes the content and the user is either temporarily banned or permanently banned. |

## 4. Glossary
