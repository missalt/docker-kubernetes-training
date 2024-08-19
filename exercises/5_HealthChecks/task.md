# Exercise 5: Using Health Checks

Our app is now running, but we fear that the disastrous Python code it runs on might crash due to the incompetence of its developer.
Since we plan on traveling the world for the next months, we don't want to manually check for it's stability and restart it all the time. But luckily, we discover Kubernetes does that for us!

## Your task

Add a Healthcheck to our Pod which sends HTTP-Requests on the Root-Directory of Port 8080. On "bad" Error-Codes, it should restart our app. 

