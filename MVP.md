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

  - How to setup Jenkins with NodeJS, mongo and mocha
    - Create a new droplet on DO
    - ssh to the new droplet
    - sudo apt-get update (update the machine)
    - sudo apt-get install default-jre (get java, say yes to the prompt) 
    - sudo apt-get install default-jdk (say yes to the prompt)
    - install jenkins
      - sudo wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
      - sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
      - sudo apt-get update
      - sudo apt-get install jenkins
    - Now that we have jenkins running we can connect to it with <your_ip>:8080 <br>
	![alt text](https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/jenkins_img1.png "jenkins plugin")
    - to get the password use the following in the ssh client:
      - cat /var/lib/jenkins/secrets/initialAdminPassword 
    - install suggested plugins
![alt text][github-plugin]    
 - Now create the following jobs       



- Jenkins build tasks
  -  Mocha tests
  ![alt text][scm-test]

  ![alt text][bt-test]

  ![alt text][es-test]

  -  Docker image
  ![alt text][scm-docker]

  ![alt text][bt-docker]

  ![alt text][es-docker]

  -  push it to dockerHub
  -  ssh to production server (DO droplet)
  -  run the docker image
- Docker then runs the server containers
  ![alt text][docker-end]
  
[github-plugin]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-02-53.jpg "github plugin"
[scm-test]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-13.jpg "Source Code Management for backend-test"
[bt-test]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-26.jpg "Build Triggers for backend-test"
[es-test]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-31.jpg "Executive Shell"
[scm-docker]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-37.jpg "Source Code Management for backend-docker"
[bt-docker]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-45.jpg "Build triggers for backend-docker"
[es-docker]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/photo_2017-09-19_15-03-56.jpg "build environment, bindings and executive Shells"
[docker-end]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/images/Screenshot_1.png "Docker running"

