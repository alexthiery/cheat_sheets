
### This is a cheat sheet for commands to use on CAMP

`ssh thierya@login.camp.thecrick.org` ssh into camp

`srun --ntasks=1 --cpus-per-task=8 --partition=int --time=1:00:0 --mem=64G --pty bash` request an interactive session on CAMP

`sbatch -c 8 --wrap="find . -maxdepth 1 -type d | sed '1d' | xargs -P 8 -I% sh -c 'du -hs %' > <FILE>.txt"` generate usage report for current dir


copy files to and from CAMP
===

`rsync -azP thierya@login001.camp.thecrick.org:/camp/home/thierya/scratch/10x_neural_tube/run.sh ~/Desktop/`

`rsync -azP ~/dev/repos/10x_neural_tube/run.sh thierya@login001.camp.thecrick.org:/camp/home/thierya/scratch/10x_neural_tube/`


make symlink from working to home dir
===

`cd` moves to home dir

`mkdir /camp/lab/luscomben/working/alexthiery/conda` creates a dir called conda in working

`ln -s /camp/lab/luscomben/working/alexthiery/conda .conda` makes symlink from this folder to .conda in homedir


once in interactive session
===

`ml purge` remove any loaded packages

`ml Anaconda2` load Anaconda client

`conda create -n <env_name> r-base r-essentials r-devtools` make conda env as local. envs are stored in `/camp/lab/luscomben/working/alexthiery/conda` due to symlink

`source activate <env_name>` activate conda environment



Singularity
===

`ml Singularity/3.4.2` load singularity on camp

`cd ~/.singularity` move to dir containing singularity images

`singularity pull docker://<IMAGE>:<TAG>` build singularity image from docker

`singularity shell <PATH_TO_IMAGE>` run shell script in container

`singularity run --bind <VOLUME_TO_MOUNT>:/home` run script through singularity container



FTP transfer
===

In order to transfer data using FTP on CAMP, you need to use the ncftp client

First start an interactive session

``` 
srun --ntasks=1 --cpus-per-task=1 --partition=int --time=16:00:0 --mem=16G --pty bash
```


`ml ncftp/3.2.6-foss-2016b` - load ncftp

`ncftp -u <username> <hostname>` - login using ncftp


Job submission info
===

Check submission time, start time etc.

`sacct -j <job id> --format=JobID,JobName,Elapsed,Submit,Start,End`
