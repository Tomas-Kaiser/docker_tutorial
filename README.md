## Running Linux

Download ubuntu image from docker hub [hub.docker.com](hub.docker.com)

Instead of running `docker pull ubuntu`, we can run `docker run ubunut` and if the image exists locally, docker will start a container with this image otherwise it will pull the image in behind the scene and then start it container.

`docker ps` to see list of running processes or running containers
`docker ps -a` to see all containers (stopped containers included)
`docker run -it ubuntu` to run a container with interactive mode

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
