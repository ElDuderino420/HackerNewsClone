A MVP of HackerNewsClone <br>
A static working http site: 188.226.152.93:3000<br>
Rest call endpoints
- 188.226.152.93:3000/api/new<br>
- 188.226.152.93:3000/api/all<br>
                        
The two REST calls works where the first one creates some random data and the second gets all the current data in the database<br>

github for frontend: https://github.com/ElDuderino420/HackerNewsClone-frontend<br>
github for backend: https://github.com/ElDuderino420/HackerNewsClone-backend<br>



- Continuous delivery guide:<br>
- Push sourcecode to github<br>
- github webhooks cathes the changes which notifies jenkins<br>
- Jenkins build tasks<br>
&nbsp;&nbsp;&nbsp;  -  Mocha tests<br>
&nbsp;&nbsp;&nbsp;  -  Docker image<br>
&nbsp;&nbsp;&nbsp;  -  push it to dockerHub<br>
&nbsp;&nbsp;&nbsp;  -  ssh to production server (DO droplet)<br>
&nbsp;&nbsp;&nbsp;  -  run the application<br>
- Docker then runs the server containers:<br>
  
  

