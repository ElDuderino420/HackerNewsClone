A MVP of HackerNewsClone 
A static working http site: 188.226.152.93:3000
Testing of the database 188.226.152.93:3000/api/new
                        188.226.152.93:3000/api/all
                        
The two database tests works where the first one creates some random data and the second gets all the current data in the database

github for frontend: https://github.com/ElDuderino420/HackerNewsClone-frontend
github for database: https://github.com/ElDuderino420/HackerNewsClone-backend



Continuous delivery guide:
Push sourcecode to github
github webhooks cathes the changes which notifies jenkins
Jenkins build tasks
  Mocha tests
  Docker image
  push it to dockerHub
  ssh to production server (DO droplet)
  run the application
Docker then runs the server containers:
  
  

