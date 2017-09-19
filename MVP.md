A MVP of HackerNewsClone
A static working http site: 188.226.152.93:3000
Rest call endpoints
- 188.226.152.93:3000/api/new
- 188.226.152.93:3000/api/all
                        
The two REST calls works where the first one creates some random data and the second gets all the current data in the database

github for frontend: https://github.com/ElDuderino420/HackerNewsClone-frontend
github for backend: https://github.com/ElDuderino420/HackerNewsClone-backend


- Continuous delivery guide:
- Push sourcecode to github
- github webhooks catches the changes which notifies jenkins
![alt text][github-plugin]
- Jenkins build tasks
  -  Mocha tests
![alt text][scm-test]
<br>
![alt text][bt-test]
<br>
![alt text][es-test]
<br>
  -  Docker image
![alt text][scm-docker]
<br>
![alt text][bt-docker]
<br>
![alt text][es-docker]
<br>
  -  push it to dockerHub
  -  ssh to production server (DO droplet)
  -  run the docker image
- Docker then runs the server containers:
***

## Backend-test Jenkins


## Backend-Docker Jenkins

  
[github-plugin]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-02-53.jpg "github plugin"
[scm-test]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-13.jpg "Source Code Management for backend-test"
[bt-test]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-26.jpg "Build Triggers for backend-test"
[es-test]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-31.jpg "Executive Shell"
[scm-docker]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-37.jpg "Source Code Management for backend-docker"
[bt-docker]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-45.jpg "Build triggers for backend-docker"
[es-docker]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-56.jpg "build environment, bindings and executive Shells"
