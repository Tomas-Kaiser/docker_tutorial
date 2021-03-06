# Docker

Docker is a platform for bulding, running & shipping applications in consistent manner.

CONTAINER vs VIRTUAL MACHINE

- Container -> an isolated environment for running an application.
- VM -> an abstraction of a machine (physical hardware)

We are using Hypervisor to create VMs on the computer such as:

- VirtualBox
- VMware
- Hyper-v (Windows only)

Downsides of using VM:

- Each VM needs a full-blown OS
- Slow to start
- Resource intensive

Whereas Container has below benefits:

- Allow running multiple apps in isolation
- Are lightweight
- Use OS of the host
- Start quickly (mostly matter of seconds)
- Need less hardware resources

Docker is using Client -> Server (Docker engine) architecture. The client component is using a RESTful API.

![Docker Architecture](./images/DockerArchitecture.png)

Technically, the container is just a process like other process running on your computer. Anyway this is a special kind of process. The container does not contain a full-blown OS. Instead all containers on a host share an OS of the host. To be accurate, all containers share the karnel of the host. Win and Linux container can be run on Win OS. Linux OS can run only Linux container and Mac OS is using a special lightweight Linux VM to run a Linux container.

![OS hosts](./images/OShosts.png)

To install Docker on your OS please click on this link [docker.com/get-started](https://www.docker.com/get-started)

<b>Development workflow using Docker:</b>
To start off, we take an application and dockerize it which means we make a small change so that we can run the app by docker. We just include a Dockerfile to the app. A Dockerfile is a plain text file which include all instruction that docker uses to package up this app into an image. This image contains everything our image app needs to run.

Image typically contains:

- A cut-down OS
- A runtime environment (eg. Node)
- Application files
- Third-party libries
- Environment variables

![Docker workflow](./images/Dockerworkflow1.png)

Once we have the image we ask docker to start a container using that image. The container as I mentioned above is just a process but it is a special kind of process because it has its own file system which is provided by the image. It means our application is loaded inside of the container. When we have the image we can push the image to Docker Registery (Docker Hub) where we can store all the images and download it to any computer.

## The Linux Command Line

Docker is built on basic Linux concept therefore I include this Linux section.

Linux is open-source software and therefore we can find out many diferent Linux distributions such as:

- Ubuntu
- Debian
- Alpine
- Fedora
- CentOS and so on...

### Running Linux

Download ubuntu image from docker hub [hub.docker.com](hub.docker.com)

Instead of running `docker pull ubuntu`, we can run `docker run ubunut` and if the image exists locally, docker will start a container with this image otherwise it will pull the image in behind the scene and then start it container.

`docker ps` to see list of running processes or running containers

`docker ps -a` to see all containers (stopped containers included)

`docker run -it ubuntu` to create & start newly created container with interactive mode where `-it` stands of interactive mode and `ubuntu` is a name of the image.

`docker start -i $CONTAINER_ID` to start a container with interactive mode
`docker exec -it -u $USER_NAME $CONTAINER_ID bash` to connect into running container with different user then root with bash.

### Ubuntu

Package manager in Ubuntu is called apt Advance Package Tool.

Always run `apt update` before installing any package like `apt install nano`

Remove keyword is used for removing packages eg. `apt remove nano`

#### Linux File System

- / root directory
  - bin -> includes binaries or programs
  - boot -> includes all the files related to booting
  - dev -> stands for devices because everything is a file in linux
  - etc -> one of the oppinion is that it is editable text configuration (for storing cofing files)
  - home -> home directory where users are stored
  - root -> home dir for root user
  - lib -> liberies file
  - var -> variable where we store files which are updated frequently
  - proc -> includes files which represents running processes

#### Navigating the File System

- `pwd` is to Print Working Directory
- `ls` is to show a list of files and directories
  - `ls -1` is to show a list of files and directories in one column
  - `ls -l` is to show a long listing with more information about files and directories
- `cd` is to Change Directory following with relative or absolute path
  - `cd ..` to go one level up
- `~` is a home directory so we can use such this command `cd ~`

#### Manipulating Files and Directories

- `mkdir` is to make (create) a directory
- `mv test test1` is to rename a directory from test to test1
- `touch` is to create a new file
  - `mv` is also to use as move file to different directory such as `mv file.txt /etc`
- `rm` is to remove one or more files such as `rm file1.txt file2.txt` etc
- `rm -r` is to remove a directory and all items in the directory. The `-r` stands for recursively

Notes:

- when in shell we can remove last whole word with hitting control-w on MacOS

#### Viewing commands

We can use `nano` which is a bisic text editor for Linux.

`nano text.txt` create/open a file and write some text
`cat text.txt` good for showing a short text (`cat` stands for concatinate)
`more text.txt` good for showing a big text but we can just scroll down
`less text.txt` which is a new command and we can also scroll up but it might need to install using `apt install less`
`head -n 5 text.txt` to show first 5 lines of the file text.txt
`tail -n 5 text.txt` to show last 5 lines of the file text.txt

#### Redirection commands

Standard output is a screen & standard input is a keyboard. We can redirect this using `>` (from output to input) or `<`.

`cat` command can combined two files into one
`cat file1.txt file2.txt > combined.txt`
`echo hello > hello.txt` create a hello.txt and write hello into the file
`ls -l /etc > files.txt` list long of etc to write into files.txt

#### Searching for Text

The ways how to search for a string in a file:

- `grep` command stands for a global regular expression print is used to search text in one or more files (it is a case-sensitive)
- `grep -i` search case insensitive
- `grep -i hello file.txt` to search hello in file.txt
- `grep -i hello file1.txt file2.txt` to search in a multiple files
- `grep -i -r hello .` -r is recursevly or we can combined the flags
- `grep -ir hello .` to find recursevly hello in current directory

#### Finding Files & Directories

- `find` command is to find files and directories. If we run `find` it will print out all files and directories in the current directory recursively.
- `find -type d` to find all directories only in current directory
- `find -type f` to find all files only in current directory
- `find -type f -name "f*"` to find all files with name which starts with f
- `find -type f -iname "f*"` iname is case insensitive
- We can also sypply the path after `find` such as `find /etc -type f -iname "f*"`

#### Chaining Commads

we can use semi-colon to seperate commands like `mkdir test ; cd test ; echo done` but if the first command fails the others will be executed. If we want to stop executing when a command faild we can use and operateor `&&` like `mkdir test && cd test && echo done`

We also have or operator `||` like `mkdir test || echo "Directory exists"`

We can also pipeling like `ls /bin | less` ls /bin output will be sent to the less command. What comes from the `ls /bin` command goes to second command `less`
Another example `ls /bin | head -n 5`

We can break a line with back slash `\` like

```
mkdir test;\
> cd hello;\
> echo done
```

#### Environment Variables

- `printenv` to see all environments variables in the machine
- `printenv PATH` to see particular variable (everything is case sensitive)
  or we can use echo but we have to include $ like `echo $PATH`
- `export` to set up a variable in current terminal session eg `export DB_USER=Tomas`
- `echo $DB_USER` or `printenv DB_USER` to print the db user
- `.bashrc` is the place where we store pernament environment variables. To have access to the newly created variable you have to `exit` session or you can use `source .bashrc` if we are not in home directory then `source ~/.bashrc`. We can create
  a new variable with appending var using `>>` such as `echo DB_USER=Tomas >> .bashrc`

#### Managing Processes

The process is an instance of the running program.

- `ps` to see all running programs/processes contains PID TTY TIME & CMD
- `sleep 40 &` to sleep command for 40 sec. `&` and put into backgroud so that we can use a terminal.
- `kill $PID` to kill program/process with its PID number

#### Managing Users

- `useradd` to add a new user
- `usermod` to modify a user
- `userdel` to delete a user
- `useradd -m john` the flag -m is to create a home directory. To see all flags run just `useradd`.

Users are store in a configuration file in /etc/passwd (It is missleading because there are no passwords but only account information about users)

- `usermod -s /bin/bash john` to use bash instead of sh when openning a new terminal
- `cat /etc/shadow` to see encripted paswords
- `adduser` is a newer command for adding a new user with interactive question to get more information but in docker we use useradd instead to run the additional information under the hood.

#### Managing Groups

- `groupadd`
- `groupmod`
- `groupdel`

- `groupadd developers` to have a group for developers with the same kind of permission
- `cat /etc/group` to see all groups

Each user has one primary group and zero or more suplimentery groups. Primary group is automatically created when we create a new user with the same name.

- `usermod -G developers john` to add john into developers group
- `usermod -G developers, artist john` to add john into multiple groups
- `groups john` to see in which groups is john

#### File Permissions

- `ls -l` to see the permissions of the files

- `-rw-r--r-- 1 root root 11 Apr 3 15:16 deploy.sh`
- `drwxr-xr-x 2 john john 4096 Apr 2 18:00 john`

First possition: `-` stands for file and `d` stands for directory
After the first possition, there three groups then.

- `rw- r-- r--`
- `rwx-xr-x`

- `r` stands for read permission
- `w` stands for write permission
- `x` stands for execute permission

By default all directories have `x` execute permisson as we can use `cd`

The First group represents the permission for the user who owns the file => root user in this case.

The second group repersents the permision for the group that onws that file => aslo root. By default every user that is created is automaticaly placed inside a group within the same name.

The third group is a permission for everyone else.

- `./deploy.sh` to execute the file. To give a executable permission we need to use - - - `chmod` change mode command.

- `chmod u` user
- `chmod g` group
- `chmod o` others

- `chmod u+x deploy.sh` to add execute permisson for a user
- `chmod u-x` to remove execute permission for a user
- `chmod og+x+r-w file1.sh file2.sh` we can write a number of variations...

## Building Images

This section is about:

- Creating Docker files
- Versioning images
- Sharing images
- Saving and loading images
- Reducing the image size
- Speeding up builds

### Images & Containers

Images include everyting what application needs to run. It contains:

- A cut-down OS
- Third-party libraries
- Application files
- Environment variables
- etc.

Once we have an image we can start a contairner from it.

Containers are like a virtual machine (vm).

- Provides an isolated environment for executing an application
- Containers can be stopped & restarted
- A container is "just" a spacial kind of system process because it has own file system which is porvided by the image!

### Docker File

The first thing to dokerize an application is adding a Dockerfile. A Dockerfile contains an instructions for building an image using below commands:

- FROM
- WORKDIR
- COPY
- ADD
- RUN
- ENV
- EXPOSE
- USER
- CMD
- ENTRYPOINT

### Choosing the Right Images

The base image can be an OS system like linux or windows. Also it can be OS + runtime environment.

For React app we need a Node running on Alpine OS.

Into Dockerfile we add `FROM`:

- `FROM node:14.16.0-alpine3.13` > Node version 14.16.0 on top of the alpine3.13 (a very small linux).
- Do not use the `:latest` as it can caused some issue in the future. It is always better to use a specific version

`docker images` or `docker image ls` to see all build images!

Now we can build an image:

- `docker build -t react-app .`
- `-t` to tag/name an image (in this case we named our image react-app)
- `.` to tell docker where to find a Dockerfile

Start a container with created image react-app with interactive mode.

- `docker run -it react-app sh` (Alpine does not support bash as it is very small version of linux, only shell can be used to see files etc.). If we omit the shell then we will be inside of node environment.
- `-i` stands for interactive regime
- `-t` stands for tag
- `-it` it is combination of above

### Copying Files & Directories

We use `COPY` & `ADD`. It is very similiar but `ADD` has additional features.

- `COPY` to copy all the current files & directories of the current directory
- `COPY . /app/` to copy all of the files & directories of the current directory to copy into /app/ which is an absolute path. If the /app directory does not exist, the docker will automatically create it. If we want to copy two or more files/directories we have to end it with / (eg. /app<b>/</b>). To use relative path we have to use `WORKDIR` like:

```
WORKDIR /app  > we set up that working directory is in /app directory
COPY . .      > therefore first . is the source and the second . is a destination which is currently in /app
```

If the file contains a space we can use special syntax ["","",""] as follow:

- `COPY ["Hello World.txt", "."]`

We can also use `ADD` which has the same syntax as `COPY` but ADD has two additional features. With ADD we can add files from url `ADD http://.../file.json .`or if we pass compressed file it will automaticaly uncompress the file `ADD file.zp .` The best practise is to use `COPY` unless we need to use those two additional features.

### Excluding Files & Directories

To exclude files & directories we have to create a file `.dockerignore`. It is similar to .gitignore

### Running Commands

We use a `RUN` command to execute any commands we would normally execute in a terminal session such as:

- `RUN npm install`
- `RUN apt install python`

### Setting Environment Variables

Sometimes we need to setup environemnt variables such as

- `ENV API_URL=http://api.myapp.com/`

### Exposing Ports

- `EXPOSE 3000` to expose a container to listen to 3000

### Setting the User

We should not use a root user because if we connect to the app with root user then a hacker can rewrite our app but with app user it does not have a permission to write anything. So to create a new user to run our app we can use folling commad:

- `RUN addgroup app && adduser -S -G app app` to add group called app & add a user called app -S system user into -G group called app. The first app after -G is a name of the group we have created before && and the second app is the username.

Note: alpine linux does not have useradd like ubuntu. It has just adduser command.

`USER app` to set user so that all the following commands will be executed by this user called app (not with root user as a default).

### Defining Entrypoints

`docker run react-app npm start` to start a container and start a react app inside of the container. If we do not want to everytime specify this command `npm start`, we have to use a command instruction `CMD` or `ENTRYPOINT` command in dockerfile.

Then we can simply run `docker run react-app`. Note `CMD` is suplying a default command so we can use it just once (the very last will be used if more CMD commands are in dockerfile).

<b>RUN vs. CMD</b>

Because of both these can execute commands. The `RUN` instruction is a build time instruction (executing in a time of building the image) in contrast `CMD` instruction is a run time instruction (executing when starting a container).

`CMD` construction has to forms:

- `CMD npm start` is a shell form (it spins up a new shell terminal)
- `CMD ["npm", "start"]` is a exec (execute) form

The difference is when execude this `CMD npm start` docke will execute this command inside of separete shell. In linux this is in /bin/sh in windows CMD. The best practise is to use execute form because of this we can execute this command `cmd ["npm", "start"]` directly and there is no need to spin up another shell process. Also it makes it easier and faster to clean up resources when you stop containers. ALWAYS use the execute form!

We also have another instruction called `ENTRYPOINT` which is very similar to `CMD`. It also have shell & exec form. The difference is that we can always override the default command `CMD` when starting a container like `docker run react-app echo hello` and this `echo hello` will overrider the default `CDM` in dockerfile wherease we cannot easily override a `ENTRYPOINT` when running a container. If we want it we have to use --entrypoint option `docker run react-app --entrypoint`.

### Speed Up Builds

To speed up builds we have to modify our dockerfile. We should orgenise a dockerfile from the top to bottom based on a frequence of changing files (from stable instructions to changing instructions).

We `COPY` the package.json and package-lock.json first then we `RUN` npm install because most likely we do not have to reinstall third party dependecies so that docker will take those info from cache memory. If we make some changes then it will be reinstalled.

```
COPY package*.json .
RUN npm install
COPY . .
```

### `Removing Images`

To remove unsued images and containers we can use following commands:

- `docker container prune` is to remove all stopped containers.
- `docker image prune` is to remove dangling images (images which lost connection to image - just empty uname images. It happents when we rebuild images)

To remove images we can use name or id

- `docker image rm $name`
- `docker image rm $ID`

### Tagging images

Use alaways explicit tag in stage & production environment. It does not care for development environment. In default if we do not supply a tag then it is automatically the latest.

- `docker build -t react-app:1 .` during the build time
- ``docker image tag react-app:1 react-app:2` change tag name after building an image

The latest tag does not nessacary mean that it is the latest!

### Sharing images

We can use docker hub to share our images. It works similar like GitHub.

First the name image must match with the name of repo on Docker Hub eg. tomas/react-app The repo can contain one or more tags.

- `docker login`
- `docker push tomas/react-app:1`

### Saving & Loading Images

We can save an image without using hub as follow:

- `docker image save -o react-app.tar react-app:3`
- `-o` stands for output we can see more option if we run `docekr image save --help`
- .tar it is compressed file like .zip in windows

- `docker image load -i react-app.tar` to load a compressed image

## Working with Containers

In this section we will look at:

- Starting & stopping containers
- Publishing ports
- Viewings logs
- Excuting commands in containers
- Removing containers

### Starting Containers

We can start a conationer with following command

- `docker run react-app`

but in this case we cannot use a terminal so in order to be able to use a terminal we can use `-d` which stands for detach.

- `docker run -d react-app` each container has own generated name so if we want to have a custom name then we can use an option `--name` as follow

- `docker run -d --name <my_container_name> react-app`.

### Viewing Logs

To see what happens with container we can use logs as follow:

- `docker logs $CONTAINER_ID`
- `docker logs -f $CONTAINER_ID` to continuoesly see the logs
- `docker logs -n 5 -t $CONTAINER_ID` to see last 5 lines (-n 5) with timestamp (-t)
- `docker logs --help` to learn more

### Publishing Ports

If we do not expose the port inside of Dockerfile then we have to do it when running the container as below:

- `docker run -d -p 80:3000 --name c1 react-app` to run a container on port 80 and the app inside of the container is mapped to 3000
- port 80 is mapped to port 3000 inside of container.

### Executing Commands in Running Containers

To execute any commands in running container we can use the following command

- `docker exec $CONTAINER_ID ls` exec stands for execute. We can use container ID or the name of the container.

- `docker exec -it $CONTAINER_ID sh` to use interactive mode with shell session. Instead of sh we can use bash too if the linux support it.

### Stopping & Starting Containers

- `docker stop $CONTAINER_ID` we can stop a running container
- `docker start $CONTAINER_ID` we can start a stopped container

Note: `docker run` is to create and start a new container.

### Removing Containers

We have two options:

- `docker container rm $NAME` or we can use container id instead of the name
- `docker rm $NAME` shorter version with omitting the container keyword
- `dokcer rm -f $NAME` in case the container is running we can force to remove it with -f or just stop the container and remove as mentioned above.
- `docker ps -a | grep $name` to search in all containers a specific name of the container. It works just for linux.

### Persisting Data Using Volumes

We should not store any data in container file system because whenever we delete the contaier the file system will be also deleted therefore we should use volumes to store persisting data (outside of the container).

Volume is a storage outside of the containers. It can be directly on the host or somewhere on the cloud.

- `docker volume crate app-data` to create a volume named app-data
- `docker volume inspect app-data`

- `docker run -d -p 4000:3000 -v app-data:/app/data react-app` -v stands for volume to pass data in app-data to the directory in a container /app/data. Any changes there will be stored outside of the container so if we delete a container and create a new one we will still have the data which we created in the previous container there.

### Copying Files between the Host and Containers

- `docker cp $CONTAINER_ID:/app/log.txt .` to copy a log file to current directory on the host. It works vice-versa too
- `docker cp secret.txt $CONTAINER_ID:/app`

### Sharing the Source Code with a Container

`docker run -d -p 5001:3000 -v $(pwd):/app react-app` to map our project directory with the app directory in the container. If we change our source code in host project directory then the app direcotry in the container will be updated too. It is a good for development environment. It is not suitable for stage or produciton environment where we should build a new image and use the image to build a container.

## Running Multi-container Apps

In this section we will cover following:

- Docker compose
- Docker networking
- Database migration
- Running automated tests

The way how to clean up docker containers & images:

- `docker container rm -f $(docker container ls -aq)` -q stands for for listing only container ids
- `docker image rm -f $(docker images -q)`

To run multi container application like (React in frontend, nodeJS in backend and MongoDB), we can run just one following command `docker-compose up`.

### JSON & YML Formats

We use JSON for exchanging data & YML (YAML, both is correct). We use YML for configuration files because it is a bit slower when parsing data.

JSON file:

```
{
  "name": "The Ultimate Docker Course",
  "price": 149,
  "is_published": true,
  "tags": ["software", "devops"],
  "author": {
    "first_name": "Mosh",
    "last_name": "hamedani"
  }
}
```

YML file:

```
---
name: The Ultimate Docker Course
price: 149
is_published: true
tags:
  - sofware
  - devops
author:
  first_name: Mosh
  last_name: hamedani
```

### Creating a Compose file

Create a file called `docker-compose.yml` in the root dir.

Setting up the compose-file.yml:

1. version: 3.8 // It is a version of compose file format see more: [compose file](https://docs.docker.com/compose/compose-file/)

```
version: "3.8" # it is supposed to be valuated as string not a number

services: # each service should have own docker file (the naming of the services does not matter)
  web:
    build: ./frontend # this is where we have a docker file
    ports:
      - 3000:3000
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly # connecting to mongo db
  db:
    # here we are not using a docker file like above. We use an image
    # from docker hub
    image: mongo:4.0-xenial
    ports:
      - 27017:27017 # this is a default port for db
    volumes:
      - vidly:/data/db # to store data outside of the container

volumes: # we have to define volumes first before we can use it (weird syntax)
  vidly:
```

### Build Images

- `docker-compose build` to build the images.

Note: we have to be in the root directory where the docker-compose.yml is located.

### Starting and Stopping the Application

- `docker-compose up -d` to start the application (containers) with the detach mode (-d)
- `docker-compose down` to stop the application (containers)

### Docker Networking

When we run docker-compose. It automatically creates a network and adds that containers on that network so these containers can talk to each other.

- `docker network ls` to see all the networks

### Viewing Logs

`docker-compose logs` to see all logs from all containers
`docker logs <container_id>` to see logs from particular running container. We can supply `-f` to follow logs

### Publishing Changes

To update the host and container with a new code without rebuilding the images we can use volumes as we have already used it above. In this case we can use dokcer-compose.yml:

```
services:
  web:
    build: ./frontend
    ports:
      - 3000:3000
    volumes:
      - ./frontend:/app   # To map ./frontend of the host to /app in the container
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly
    volumes:
      - ./backend:/app   # To map ./backend of the host to /app in the container
```

### Migrating the Database

We add command to run shell scirpt in order to migrate DB data.

```
services: # each service should have own docker file (the naming of the services does not matter)
  web:
    build: ./frontend # this is where we have a docker file
    ports:
      - 3000:3000
    volumes:
      - ./frontend:/app
  api:
    build: ./backend
    ports:
      - 3001:3001
    environment:
      DB_URL: mongodb://db/vidly # connecting to mongo db
    volumes:
      - ./backend:/app
    command: ./docker-entrypoint.sh # to runs shell script to migrate db data
```

### Running Tests

`npm test` to run automation tests but we can also run the tests inside of the container but it might be very slow. We update our docker-compose as follow:

```
services: # each service should have own docker file (the naming of the services does not matter)
  web:
    build: ./frontend # this is where we have a docker file
    ports:
      - 3000:3000
    volumes:
      - ./frontend:/app
  web-test:
    build: vidly_web
    volumes:
      - ./frontend:/app
    command: npm test # this overwrite the default command for this image
  ...
```
