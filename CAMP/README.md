
### This is a cheat sheet for commands to use on CAMP

`time scp -r /local_dir thierya@login001.camp.thecrick.org:/camp/home/thierya/working/alexthiery/camp_dir` - copy files from local to CAMP

`ssh thierya@login.camp.thecrick.org` ssh into camp

`srun --ntasks=1 --cpus-per-task=2 --partition=int --time=1:00:0 --mem=16G --pty bash` request an interactive session on CAMP



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

`conda activate <env_name>` activate conda environment




