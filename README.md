# Liberate docker-machine setups with dind-machine

[docker-machine](https://github.com/docker/machine) is an easy to use and powerful tool. Provisioning and setting up cloud server with docker-machine is fast and simple. But it has one major drawback: sharing the server setup is a hassle due to missing configuration options and diverging versions. dind-machine solve this problems by offering a docker in docker based 
image for docker-machine.

## Getting Started

There are two ways you can use dind-machine. As an envirnoment inside a docker container or as shell
replacement for docker-machine. In both cases you need first to pull the docker image of dind-machine:
```
docker pull nukapi/dind-machine
```  

Now you can execute the image and use docker-machine inside it. **The option `-v ~/.dind-machine:/root/.docker/`
creates a volume mapping to the path with the docker-machine data. Do not remove it to not lose your docker-machine data! 
If you start the container without volume mapping, you will loose all certifcates docker-machine creates when you exit the container.**


```
mkdir -p ~/.dind-machine
docker run -it -v ~/.dind-machine:/root/.docker/ nukapi/dind-machine sh -l
docker-machine ls
```

To use dind-machine as docker-machine replacement you need to set an environment variable
DIND_MACHINE_DATA with the path where the docker-machine data should be stored. 

```
mkdir -p ~/.dind-machine
export DIND_MACHINE_DATA=~/.dind-machine
echo  'export DIND_MACHINE_DATA=~/.dind-machine' >> ~/.bashrc
```  
Then define an alias for dind-machine with:  
```
alias dind-machine="docker run -v $DIND_MACHINE_DATA:/root/.docker/ nukapi/dind-machine docker-machine"
echo 'alias dind-machine="docker run -v $DIND_MACHINE_DATA:/root/.docker/ nukapi/dind-machine docker-machine"' >> ~/.bashrc
```  

Now you can use docker-machine commands with dind-machine by replacing docker-machine with `dind-machine`. 
For example 
```
dind-machine ls
```
Take a look in into the [reference](https://docs.docker.com/machine/reference/)

## Take care of your data!
Backup the dind-machine setup by backing up the folder defined by DIND_MACHINE_DATA. You can also share your machine setup by sharing this folder.
