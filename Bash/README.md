Cheat sheet for common bash commands
===

</br>

## $PATH variable
***

bash environment variables are HARDCODED into `~/.bash_profile`

appending `$PATH` variable in a bash session will not save the new `$PATH` to your bash_profile unless it is *EXPORTED*


- `echo $PATH` - view PATH variable

- `sudo nano ~/.bash_profile` - edit PATH variable in nano editor

- `export PATH="/path/to/file:$PATH"` - add this to end of ~/.bash_profile to add new variable to $PATH

- `seqtk sample -s100 read1.fq 10000 > sub1.fq` - this randomly subsets a number of reads (10k) from the original dataset for testing. Keeping the seed (-s) the same between samples ensures random reproducibility

- `ls -lh` lh flag provides information including r/w rights and file size

- `for i in x; do echo i; done` standard bash loop format

- `ls -d $PWD/*` to get the absolute path from ls

- `rsync -azP <file> <target_dir>` to transfer files with a progress bar and checking contents of receiving folder

</br>

## UNIX pattern matching
***

- `a='~/dev/dir/temp.txt'; echo $a` prints '/dev/dir/temp.txt'

- `a='~/dev/dir/temp.txt'; b=$(basename $a); echo $b` prints 'temp.txt'

- `a='~/dev/dir/temp.txt'; b=$(basename $a); echo ${b%.*}` prints 'temp'

</br>

## Conda
***

- `conda create -n <env_name>` creates a new environment

- `conda env list` lists conda environments on system

- `conda activate <env_name>` activate conda environment

- `conda deactivate` deactivate conda environment

- `conda remove --name <env_name> --all` remove conda environment

- `conda list` view list of packages and versions installed in active environment

- `conda install -n <env_name> <package>` install packages in conda env

</br>

## Docker
***

- `docker build -t <IMAGE>:<TAG> <FOLDER CONTAINING DOCKERFILE>` -- build new image from docker file

- `docker images` -- View local images

- `docker container create <IMAGE>:<TAG>` -- creates the container

- `docker ps -a` -- lists all containers *including* stopped containers

- `docker container start <CONTAINER_ID>` -- restart stopped container

- `docker run -it <IMAGE>:<TAG> /bin/bash` -- Run a container with an interactive shell

- `docker run -v <DIR TO MOUNT>:/home -it <IMAGE>:<TAG> /bin/bash` -- Run a container and mount volume

- `docker container stop <CONTAINER>:<TAG>` -- stop container

- `docker container prune` -- remove all stopped containers

- `docker image prune` -- by default, docker image prune only cleans up dangling images. A dangling image is one that is not tagged and is not referenced by any container.

- `docker image prune -a` -- To remove all images which are not used by existing containers, use the -a flag

- `docker run --rm -e PASSWORD=test -p 8787:8787 -v <DIR TO MOUNT>:/home/rstudio <IMAGE>:<TAG>` -- run rstudio in browser with docker container


</br>

## Useful Bash commands
***

- `rsync -azvP --max-size=250m --exclude '*.bam.bai' --exclude '*.bam' <SRC_DIR> <DST_DIR>` -- rsync files but excluding files matching specific patterns. Also omits large files.






