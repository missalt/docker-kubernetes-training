# Exercise 1: Building your first Image

In this task, your goal is to create a Container Image using Docker and push it to DockerHub for later use.

## Creating your Image 

Our goal is to package the Python-Code in the /src-Directory into a Container. Create a Dockerfile that achieves this in the Root-Directory (/) of this Repository.

Then, run the Image locally using *docker run -d -p 8080:8080 <image_name>* and open *http://localhost:8080* to test it. 

If you succeeded in this task you should be greeted by a "Hello, World!" message.

## Publishing your Image

While your newly created Image can be used from local storage using Docker, using it in Kubernetes Clusters (including minikube) requires you to publish it to a public or private repository. To achieve this, create a Docker Account (or ask your instructor for credentials if you don't want to), Login and navigate to "Repositories" -> "Create repository".

You now have to "re-tag" your image using the following command: 

*docker tag <local_image_name> <docker_username>/<repository_name>:<image_tag>*

Now, you can push your image to the Hub using the *docker push <full_name>*. Note that the full_name includes the namespace (your username), the repository and tag as above. Re-Run your image from the external repository to confirm everything is working.