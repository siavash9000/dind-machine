#liberate docker-machine setups with dind-machine.

docker-machine is super easy and super powerful. 
Provisioning and setting up a docker swarm cluster 
with docker-machine is fast and simple.

there is only one major drawback: sharing the docker-machine setup with 
colleauges or backing up the setup is a hassle due to missing configuration 
options and diverging versions.  

dind-machine solve this problems by simply offering a docker in docker based 
image for docker-machine.

## Getting Started

1. pull the docker image of dind-machine:
`docker pull nukapi/dind-machine`  

2. define where dind-machine shall save the docker-machine data by defining the environment variable `DIND_MACHINE_DATA`:  
`mkdir ~/.dind-machine && export DIND_MACHINE_DATA=~/.dind-machine`  

3. define an alias for dind-machine with:  
`alias dind-machine="docker run -v $DIND_MACHINE_DATA=/root/.docker/ nukapi/dind-machine docker-machine"`

Now you are ready to use dind-machine! You can use all docker-machine commands with dind-machine.  

E.g. provision a small digitalocean droplet with the following command. Replace **PERSONAL_ACCESS_TOKEN** with
your DigitalOcean Personal access token.

`dind-machine create --driver digitalocean \
--digitalocean-access-token=$PERSONAL_ACCESS_TOKEN \
--engine-install-url https://raw.githubusercontent.com/rancher/install-docker/master/17.12.0.sh \
--digitalocean-size 1gb digitalocean-machine`  

Open an ssh shell on your remote server like this:  

`dind-machine ssh digitalocean-machine`  

Destroy it with:  
 
`dind-machine rm digitalocean-machine`

Backup the dind-machine setup by backing up the 
folder defined by DIND_MACHINE_DATA. You can also 
share your machine setup by sharing this folder.
