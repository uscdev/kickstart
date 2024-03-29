# kickstart
Instructions for getting started with ITS development infrastructure

We use the MEAN as our primary dev stack. Here are the tools to learn:

- IDE https://www.jetbrains.com/student This version is free for students. Download Webstorm.
- Git / Github (www.github.com)
- Jenkins (https://jenkins.io)
- Docker (www.docker.com)
- Node (nodejs.org)
- Angular2 (https://angular.io)

After learning these technologies, follow this tutorial to familiarize yourself with our dev environment:

The swarm is located at:
````
swarm.docker.usc.edu
````
You will need a new set of private keys to communicate with this swarm. These keys are in the swarm-tls directory of a blue USB drive located in the top-right drawer in my office.

Jenkins is now Shibbed and has also moved to the swarm. If you cannot sign-on, please send an email to me or Eric Hattemer ehatteme@usc.edu to be added to the Jenkins group. Jenkins is located at https://jenkins.docker.usc.edu
We also have a registry on the swarm.
To use the registry, push to this URI: registry.docker.usc.edu
Pulls can be done anonymously using this base URI: dtr.docker.usc.edu
Try this quick test:
Checkout this git project:

````
git clone https://github.com/uscdev/helloweb
cd helloweb
````

Build the maven project. You don't have to set up a dev environment, use the maven docker container.
For those who are new to Apache Maven. Follow the instructions. Download the binary zip archive. Download Install
````
docker run -it --rm --name my-maven-project -v "$PWD":/usr/src/mymaven -w /usr/src/mymaven maven:3.2-jdk-7 mvn clean package
````

See if you can set up and build this project in jenkins. Can you deploy to nexus? Can you rebuild on a trigger from github? Can you auto-deploy the docker image? Can you auto-deploy to the swarm?
 
Create a docker image and test it in your local environment:
````
docker build -t helloweb .
docker run --rm -p 8085:8080 helloweb
Point your web browser to:
http://localhost:8085/helloweb
````

Try to deploy the docker image to our private docker repo:
````
docker login registry.docker.usc.edu
(username: docker 
 password: on the blue USB drive)
docker tag helloweb registry.docker.usc.edu/helloweb:latest
docker push registry.docker.usc.edu/helloweb:latest
````
 
Set your local environment so you can talk to the swarm:
To make the swarm the default target for docker commands, set these params:
For Linux users
````
export DOCKER_CERT_PATH=/home/path to certs/swarm-tls
export DOCKER_TLS_VERIFY=1
export DOCKER_HOST=tcp://dcorley-swarm-mgr01.usc.edu:2376
````

For Windows users, (use powershell, not command prompt)
(Note: the $ sign is a part of the command. Do not exclude it)
````
$env:DOCKER_HOST="tcp://dcorley-swarm-mgr01.usc.edu:2376"
$env:DOCKER_CERT_PATH="/absolute path to certs/swarm-tls"
$env:DOCKER_TLS_VERIFY=1
````
(Note: replace "/absolute path to certs" with the path to the certs on your hard drive)
(Windows note: You may have to escape the backslash in your paths with \\)

(If you don’t have the private certs, see me – on the blue USB drive in the upper left drawer)
 
See if you can run this container on our swarm:
````
docker stack deploy --compose-file docker-compose.yml helloweb
````
(Take a look at the docker-compose file. Note that the anonymous registry name is different from the authenticated registry name)                                  
 
Point your browser to:
````
http://helloweb.docker.usc.edu:8085/helloweb
````

Note:- In case the service helloweb already exists, use the commands 
````
docker stack ls  # list the services
docker service ls   # list the running services
docker stack rm helloweb    # remove the listed service
````
(be very careful with "docker service rm" # do not remove any other services. ) 
