# Docker

## Docker hub account and repo

## Building customised images, microservices

```
docker --version
```
To build a test container:
```
docker run hello-world
```

Pull images
```
docker pull hello-world
```
Check available images
```
docker images
```
```
# Check docker proceses currently running
docker ps 

# List currently and previously run docker processes.
docker ps -a
```
### Shell inside the container
We can enter a container with  an interactive terminal
```
docker exec -ti <container-id> sh
```
### Execute commands
We can use the shell to perform the commands that we would like. However, we can also perform one time command from the host machine.
To do this, we can use the `exec` command:
```
docker exec -d <container-id> command
```
In the example, we removed the `index.html` file that is used as a landing page for NGINX.
```
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
