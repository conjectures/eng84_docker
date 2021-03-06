# Docker

## Contents
- [Install](#install)
- [Docker Hub](#docker-hub)
  - [Creating account](#creating-account)
  - [Connecting with CLI](#connecting-with-cli)
- [Docker Runtime](#docker-runtime)
  - [Testing the runtime](#testing-the-runtime)
  - [Pulling images](#pulling-images)
  - [Docker processes](#docker-processes)
  - [Shell inside containers](#shell-inseide-containers)
  - [Execute commands](#execute-commands)
  - [Copying files between host and container](#copying-files-between-host-and-container)
  - [Creating image from a container](#creating-image-from-a-container)
- [Custom Images](#custom-images)
- [Multi Stage Builds](#multi-stage-builds)


## Install
The steps to install the docker engine for Linux can be found [here](https://docs.docker.com/engine/install)
For [ubuntu](https://docs.docker.com/engine/install/ubuntu/) it can be installed with `apt` or with a convenience script.
After installing, `docker` should be added to the user's group so that regular docker commands don't require escalating privileges.
The guide to performing that can be found [here](https://docs.docker.com/engine/install/linux-postinstall/).

We can check if docker is properly installed by checking its version
```sh
docker --version
```
## Docker Hub
Docker Hub is an online repository for docker images. It can be used to store images on the cloud, share them to your team, or in public.

### Creating account
We can create a docker hub account so that we can store our custom images on the cloud.
To create the account, follow the steps [here](https://hub.docker.com/).

After creating the account, the images that are uploaded can be found on your repository page

![Docker Hub Repo page](dockerhub_panel.png)

### Connecting with CLI 
After creating an account, we can connect with it locally, using the docker cli.
When we have connect docker with cli in our machine, we can pull or push private images.

To log-in from a machine, use the `login` comand:
```bash
docker login
```
The credentials to the login can be stored in a **Credentials store**. For Linux, we can use the `pass` keychain manager.
If installed, docker can get the credentials from the keychain automatically, after the `.docker/config.json` file is [configured correctly](https://docs.docker.com/engine/reference/commandline/login/#credentials-store)

## Docker Runtime

### Testing the runtime
When we run a container, we specify the image of the container we want to run.
If the image is not in in the local repository on our host, it will pull it from docker hub, an online repository of docker images.

For example we can run a test container:
```
docker run hello-world
```
If this is the first time we ran this container, it will probably not be in our image list.
To check the current images, we can use:
```
docker images
# or
docker image ls
```
After the image is ran for the first time, we can find the original image on the above list.


### Pulling images
We can pull available images without running a container with them. To do this, we use the `pull` command.
> Note: To pull images, we don't require an account or login credentials, unless the image is private.

```
docker pull hello-world
```

### Docker processes
After running a lot of containers, we can check which of them are currently running:
```
docker ps 
```
We can also check for previously run containers by specifying the `-a` flag:
```
# List currently and previously run docker processes.
docker ps -a
```
To clean the docker processes, that are not used, we can use `docker rm` command:
```
docker rm <container-id>
```
To delete all unused containers from history, we can use the following command:
```sh
docker rm $(docker ps -qa)
```
> Note: In place of the container ID, we can also use the name that is assigned or generated for the container.

After all containers are removed that use a particular image, it can be removed from the images list:
```sh
docker rmi <image>
```

If the container was run with the `-rm` flag, then when it is stopped, it will be automatically removed from the processes list.
For example, we could have ran the `hello-world` container with the `-rm` flag:
```sh
docker run --name test_rm_flag -rm hello-world
```
Now, when the container executes, it will automatically stop as there is no other process running, and it will be removed from the history list.
The `--name` flag is used to assing a name to the container, instead of getting a randomly generated one.

### Shell inside containers
We can enter a container with  an interactive terminal
```
docker exec -ti <container-id> sh
```
### Execute commands
We can use the shell to perform the commands that we would like. However, we can also perform one time command from the host machine.
To do this, we can use the `exec` command:
```sh
docker exec -d <container-id> command
```
In the example, we removed the `index.html` file that is used as a landing page for NGINX.
```bash
docker exec -d nginx_container rm /usr/share/nginx/html/index.html
```

### Copying files between host and container
To copy a file between the host and the container, we can't use an `exec` command, as those are used to perform an action inside the container.
Instead, we use the `cp` subcommand that can copy a file from our host machine to the container:
```
docker cp index.html <container-id>:<destination-path>
```
In our case, we created a custom landing page, in `index.html` removed

> Note: To restart the nginx process inside the container we can use `nginx -s reload`

### Creating image from container

```bash
docker commit <container-id> <repository>/<image_name>:<tag>
```
After we perform the above command, we will be able to find the image in our image list.
In our example, we have created an image from the custom nginx container, with the updated landing page and named it `eng84_nginx`:
![docker custom image shown in list](docker_custom_image_ls.png)


## Custom Images
The creation of docker images can be automated with the use of a **Dockerfile**.
Dockerfiles are usually named `Dockerfile`

...

## Multi Stage Builds
We can create multi stage builds for dockerfiles so that the overall size of the container is reduced.
This is achieved by using a large container to perform the necessary builds for our application, then moving the build binaries to a smaller container that doesn't even include package managers.


...
