Cheat sheet for common bash commands
===


`echo $PATH` - view PATH variable

`sudo nano ~/.bash_profile` - edit PATH variable in nano editor

`export PATH="/path/to/file:$PATH"` - add this to end of ~/.bash_profile to add new variable to $PATH

`seqtk sample -s100 read1.fq 10000 > sub1.fq` - this randomly subsets a number of reads (10k) from the original dataset for testing. Keeping the seed (-s) the same between samples ensures random reproducibility

`ls -lh` lh flag provides information including r/w rights and file size

`for i in x; do echo i; done` standard bash loop format

`ls -d $PWD/*` to get the absolute path from ls

Pattern matching
===

`a='~/dev/dir/temp.txt'; echo $a` prints '/dev/dir/temp.txt'

`a='~/dev/dir/temp.txt'; b=$(basename $a); echo $b` prints 'temp.txt'

`a='~/dev/dir/temp.txt'; b=$(basename $a); echo ${b%.*}` prints 'temp'


Conda
===

`conda create -n <env_name>` creates a new environment

`conda env list` lists conda environments on system

`conda activate <env_name>` activate conda environment

`conda deactivate` deactivate conda environment

`conda remove --name <env_name> --all` remove conda environment

`conda list` view list of packages and versions installed in active environment

`conda install -n <env_name> <package>