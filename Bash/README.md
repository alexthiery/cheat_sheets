### Cheat sheet for common bash commands


`echo $PATH` - view PATH variable

`sudo nano ~/.bash_profile` - edit PATH variable in nano editor

`export PATH="/path/to/file:$PATH"` - add this to end of ~/.bash_profile to add new variable to $PATH

`seqtk sample -s100 read1.fq 10000 > sub1.fq` - this randomly subsets a number of reads (10k) from the original dataset for testing. Keeping the seed (-s) the same between samples ensures random reproducibility
