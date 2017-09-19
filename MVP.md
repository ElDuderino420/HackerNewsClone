A MVP of HackerNewsClone <br>
A static working http site: 188.226.152.93:3000<br>
Testing of the database 188.226.152.93:3000/api/new<br>
                        188.226.152.93:3000/api/all<br>
                        
The two database tests works where the first one creates some random data and the second gets all the current data in the database<br>

github for frontend: https://github.com/ElDuderino420/HackerNewsClone-frontend<br>
github for database: https://github.com/ElDuderino420/HackerNewsClone-backend<br>



Continuous delivery guide:<br>
Push sourcecode to github<br>
github webhooks cathes the changes which notifies jenkins<br>
Jenkins build tasks<br>
  Mocha tests<br>
  Docker image<br>
  push it to dockerHub<br>
  ssh to production server (DO droplet)<br>
  run the application<br>
Docker then runs the server containers:<br>
  
  

