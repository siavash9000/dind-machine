# Liberate docker-machine setups with dind-machine

[docker-machine](https://github.com/docker/machine) is an easy to use and powerful tool. Provisioning and setting up cloud server with docker-machine is fast and simple. But it has one major drawback: sharing the server setup is a hassle due to missing configuration options and diverging versions. dind-machine solve this problems by offering a docker in docker based 
image for docker-machine.

## Getting Started

1. pull the docker image of dind-machine:  
```
docker pull nukapi/dind-machine
```  
2. Define the path DIND_MACHINE_DATA to the sensitive docker-machine data:  
```
mkdir -p ~/.dind-machine
export DIND_MACHINE_DATA=~/.dind-machine
echo  'export DIND_MACHINE_DATA=~/.dind-machine' >> ~/.bashrc
```  
3. define an alias for dind-machine with:  
```
alias dind-machine="docker run -v $DIND_MACHINE_DATA:/root/.docker/ nukapi/dind-machine docker-machine"
echo 'alias dind-machine="docker run -v $DIND_MACHINE_DATA:/root/.docker/ nukapi/dind-machine docker-machine"' >> ~/.bashrc
```  

Now you are ready to use dind-machine! You can use all docker-machine commands with dind-machine by replacing docker-machine with `dind-machine`. Take a look in into the [reference](https://docs.docker.com/machine/reference/)
## Examples

Replace `PERSONAL_ACCESS_TOKEN` with your digitalocean personal access token in the following command and provision a small digitalocean droplet:  

```
dind-machine create --driver digitalocean \  
--digitalocean-access-token=PERSONAL_ACCESS_TOKEN \  
--engine-install-url https://raw.githubusercontent.com/rancher/install-docker/master/17.12.0.sh \  
--digitalocean-size 1gb digitalocean-machine  
```  
Open an ssh shell on your remote server like this:  

`dind-machine ssh digitalocean-machine`  

Destroy it with:  
 
`dind-machine rm digitalocean-machine`  

## Take care of your data!
Backup the dind-machine setup by backing up the folder defined by DIND_MACHINE_DATA. You can also share your machine setup by sharing this folder.
