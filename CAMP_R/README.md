## Run R in Singularity container on CAMP

Pull and build singularity container from dockerhub

```
cd ~/.singularity
singularity pull docker://<IMAGE>:<TAG>
```

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



## Run R with conda env on CAMP

 load conda
 ```
 ml Anaconda2
```


create your conda env
```
 conda create -n rtest r-base r-essentials r-devtools

```


 save the following as an executable script in your home dir (rserver.sh)
 ```
#!/bin/bash

if [ -z ${SLURM_JOB_ID+x} ]; then
  echo "Please request resources first! RStudio is not supposed to be running on login nodes."
  exit 0

else

  exitfn () {
      trap SIGINT              # Restore signal handling for SIGINT
      echo; echo 'Exiting from Rstudio session'    # Growl at user,
      #scancel $SLURM_JOB_ID
	  bash
	  exit                     #   then exit script.
  }

  trap "exitfn" INT            # Set up SIGINT trap to call function. 
  port=8787  # the default port, could be generated on startup
  ip_list=`hostname -I`
  set -- $ip_list
  link=http://$1:$port
  link2=http://$SLURMD_NODENAME.camp.thecrick.org:$port
  
  
  CWD="$( cd "$( dirname "${BASH_SOURCE[0]}" )" >/dev/null && pwd )"
  USER=`whoami`
  # set a user-specific secure cookie key
  COOKIE_KEY_PATH="/tmp/${USER}_secure-cookie-key"
  rm -f $COOKIE_KEY_PATH
  mkdir -p $(dirname $COOKIE_KEY_PATH)  
  python -c 'import uuid; print(uuid.uuid4())' > $COOKIE_KEY_PATH
  # uuid > $COOKIE_KEY_PATH
  chmod 600 $COOKIE_KEY_PATH
  
  #module reset
  echo 'Loading Rstudio-server & Cairo'
  module load rstudio-server/1.2.5033-foss-2019b-Java-11
  module load cairo
  echo 'Loading Anaconda3'
  module load Anaconda3
  echo 'Loaded all modules'
  source activate rstudio

  echo ''
  echo "The RStudio-Server will be running on this address:"
  echo $link
  echo "Or"
  echo $link2

  rserver --server-daemonize=0 \
	--www-port $port \
	--secure-cookie-key-file="$COOKIE_KEY_PATH" \
	--rsession-which-r="$(which R)" \
	--auth-none=1
  trap SIGINT
fi
```

start interactive session
```
srun --ntasks=1 --cpus-per-task=2 --partition=int --time=1:00:0 --mem=16G --pty bash
```

source ~/rserver.sh
