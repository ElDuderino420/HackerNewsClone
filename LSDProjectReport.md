

# LSD REPORT

##Hackernews Clone

---

## Authors
- David Blum
- Marco Blum
- Alexandar Kraunsøe
- Christian Lind
- Kasper Pagh

---

Relevant links for this project:
The site can be accessed at: http://188.226.152.93/#/.

All the handins during the lsd project can be found here: https://github.com/ElDuderino420/HackerNewsClone

The frontend repository can be found here: https://github.com/ElDuderino420/HackerNewsClone-frontend 

The backend repository can be found here: https://github.com/ElDuderino420/HackerNewsClone-backend 


## System Requirements
The objective of this project is to make a clone of https://news.ycombinator.com/. 
This requires the app to have a list of features that have been extracted from here.

Functional requirements can be summed up as the following 6:
- Users should be able to create an account and login
- Users should be able to create stories or links on the website
- Users should be able to create comments to other Users stories
- Users should be able to upvote other users content.
- Users should be able to flag other users content 
- Users should not be able to downvote if they have less than 500 karma accumulated.

Non-functional requirements can be summed up as the following:
- The system needs to have an uptime of at least 95% between 11. of Nov and 14. of Dec
- There should be a REST api to post and get the latest posted story which a simulator can access.
- A Rest call that can respond Alive, Update, or Down accordingly


## Development Process
Throughout this project we, group A, have been working using parts from Scrum and XP. As Scrum is agile and always is open to changes in requirements, it was the most suitable choice for this development process.
We integrated some principles from XP, such as continuous integration and pair programming. The latter being a big factor, since it often allows us to catch errors quite quickly. One person codes while the other observes, navigates and review the code that is being made.

While using the Waterfall methodology was a possibility too, we quickly discarded it, because while some of the project's requirements were known to us beforehand, there was also a lot of unknowns and not yet requested features. This, in the end, would have messed up our development planning.  

As mentioned earlier, we’ve been using continuous integration and deployment.
In a conceptual sense, the practice of continuous integration is used to encourage development teams to commit and integrate their code, to a shared repository more frequently during the development process. Sometimes several times a day. 

Continuous deployment is an extension of continuous integration, however, it focuses on minimizing the time between a member of the development team writing some code, before it’s been tested, built and added to the live application.

To achieve this goal of continuous integration and deployment we use github, Jenkins and DockerHub. Whenever a change in the master branch of the given github repository is detected, our instance of Jenkins automatically pulls the new code from github.
This code is then tested and if it passes, a new docker image is built and uploaded to DockerHub. 
When the upload of the image is done, Jenkins creates a SSH connection to the production server and run a few select scripts, that handles the download and subsequent redeployment of the images. 

---

## System Architecture

### Subsystems
In essence our system consists of a number of distinct applications and their supporting resources.
These applications are as follows:
- Backend/server.
- The database.
- The frontend.
- Metrics collection and visualisation.

Furthermore we also have a buildserver that rebuilds the above applications whenever a change happens to their source code, as well as a repository where said source code resides. 

The buildserver and the source repository communicate via a so called webhook, which is to say, that the repository fires a POST request towards the buildserver whenever a change to a specific branch in either the backend or frontend repository is detected. 
This requests prompts the buildserver to get the latest code to test, build and deploy it.

When the frontend is up and running it’ll only communicate with the backend, in turn meaning that all communication to and from the database is handled exclusively by the backend. 

