Project Overview

Part 1- Dockerize it

1. How was Docker and it's dependencies installed?

To install Docker and all it's dependencies, I had to setup a docker file within this project. I also had to download an older version of docker as the newest version would not run on my system. 
Within the docker file, I have essentially a script of commands written in order to install an apache webserver within the container, making port 80 the exposed port, and loading up my index.html file so it is viewable. 
The docker file looks like this: 

FROM ununtu:latest
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get -y update
RUN apt-get install -y apache2 curl
EXPOSE 80
WORKDIR /var/www/html
COPY index.html /var/www/html/index.html
ENTRYPOINT ['/usr/sbin/apache2ctl"]
CMD ["-D", "FOREGROUND"]

2. How to build the container?

From there, I followed the tutorial that accompanied the download for docker and used the terminal that was apart of the docker GUI. 

3. How to run the container?

I used the command: docker run -d --name nicoleschool apacheserver to get the docker image to run in the browser.

4. How to view the project? (open a browser, go to ip and port...)

Part 2- GitHub Actions and DockerHub

1. Process to create DockerHub public repo. 

To create a DockerHub repo, first you must make a docker account. A benefit to making the account is the ability to save the image as well as sharing it on their site. 
To create the repository, click on Repositories at the top of the page in your docker hub account. 
Then click create repository. 
We must fill out the repo name and description as well as making the repo public. Then click create at the bottom. 
I am able to push a new tag to this repo using the following command: 
docker push nicoleschool/nicoleschoolrepo:tagname

2. Allow DockerHub authenication via CLI using DockHub credentials

To do this, first we must ensure we are logged into our hub.docker.com account. 
We will click on the username in the upper right-hand corner and select account settings. 
Then we have to click on Security on the left-hand side and select to create a new access token. 
We have to give the token a description and leave the access permission at read, write, delete. 
Click generate once complete. 
The token, once generate, should be copied and placed in a safe place. This token is your password. And it is vital that it is copied because once this window is closed, you cannot retrieve the token or view it in your settings. 
To use this token from the docker CLI, we can use the following command: 
docker login -u nicoleschool

3. Configure GitHub Secrets

Then we want to add this token to our github secrets API. 
To do this we will go to our settings in github, go to Secrets, new repo secret, name the secret, enter docker credentials as the value, and you're done. If you have problems finding the correct settings section, like me, it is right above the repo above the green code button. 

   
4. Configure Github Workflow

To configure the gitflow, we would need to change the repository in order to build and push docker images to dockerHub. 

Part 3- Deployment

In order to configure the workflow, we have to start by making a clone of our EC2 instance and build the image onto this. 
The following commands are how we are going to pull, build the image in the container, and verify that we did not make any mistakes along the way. 
docker build -t apacheserver
docker run -d --name nicoleschool apacheserver
docker exec nicoleschool curl localhost