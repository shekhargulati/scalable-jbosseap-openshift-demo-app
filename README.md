## OpenShift JBoss EAP Session Replication Demo##

This is a very simple application that shows how session replication works on JBoss EAP OpenShift cartridge. 

Start by creating a scalable application.
```
$ rhc create-app scalableapp jbosseap --from-code https://github.com/shekhargulati/scalable-jbosseap-openshift-demo-app.git
```

Set the minimum number of instances of JBoss EAP. This would help us see multiple JBoss EAP instance in action.
```
$ rhc scale-cartridge --min 3 --cartridge jbosseap
```
The command above will take few minutes to finish. 

## Session Replication Demo##

1. Open your favorite browser and go to http://scalableapp-{domain-name}.rhcloud.com

2. You would see a simple page that would show session attribute "gear" value null as shown below. Please note your session id would be different.
<img src="https://whyjava.files.wordpress.com/2014/08/screen-shot-2014-08-26-at-1-24-43-am.png" height="300" width="500">

3. Now, visit the URL http://scalableapp-{domain-name}.rhcloud.com/set and application would set the $OPENSHIFT_GEAR_UUID in the session and redirect page to '/'. Now you would see a valid value in the session as shown below. Please note $OPENSHIFT_GEAR_UUID and session id value would be different for your application. 
<img src="https://whyjava.files.wordpress.com/2014/08/screen-shot-2014-08-26-at-1-30-24-am.png" height="300" width="500">

4. To view all the gear ids, you can run following command.
```
$ rhc show-app --gears --app scalableapp
```

5. Open a new web browser to simulate a new user. You would see similar behavior as mentioned in step 2. To set the value for this new session follow step 3.

6. Now, you have two different users with two different sessions. The '/' would show different values of gear.

7. Now lets test the session replication. Log in to gear one. The one that is handling your request in step 3. You can find the ssh details for that gear by typing following command.
```
$ rhc show-app --gears |grep 53fb46465973ca21b200016b
```

8. SSH into the application gear using the SSH URL you got in  step7. To stop the gear, run the <code>gear stop</code> command. This would stop the JBoss EAP container running on that gear. You could check the HAProxy status page to view gear status
<img src="https://whyjava.files.wordpress.com/2014/08/screen-shot-2014-08-26-at-1-40-59-am.png" height="200">


9.  Now, refresh your first browser, this time request would be served by other gear but you would still see the data from the session. This is because data was replicated to the second gear.

