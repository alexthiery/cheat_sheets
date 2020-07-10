
### This is a cheat sheet for commands to use on CAMP

`ssh thierya@login.camp.thecrick.org` ssh into camp

`srun --ntasks=1 --cpus-per-task=2 --partition=int --time=1:00:0 --mem=16G --pty bash` request an interactive session on CAMP

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

`singularity build /camp/home/thierya/working/singularity/<IMAGE>.img docker://<IMAGE>:<TAG>` build singularity image from docker

`singularity shell <PATH_TO_IMAGE>` run shell script in container

`singularity run --bind <VOLUME_TO_MOUNT>:/home` run script through singularity container