An illustration detailing the connections between the various subsystems, that make up 
our Hackernews clone (source: ill. made for earlier assignment. Org. hand-in: https://github.com/ElDuderino420/HackerNewsClone/blob/master/documentation.pdf)
Communication
The communication between the various subsystems mentioned above can be outlined as follows in this table:    

System A
Method of communication
System B
Backend
HTTP REST API
Frontend
Backend
Network connection
Database
Backend
HTTP request 
Metrics visualizer 
Metrics visualizer
HTTP request 
Alert system
Source repository
 HTTP request 
Buildserver 
Buildserver
SSH connection 
Backend
Buildserver
SSH connection 
Frontend

One arrow in the middle column indicates that the communication between system A and system B is unidirectional, while two arrows, pointing seperate directions, indicates that the data exchange has the possibility to be omnidirectional.

### Reasoning for Architecture
The reason we have chosen the system architecture outlined in the above parts, is that we wanted the different subsystems to be as independent of one another as possible.

This means that we wanted as few connections between the subsystems as we could get away with. 

Besides being easier to manage this also allows easy debugging since this, somewhat linear, system architecture makes it a breeze to isolate potential errors. 

### Software Design
In the beginning of the project, we had a pretty clear idea of how we wanted to design the system; a simple single page application with a REST API backend. We had the various REST calls figured out and documented. Since the system was a clone of an already existing system “hackernews” we also had the general design of the frontend decided.
As time passed, we learned new technologies that we wanted to implement in our system. For example, we had never worked with Docker containers before learning about them at school, so we decided to have each of our subsystems in individual Docker containers. 


The base image used for our backend was node:boron. The backend dockerfile shown below:

```Dockerfile
FROM node:boron

# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install

# Bundle app source
COPY . /usr/src/app

EXPOSE 3000
CMD [ "npm", "start" ]
```
https://github.com/ElDuderino420/HackerNewsClone-backend/blob/master/Dockerfile 


We also soon after we started working on our project got the requirements from school that Helge would be posting Posts and Comments to our web application. These requirements altered our plans for the REST calls slightly, because of the way they would be sent and handled. 
The posts and comments that were sent to us as JSON blobs, had the following format: 

```json
{"username": "<string>", 
 "post_type": "<string>", 
 "pwd_hash": "<string>", 
 "post_title": "<string>",
 "post_url": "<string>", 
 "post_parent": <int>, 
 "hanesst_id": <int>, 
 "post_text": "<string>"}
```


As we had designed the system to be able to handle request from the frontend as well as from Helge, there were a few things that made it so we had to split these into separate API calls. One would handle REST calls sent from the frontend, another would handle REST calls sent from Helge. 
Helges posts were special as they had to contain hanesst_id, which was a special id used only by Helge to keep track of our latest received post. 
They also sent a hashed version of the user's password. This was also against our first instinct when designing the application. Normally one would never send the password along with the data in the REST call, but handle with JWT(JSON Web Tokens), token based security. We ended up never implementing JWT because of this.
The data sent to us from Helge was also said to be in the format of JSON blob. This was also not according to our original design plan, normally we could send the data as pure JSON object with no need of conversion. This created some troubles when we actually had to parce to data sent to us. To fix this we created a middleware function that parses the data and adds it to the request.

```Javascript
let rawBodySaver = function (req, res, buf, encoding)
{
    if (buf && buf.length)
    {
        req.rawBody = JSON.parse(buf.toString(encoding || 'utf8'));
    }
}
```
https://github.com/ElDuderino420/HackerNewsClone-backend/blob/master/server.js 


In the end we got everything designed to work out according to the requirements.

---

## Software Implementation
Our service was set up using a node.js backend with a HTML and Angular frontend. Our backend connects to a Mongo database where we have setup schemas using the mongoose framework. Here is an example of our post schema using mongoose:

```Javascript
var postSchema = new Schema({
    user_name: {type: String},
    post_type: {type: String, required: true},
    post_title: {type: String},
    post_parent: {type: Number, required: true}, //if the post is STORY this needs to be -1 (minus one)
    hanesst_id: {type: Number},
    pwd_hash: {type: String},
    post_url: {type: String},
    post_id: {type: String},
    post_text: {type: String},
    post_upvotes: {type: Number},
    post_downvotes: {type: Number},
    total_score: {type: Number},
    post_flagged: {type: Number}, //This number counts the amount of reports this post has received
    created_at: Date,
    updated_at: Date,
    comments: {type: []}
});
```
An illustration detailing the post schema https://github.com/ElDuderino420/HackerNewsClone-backend/blob/master/models/Post.js 

Our backend is set up to retrieve and send JSON encoded data, as per the requirements. We, however were forced to change the way our backend retrieved data, as some of the data that we were given was not JSON encoded. Our way of converting this data is seen in the example below:

```Javascript
let rawBodySaver = function (req, res, buf, encoding)
{
    if (buf && buf.length)
    {
        req.rawBody = JSON.parse(buf.toString(encoding || 'utf8'));
    }
}

app.use(cors());

app.use(bodyParser.text());
app.use(bodyParser.json());
app.use(bodyParser.raw({
    verify: rawBodySaver,
    type: function ()
    {
        return true
    }
}));
app.use(bodyParser.urlencoded({
    extended: true
}));
```
An illustration detailing how we made our backend parses the incoming requests https://github.com/ElDuderino420/HackerNewsClone-backend/blob/master/server.js

The frontend was comprised of some simple HTML pages, some more simple than others (single story), and using angular to handle the logic. We used bootstrap to style the html pages as well as some of our own css. We used angular.js as none of us have experience using angular 2 or angular 4. Here is an example of one of our html pages with the angular logic behind it:

```HTML
<h2>Haxor news new post</h2>

<form name="form" ng-submit="newPost()" role="form">
    <div class="form-group">
        <label for="post_type">Post Type</label>
        <select style="padding: 0 .75rem 0 .75rem" class="form-control" ng-model="post.post_type" name="post_type" id="post_type" ng-options="type for type in types"></select>
    </div>
    <div class="form-group">
        <label for="post_title">Post Title</label>
        <input type="text" name="post_title" id="post_title" ng-model="post.post_title" class="form-control" required />
    </div>
    <div class="form-group">
        <label for="post_text">Tell us your story!</label>
        <textarea type="text" name="post_text" id="post_text" ng-model="post.post_text" class="form-control" required ></textarea>
    </div>
    <div class="form-actions">
        <button type="submit" class="btn btn-primary">Post Story</button>
        <a href="#/" class="btn btn-link">Cancel</a>
    </div>
</form>
```
An illustration detailing how we used forms to create posts on the fronend https://github.com/ElDuderino420/HackerNewsClone-frontend/blob/master/public/pages/newPost.html 

```Javascript
angular.module('haxorNews')
    .controller('newPostCtrl', function ($scope, AuthService, $location) {
        $scope.post = {
            "user_name": AuthService.currentUser(),
            "post_type": "story",
            "post_title": "",
            "post_parent": -1,
            "post_text": ""
        }
        $scope.types = ["story","poll"]

        $scope.newPost = function () {
            
            console.log($scope.post);
            if($scope.post.user_name != null){
                AuthService.createPost($scope.post,function(res){
                    $location.path('#/')
                })
            }else{
                console.log("wat")
            }
            

        }
    });
```
An illustration detailing how we used angular.js to parse a post to the factory https://github.com/ElDuderino420/HackerNewsClone-frontend/blob/master/public/js/controllers/newPostCtrl.js 

The html is just used for the visuals and calls back to our angular controller. Our angular controller then takes one of our rest calls from our backend in order to show the data on the html page.
This in turn will get relayed to the factory that holds all the functions that connect to the database, these are kept together such that in the case that if the api is changed, the functions only need to be changed one time and in one place.

```Javascript
createPost: function (post, callback) {
                $http.post(API_ENDPOINT.url + "/api/post/new", post).then(function (result) {
                    if (result != null) {
                        callback(result)
                    }
                }, function (err) {
                    if (err != null) {
                        callback(err)
                    }
                })
            }
```
An illustration detailing how the frontend calls the backend with angular a factory. https://github.com/ElDuderino420/HackerNewsClone-frontend/blob/master/public/js/factory.js 

As this project was a group project, we needed a way to work on it at the same time. We used github to allow for easy collaboration on the project allowing all our members to work simultaneously. We used git branches to make sure we wouldn’t end up with a bunch of merge conflicts, if people happened to be looking at the same files.
We used digitalocean to host our web service to the world wide web so that other people can view our service.
We used dockerhub to host our docker images. 
We tried to follow all requirements as they were given. We never changed our software choices as we always wanted to use node.js and angular.js for our backend and frontend respectively. We had to create a few changes here and there such as add indices to our backend as the post count got into the millions, however we did not change our design too much during the work process.

---

## Maintenance and SLA Status
It should be noted that the group we were operating, promised on the day we were supposed to do the handover, that their SLA would be ready within a week and that their monitoring was not running but it would be done soon.
We have since then been asking them to handover everytime we met in class and several times on sms, as that was the only contact information they ever gave us. 

### Hand-Over
We received the SLA on the 15/12/2017, we have still not received any monitoring from them. The SLA in itself is fine, but we feel the resources given to us, are inadequate to maintain their application.

### Service-Level Agreement
We looked through it and there was some noticeable disagreements that have not been resolved as of yet.
- They have written in the SLA, that they are the customer and we are the providers, which should be the other way around as we are monitoring them.
- They have stated that they would provide three distinct metrics and a grafana instance to monitor them, none of which we have been given. This means that the following were impossible to observer: 
 - Uptime
 - CPU Usage
 - Support Response Time

### Maintenance and Reliability
We could not really monitor them as mentioned before, we did however try to use Helges chart over server responses from the hanesst_id posts. We noticed fairly quickly that their server was not responding, and we backtracked through Helges mails, what their ip for their server was and found that it was not functioning properly. We notified them via sms, they confirmed the message, but it was never fixed. 

Ill. A screenshot of the group K’s responses to /latest. After the 8th of November all responses stop. (Source: http://138.68.91.198/chart.svg Hosted by Helge)

---

## Technical Discussion
Overall most of us have enjoyed the project. The idea to make a clone of an existing webapp, and have us use it for a variety of different subjects, while still maintaining the core of the app is nice.

In the start we were really enthusiastic, but when we were hit with a lot of restrictions, such as delayed requirements or forced limitations our enthusiasm plomitted. For example we would have liked to use web tokens, but with the test data sending us the password in plain text we were kinda forced to avoid it, unless we wanted to do a lot of extra work to implement it.

There were some things we liked. Even though we were building another webapp, it was used as a platform to learn a lot of different tools that can be used for multiple purposes such as: CI/CD, jenkins, virtual machines, docker, and hosting.

We also liked that the database was put under a lot of stress to make us actively look at how to optimize it. We have previously skimped on it as it did not have to host such a large dataset.

We also enjoyed the idea that it had to have a certain amount of uptime, forcing us to look at how we deployed, and really showing us the value of a tool such as jenkins. The implication that the backend was online for an extended period of time, exposed it to hacker attempts, which we have not tried previously.

All in all even though we had to make another webapp which could have been a bit repetitive, it ended up being both an informative and valued experience. 

As a side note we want to elaborate on why we think that choosing to only accept .md formats is a bad idea. It has a good implementation of code snippets and is good for readme files but in the grand scheme of things it does not even come close to other formats such as pdf when handing in a report. 
We have throughout the whole project written the handins in google docs and then spent a good half hour on trying to make it look just half as good as would have in pdf. Other editors give a multitude of better tools such as:
- Multiple editors, some with with multiple authors support such as google docs.
- Table of contents
- Footnotes
- Page numbers
- Pictures are much easier implemented in any other text editor.
- Line breaks, .md often has lines that break and some that doesn’t where one has to go through it multiple times to put <br> everywhere.
- A host of other tools and formats that are just not viable in .md

 
### Group Work Reflection & Lessons Learned
We designated roles to every member of the group, and after the first day we completely forgot about them and just worked as we liked. That system worked much better for us. The three biggest lessons we learned are as follows:
- Docker and vms

We learned a lot from having to make docker and vms, even though we might still be rudimentary in our handling of docker, we still learned the importance of having an isolated environment.
- CI/CD and jenkins

We have often used continuous integration prior to this semester without really knowing what it was, it was just our natural way of working. But after having it clarified and learning the principles and tools to deal with continuous delivery, it has been an eye opener for how easy webapp iterations can be deployed.
- Big data and security

We learned that even though mongoDB is supposed to be faster for web development, that depends on how you use it (indexes). Security is also something we learned a lot about, especially how mongoDB does not seem to have any default settings, and should be setup from the start.



