# How to run dalineage docker image on your local environment

## Prerequisites
  1) To run dalineage docker image on your system, **Docker Desktop** has to be installed and running. For more detail read oficial docker documentation.
     - [Get Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/)
     - [Get Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
     - [Get Docker Desktop for Linux](https://docs.docker.com/desktop/install/linux-install/)

## Run Dalineage image in container

### Extrac .zip file
  In attached zip are two files
  - image.tar archived docker image
  - data.tar archived data structure

  To extrac this `dalineage.zip` archive run
  ```
    unzip ./dalineage.zip -d .
  ```
  Or use your favorite archive program.


### Load an image from a tar archive
  Load an image or repository from a tar archive. It restores both images and tags. [Read more](https://docs.docker.com/engine/reference/commandline/load/)
  ```
    docker load --input image.tar
  ```

### Create and run a new container from an image
  To run the docker container in background mode simply run this command in your terminal. [Read more](https://docs.docker.com/engine/reference/commandline/run/)
  ```
  docker run --interactive --tty --volume data:/opt/dalineage/data --publish 81:8080 --name frontend --detach dalineage/frontend:demo
  ```
  where
  ```
  --interactive                          Keep STDIN open even if not attached
  --tty                                  Allocate a pseudo-TTY
  --volume data:/opt/dalineage/data      Create persistent storage /opt/dalineage/data
  --publish 81:8080                      Expose port 8080 on host port 81
  --name frontend                        Name the container
  --detach                               Run container in background and print container ID
  dalineage/frontend:demo                Docker image to run
  ```

  You can restart a stopped container with all its previous changes intact using [`docker start frontend`](https://docs.docker.com/engine/reference/commandline/start/). Use [`docker ps -a`](https://docs.docker.com/engine/reference/commandline/ps/) to view a list of all containers, including those that are stopped.

 ### Import prebuild data from tar file
  This step is optional. You can run container with empty structure by default. Run this command, only if you want to import some examples into public user structure.
  ```
  docker run --rm --volumes-from frontend -v $(pwd):/backup busybox sh -c "cd /opt/dalineage && rm -vrf ./data/* && tar xvf /backup/data.tar --strip 1"
  ```
  Stop container
  ```
  docker stop frontend.
  ```
  And start again.
  ```
  docker start frontend
  ```
  :warning: Once you run this command, all your data will be replaced with content in data.tar archive!!!

### Stop running container

  To stop docker container simply run. [Read more](https://docs.docker.com/engine/reference/commandline/stop/)
  ```
  docker stop frontend
  ```

### Fetch the logs of a container
  The docker container is running in background mode.
  To view container logs simply run. [Read more](https://docs.docker.com/engine/reference/commandline/logs/)
  ```
  docker logs --follow frontend
  ```
  To exit container logs use `Ctrl + c`

## Basic usage
 Once your container is up and running, you can access dalineage application on [http:localhost:81](http:localhost:81).
 By default you can find three users in this build

  - admin/admin-123     This is special user with full rights. Admin user can see all users data.
  - user/user-123       This is standard user. Standard user can see only own and public user data.
  - public/public-123   This is special user with standard rights. Public user can see own data, but everybody else can see his data.