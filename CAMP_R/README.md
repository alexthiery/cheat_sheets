#### Create new R environment on CAMP

login to CAMP
``` bash
ssh thierya@login.camp.thecrick.org
```

start interactive session on cluster
``` bash
srun --ntasks=1 --cpus-per-task=2 --partition=int --time=1:00:0 --mem=16G --pty bash
```

load modules
``` bash
ml purge
ml Anaconda2
ml R/3.6.0-foss-2019a
```

create conda environment
``` bash
conda create -n <env_name> r-base r-essentials r-devtools
```

load conda environment
``` bash
source activate <env_name>
```

install renv in conda environment
``` bash
conda install -c conda-forge r-renv
```

cd to directory where r environment is to be created
``` bash
cd <dir_name>
```

start R interactively
``` bash
R
```

intialise renv
``` R
renv::init()
```

exit and restart R - whenever R is loaded from this directory, the custom r environment will be loaded
``` R
q()
```
``` bash
R
```

install required R packages
``` R

install.packages('devtools')
devtools::install_github("juliendelile/Antler")

install.packages('dplyr')
install.packages('cowplot')
install.packages('clustree')
install.packages('grid')
install.packages('gridExtra')
install.packages('pheatmap')

install.packages('BiocManager')
BiocManager::install('multtest')
install.packages('Seurat')
```

then exit
```R
q()
```
``` bash
exit
```

***
#### Running script in sbatch mode

make job script establishing sbatch conditions

this requires activating the above conda environment, and calling the r script form the directory with the r environmnent installed
``` bash
#SBATCH -t 6:00:00
#SBATCH --mem=0
#SBATCH -c 10
#SBATCH --job-name=10x_neural
#SBATCH --mail-type=ALL,ARRAY_TASKS
#SBATCH --mail-user=alex.thiery@crick.ac.uk

ml purge
ml Anaconda2
ml R/3.6.0-foss-2019a
source activate 10x_new
cd ~/working/alexthiery/analysis/10x_scRNAseq_2019/test_repo
Rscript ../repo/scripts/1_seurat_full.R CAMP
```

login to CAMP
``` bash
ssh thierya@login.camp.thecrick.org
```

copy job script to CAMP
``` bash
scp Desktop/job.sh /camp/lab/luscomben/working/alexthiery/analysis/dir
```

run job script
``` bash
sbatch /camp/lab/luscomben/working/alexthiery/analysis/dir/job.sh
```









