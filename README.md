## Repo for tracking useful snippets which utilize parallel to speed things up!

#### Index all the bam files in a directory using samtools
`ls *.bam | parallel samtools index '{}'`
#### Subset all pileup files for a given contig, where `-j` controls number of cpus
`ls *pileup | parallel -j8 'grep $contig {} > {.}.tmp'`
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

##### Using pigz to compress directory (see https://biowize.wordpress.com/2012/10/18/compression-with-pigz/)
`tar cf - $DATA/*.fastq | pigz -p 16 > $WD/RawReads.tar.gz`

##### And decompress with pigz
`pigz -p 16 -d -c $WD/RawReads.tar.gz | tar x`

##### Run script multiple times without needing to apply different arguments (see https://serverfault.com/questions/513789/using-parallel-to-run-script-without-input)
`seq 3 | parallel -n0 Rscript runmodel_15b_dvariable_v3.R`

