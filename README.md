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
- Windows 7/8/10, linux and iOS.
- Chrome, Firefox and Safari.
- Mobile and Desktop renditions.
It should also be able to run even while updating the site.

#### e. Implementation & Constraints
The site should be developed in Javascript and HTML. 

#### f. Interface
One user page, one story page, one comment thread.
Buttons and features should be intuitive and easy to both find and use.

#### g. Packaging
The Design should be easy to use and light on the colours to make a clear cut looking site.

#### h. Legal
The site uses cookies which means we will have to get consent from ALL new users, before they can use the site.

### D. Systemmodels

#### a. Scenarios

#### b. Use case model

## 4. Glossary
