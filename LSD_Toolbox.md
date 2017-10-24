### A discription of our H4x0r news site.



## Division of Sub-systems
#### Subsystems
Our system consists of a number of different subsystems, that depend on each other for the system to function optimally. 
In our implementation we have decided on the following separation:
- Backend/server: Contains logic for interacting with our Mongo database and a REST Api for interacting with the frontend and the teacher tests.
- Mongo database: A NoSQL database, that holds all of our persistent information (such as posts and users).
- Frontend: An AngularJS application that interacts with the backend for data to populate its views.
- Buildserver/Jenkins: A separate entity that manages the deployment of all the other subsystems.
#### General strategy
The common factor for all of the above is that they all reside in individual docker containers. 
This has several distinct advantages, as opposed to deploying everything in one monolithic web application. Most notably it allow us to treat each of the aforementioned subsystems as modules, which means we can make changes to them individually without affecting the remainder of the system. A fairly common case where this is very efficient is when we need to update the frontend (eg. fixing a misspelled spring presented to the user), which is a rather common procedure. 
Now, in a traditional monolithic web application ( this would require us to rebuild the entire application from scratch, with the fix included, but with our architecture we can push the fix, and the only thing affected system would be the frontend - the server and database is unaffected.   
Another potential gain from this division of the system is that we don’t necessarily have to host the various subsystems in the same location. At the moment we host our application over three different servers in four different docker containers (the backend and database resides on the same virtual machine, still in different containers though. We did this for convenience and in an effort to limit the latency for the servers database queries).

### Backend
#### Database
This module consists of nothing but an instance of MongoDB running in a separate docker container. We connect the server to the Mongo container by finding it’s IP with the command: 
<br>**_“docker inspect $CID | grep IPAddress | cut -d '"' -f 4”_**
<br>Where $CID represents the container ID obtained through the command:
<br>**_“docker ps”_**
<br>Besides the above, nothing really exciting is going on in this part of the system.

#### Server
This module consists of a Node.js/Express http server, that contains all our API endpoints. 
<br>![alt text][server_1]<br>
To make for seamless integration between the API and the frontend  (that consumes the responses from the API), we use a combination of two strategies. Firstly we make sure to heavily comment on all the methods in our API. Below is an example of this practice
<br>![alt text][server_2]<br>
Secondly we make use of a continuously updated document, that resides in the git repositories root folder. This document, called “HackerNewsApiDocumentation” contains documentation and examples for every single endpoint in the API. Here follows an example:
<br>![alt text][server_3]<br>
Together the above practices assures we always have up-to-date and easily accessible documentation at hand for developing up against the REST API.  


### Frontend
#### Description of Modules
<br>![alt text][frontend_1]<br>

#### Nginx
This is a load balancing feature which is used to have multiple instances of backend running so that even if one goes down the site continuous. As we are using continuous integration with Jenkins, this also means that when updating the backend, it is down sequentially. So that one of the the backend instances also operates, while the other is updating.

<br>![alt text][frontend_2]<br>

## Logical Data Model
The above is our logical data model. It deviates quite a bit from the usual SQL based models. Since we use Mongo DB we have seen no sense in dividing the model into more than two parts.
<br>![alt text][lgm_1]<br>
We plan on implementing more fields in the post, at a later point (the backend is still a work in progress). Chief amongst these is the comment_parent which serves the purpose of alleviating some pressure from the backend, since finding all the replies to a heavily commented post would demand a lot of search queries to the database.   
<br>In essence a given user just submits posts, the rest is logic in the database facade.   

## Use Cases
<br>![alt text][use_case]
### A brief introduction to the use cases and actors
#### Actors
- The one most noticeble is Bob, who is a site user, which is why we will refer to Bob in most cases.
- Helge, is our other actor who only uses some of the backend.
- The last one is the Status, which is the middleman for checking if the server is down, alive or updating

#### Use Cases
- As a user you can Browse Stories posted on the site.
- As a user you can register, so the system can recognize you.
- As a registered user you can login, so that others can recognize you, and gain access to other features on the site.
- As a logged in user you can post stories and/or comments, so that other users may read them.
- As a logged in user you can upvote stories and comments and gain 1 karma per upvote.
- As a logged in user you can flag any content for spam.
- As a logged in user with more than 500 karma you can downvote stories and comments.
- As a logged in user you can logout of the system.
- As a user you can try out our own Loanbroker system from the System Integration Course.
- As a Helge you can post superfluous users.
- As a Helge you can post stories with those users containing hanesst_id.
- As a Helge you can get the latest hanesst_id post.
- As an application you can check the status of the site.

### A thorough description of the use cases

| **Use Case Name** | **Browse Stories** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Unregistered User |
| **Flow of events** |   1. While Bob is viewing the site he can browse all stories and comments. |

| **Use Case Name** | **Register User** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Unregistered User |
| **Flow of events** | 1. While Bob is viewing the site there is an button on which to register <br>2. Bob clicks the button and is asked to fill in some user information about himself.<br> 3. Bob then clicks register user after all information has been filled in. <br> 4. Bob is now a registered user. |

| **Use Case Name** | **log-in** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Registered User |
| **Flow of events** | 1. While Bob is viewing the site he sees an option to log-in to the site.<br> 2. Bob clicks this option and is required to fill in username and password <br> 3. Bob then clicks log in. <br> 4. Whilst bob is logged in other users have the option to see his presence.|

| **Use Case Name** | **LogOut** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Logged in User |
| **Flow of events** | <br>1. While Bob is logged in he can browse all stories and comments. <br> 2. Bob decides after 14 hours on the website enough is enough <br> 3. Bob doesn’t want the rest of the sites users to know of his presence any longer.<br> 4. Bob then decides to hit the logout button which removes his presence from the server.|

| **Use Case Name** | **Flag** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Logged in User |
| **Flow of events** | 1. While Bob is logged in he can browse all stories and comments. <br> 2. Bob finds something he find offensive and flags the post for lack of racism. |

| **Use Case Name** | **Loanbroker** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Logged in User |
| **Flow of events** | 1. While Bob is logged in he has the option to visit a loanbroker webservice<br> 2. Bob clicks the link and is redirected to the most amazing loanbroker service in the world. |

| **Use Case Name** | **User upvotes and downvotes** |
| :--- | :--- |
| **Participating actor instances** | **Bob** :Logged in User |
| **Flow of events** | 1. While Bob is logged he can browse all stories and comments. <br> 2. Bob finds something he likes and clicks upvote <br> 2. The creator of said story or comment then gains one karma <br> 3. Bob then browse other stories and finds some offensive content and clicks the downvote feature<br> 4. If Bob has more than 500 karma then the site accepts the downvote, if not nothing happens.|

| **Use Case Name** | **Superfluous Helge** |
| :--- | :--- |
| **Participating actor instances** | **Helge** : A Mythical Creature found in German Folklore |
| **Flow of events** | 1. Helge has access to the backend<br> 2. Helge can create as many users as he choses. |

| **Use Case Name** | **OverPowered Helge** |
| :--- | :--- |
| **Participating actor instances** | **Helge** : A Mythical Creature found in German Folklore |
| **Flow of events** | 1. Helge has access to the backend <br> 2. Helge has created a superfluous amounts of extra users. <br> 3. Helge can make posts for all of his created users spamming the site at his will. |

| **Use Case Name** | **Lost Helge** |
| :--- | :--- |
| **Participating actor instances** | **Helge** : A Mythical Creature found in German Folklore |
| **Flow of events** | 1. Helge has access to the Backend<br> 2. Helge has made so many superfluous users and posts that he can’t keep track of them himself.<br> 3. Helge can find the latest post from one of his users at will. |

| **Use Case Name** | **Status Check** |
| :--- | :--- |
| **Participating actor instances** | **Application**: Server status checker. |
| **Flow of events** | 1. As anyone you can request service status from Actor. |

## Sequence Diagram
The sequence diagram is quite big for us to share, click on the following link which allows you to zoom in and maneuver around yourself to see each individual part.

https://go.gliffy.com/go/share/s5ei0aw20l2mwrv20qjp 








[server_1]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/server_1.png "An example of one of our endpoints in the API"
[server_2]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/server_2.jpg "API documentation"
[server_3]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/server_3.png "Illustration of the backend API"
[lgm_1]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/lgm_1.png "Illustration of the Logical Data Model"
[frontend_1]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/frontend_1.png "An example of one of our restcalls made from the frontend as well as the file structure of the frontend"
[frontend_2]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/frontend_2.png "the nginx.conf file"
[use_case]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/toolbox_img/use_case_diagram.png "A complete use case diagram"