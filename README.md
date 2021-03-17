## Repo for tracking useful snippets which utilize parallel to speed things up!

#### Index all the bam files in a directory using samtools
`ls *.bam | parallel samtools view -b -q 23 '{}'`

#### Use a custom script run pipeline with parallel
##### script has form (example taken from run fq2psmcfa utility from PSMC package):
```
#!/bin/sh

sample=$1
describer=$(echo ${sample} | sed 's/.fq.gz//')

fq2psmcfa -q20 ${describer}.fq.gz > ${describer}.psmcfa
```
##### We're using sed to remove suffix to iterate over input names and allow different suffixes for outputs
##### run script with, where `-j` controls number of cpus :
`ls *.fq.gz | parallel -j4 -k bash script.sh {}`


#### Parallelize a Bash FOR Loop
##### Sample task (good system for complex tasks or nested loops)
```
task(){
   sleep 0.5; echo "$1";
}
```
##### Parallel runs in N-process batches
```
N=4
(
for thing in a b c d e f g; do 
   ((i=i%N)); ((i++==0)) && wait
   task "$thing" & 
done
)
```
