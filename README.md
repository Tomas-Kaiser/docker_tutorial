## Running Linux

Download ubuntu image from docker hub [hub.docker.com](hub.docker.com)

Instead of running `docker pull ubuntu`, we can run `docker run ubunut` and if the image exists locally, docker will start a container with this image otherwise it will pull the image in behind the scene and then start it container.

`docker ps` to see list of running processes or running containers
`docker ps -a` to see all containers (stopped containers included)
`docker run -it ubuntu` to create & start newly created container with interactive mode
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
  - etc -> one of the oppinion is that it is editable text configuration
  - home -> home directory where users are stored
  - root -> home dir for root user
  - lib -> liberies file
  - var -> variable where we store files which are updated frequently
  - proc -> includes files which represents running processes

#### Viewing commands

`nano text.txt` create/open a file and write some text
`cat text.txt` good for short text
`more text.txt` good for big text but we can just scroll down
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

`grep` global regular expression print is used to search text in one or more files
`grep -i` search case insensitive
`grep -i hello file.txt`
`grep -i hello file1.txt file2.txt`
`grep -i -r hello .` -r is recursevly or we can combined the flags
`grep -ir hello .` to find recursevly hello in current directory

#### Finding Files & Directories

`find` command is to find files and directories. If we run `find` it will print out all files and directories in the current directory recursively.
`find -type d` to find all directories only in current directory
`find -type f` to find all files only in current directory
`find -type f -name "f*"` to find all files with name which starts with f
`find -type f -iname "f*"` iname is case insensitive

#### Chaining Commads

we can use semi-colon to seperate commands like `mkdir test ; cd test ; echo done` but if the first command fails the others will be executed. If we want to stop executing when a command faild we can use and operateor `&&` like `mkdir test && cd test && echo done`

We also have or operator `||` like `mkdir test || echo "Directory exists"`

We can also pipeling like `ls /bin | less` ls /bin output will be sent to the less command. What comes from the `ls /bin` command goes to second command `less`
Another example `ls /bin | head -n 5`

We can break a line with back slash `\` like
'''
mkdir test;\
cd hello;\
echo done
'''

#### Environment Variables

`printenv` to see all environments variables in the machine
`printenv PATH` to see particular variable (everything is case sensitive)
or we can use echo but we have to include $ like `echo $PATH`
`export` to set up a variable in current terminal session eg `export DB_USER=Tomas`
`echo $DB_USER` or `printenv DB_USER` to print the db user
.bashrc is the place where we store pernament environment variables. To have access to the newly created variable you have to `exit` session or you can use `source .bashrc` if we are not in home directory then `source ~/.bashrc`

#### Managing Processes

The process is an instance of the running program.

`ps` to see all running programs/processes contains PID TTY TIME & CMD
`sleep 40 &` to sleep command for 40 sec, `&` and put into backgroud so that we can use a terminal.

`kill $PID` to kill program/process with its PID number

#### Managing Users

`useradd` to add a new user
`usermod` to modify a user
`userdel` to delete a user

`useradd -m john` the flag -m is to create a home directory. To see all flags run just `useradd`.

Users are store in a configuration file in /etc/passwd (It is missleading because there are no passwords but only account information about users)

`usermod -s /bin/bash john` to use bash instead of sh when openning a new terminal
`cat /etc/shadow` to see encripted paswords
`adduser` is a newer command for adding a new user with interactive question to get more information but in docker we use useradd instead to run the additional information under the hood.

#### Managing Groups

`groupadd`
`groupmod`
`groupdel`

`groupadd developers` to have a group for developers with the same kind of permission
`cat /etc/group` to see all groups

Each user has one primary group and zero or more suplimentery groups. Primary group is automatically created when we create a new user with the same name.

`usermod -G developers john` to add john into developers group
`usermod -G developers,artist john` to add john into multiple groups
`groups john` to see in which groups is john

#### File Permissions

`ls -l` to see the permissions of the files

`-rw-r--r-- 1 root root 11 Apr 3 15:16 deploy.sh`
`drwxr-xr-x 2 john john 4096 Apr 2 18:00 john`

First possition: `-` stands for file `d` stands for directory
After the first possition, there three groups then.
`rw- r-- r--`
`rwx-xr-x`

`r` stands for read permission
`w` stands for write permission
`x` stands for execute permission

By default all directories have `x` execute permisson as we can use `cd`

The First group represents the permission for the user who owns the file => root user in this case.

The second group repersents the permision for the group that onws that file => aslo root. By default every user that is created is automaticaly placed inside a group within the same name.

The third group is a permission for everyone else.

`./deploy.sh` to execute the file. To give a executable permission we need to use `chmod` change mode command.

`chmod u` user
`chmod g` group
`chmod o` others

`chmod u+x deploy.sh` to add execute permisson for a user
`chmod u-x` to remove execute permission for a user
`chmod og+x+r-w file1.sh file2.sh` we can write a number of variations...

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

Containers is like a virtual machine (vm).

- Provides an isolated environment for executing an application
- Can be stopped & restarted containers
- A container is "just" a spacial kind of system process!

### Docker File

The first thing to dokerize an application is adding a Dockerfile. A Dockerfile contains an instructions for building an image.

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

For React app we need a Node running on Alpine OS.
Into Dockerfile we add `FROM`:
`FROM node:14.16.0-alpine3.13`

Now we can build an image:
`docker build -t react-app .`
`-t` to tag/name an image (in this case we named our image react-app)
`.` to tell docker where to find a Dockerfile

`docker images` or `docker image ls` to see all build images!

Start a container with created image react-app.
`docker run -it react-app`
`-i` stands for interactive regime
`-t` stands for tag
`-it` it is combination of above

`docker run -it react-app sh` To run the container with shell (Alpine does not support bash as it is very small version of linux, onlu shell can be used to see files etc.)

### Copying Files & Directories

We use `COPY` & `ADD`. It is very similiar but `ADD` has additional features.

`COPY` to copy all the current files & directories of the current directory

`COPY . /app/` to copy all of the files & directories of the current directory to copy into /app/ which is an absolute path. To use relative path we have to use `WORKDIR` like:

'''
WORKDIR /app
COPY . .
'''

We can also use `ADD` which has the same syntax as `COPY` but ADD has two additional features. With ADD we can add files from url `ADD http://.../file.json .`or if we pass compressed file it will automaticaly uncompress the file `ADD file.zp .` The best practise is to use `COPY` unless we need to use those two additional features.

### Excluding Files & Directories

To exclude files & directories we have to create a file .dockerignore. It is similar to .gitignore

### Running Commands

We use a `RUN` command to execute any commands we would normally execute in a terminal session like `RUN npm install` or `RUN apt install python`.

### Setting Environment Variables

Sometimes we need to setup environemnt variables such as `ENV API_URL=http://api.myapp.com/`

### Exposing Ports

`EXPOSE 3000` to expose a container to listen to 3000

### Setting the User

We should not use root user because if we connect to the app with root user then a hacker can rewrite our app but with app user it does not have a permission to write anything. So to create a new user to run our app we can use folling commad:
`RUN addgroup app && adduser -S -G app app` to add group called app & add a user called app -S system user into -G group called app. Note: alpine linux does have useradd like ubuntu. It has just adduser command.

`USER app` to set user so that all the following commands will be executed by this user called app (not with root user as a default).
