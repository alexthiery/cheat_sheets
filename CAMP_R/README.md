#### Create new R environment on CAMP

Create .aliases.sh file in home directory, containing custom functions
```
#!/bin/bash

function singularity_rstudio(){
	while [[ $# -gt 0 ]]
	do
	    case $1 in
	    -v|--volume)
	        local VOLUME="$2"
	        ;;
	    -c|--containder)
	        local CONTAINER="$2"
	        ;;
	   	-p|--port)
	        local PORT="$2"
	        ;;
	    esac
	    shift
	done

	link=http://$SLURMD_NODENAME.camp.thecrick.org:$PORT

	ml Singularity/2.6.0-foss-2016b
	export PASSWORD='password'
	export USERNAME=`id -un`

	echo "The RStudio-Server will be running on this address:"
	echo $link
	echo "Username:" $USERNAME
	echo "Password:" $PASSWORD

	singularity exec -c -B $VOLUME:/home/rstudio $CONTAINER rserver --www-port $PORT --www-address 0.0.0.0 --auth-none=0 --auth-pam-helper-path=pam-helper

}

function srun_default() {
	srun --ntasks=1 --cpus-per-task=2 --partition=int --time=1:00:0 --mem=16G --pty bash
}

function srun_rstudio() {
	srun --ntasks=1 --cpus-per-task=8 --partition=int --time=1:00:0 --mem=32G --pty bash
}
```

Start interactive session with 8cpus
```
srun_rstudio
```

Re-load custom functions in interactive node
```
source ~/.aliases.sh
```

Start rstudio session in singularity container
```
singularity_rstudio -v ~/scratch/repo-10x_neural_plate_border -c ~/.singularity/10x-modules-r_analysis-latest.simg -p 8787
```
