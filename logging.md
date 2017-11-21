  #logging
  
we used Winston nodejs package to create log files.
and Filebeat, Logstash, Elasicsearch, Nginx and Kibana

<br>![alt text][kibana_1]<br>

Alerting is done with grafana, we have sat up a group for the developers and a bot. If at anytime the server is not responding, the bot wil send a message to the telegram. 
This can be expanded upon and add the oporations team, if they at anytime setup a telegram group.

Here is an example of the bot telling the dev group that there are problems on the server:


[kibana_1]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/log/kibana.png "An example of our Kibana dashboard"
[telegram_1]: https://github.com/ElDuderino420/HackerNewsClone/blob/master/log/telegram_crash_showcase.png "An example of the error message when the server is down"