# kickstart
Instructions for getting started with ITS development infrastructure

Give it a try: (Thanks to Varnit for reviewing and improving these instructions!)
The swarm is located at:
swarm.docker.usc.edu
You will need a new set of private keys to communicate with this swarm. These keys are in the swarm-tls directory of a blue USB drive located in the top-right drawer in my office.

Jenkins is now Shibbed and has also moved to the swarm. If you cannot sign-on, please send an email to me or Eric Hattemer ehatteme@usc.edu to be added to the Jenkins group. Jenkins is located at https://jenkins.docker.usc.edu
We also have a registry on the swarm.
To use the registry, push to this URI: registry.docker.usc.edu
Pulls can be done anonymously using this base URI: dtr.docker.usc.edu
Try this quick test:
Checkout this git project:

````
git clone https://github.com/usc-its/helloweb
cd helloweb
````

Try to push it to the nexus repo configured in the pom file:
For those who are new to Apache Maven. Follow the instructions. Download the binary zip archive.:- Download Install
````
mvn clean package
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
export DOCKER_HOST= tcp://dcorley-swarm-mgr01.usc.edu:2376
````

For Windows users, (use powershell, not command prompt)
(Note: the $ sign is a part of the command. Do not exclude it)
````
$env:DOCKER_HOST="tcp://dcorley-swarm-mgr01.usc.edu:2376"
$env:DOCKER_CERT_PATH="/absolute path to certs/swarm-tls"
$env:DOCKER_TLS_VERIFY=1
````

(If you don’t have the private certs, see me – on the blue USB drive in the upper left drawer)
 
See if you can run this container on our swarm:
````
docker service create --name helloweb -p 8085:8080 dtr.docker.usc.edu/helloweb
````
(note the anonymous registry name is different from the authenticated registry name)                                  
 
Point your browser to:
````
http://helloweb.docker.usc.edu:8085/helloweb
````

Note:- In case the service helloweb already exists, use the commands 
````
docker service ls   :- list the running services
docker service rm SERVICE_NAME    :- remove the listed service
````
(be very careful with "docker service rm":- do not remove any other services. ) 
