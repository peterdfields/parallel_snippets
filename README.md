## Repo for tracking useful snippets which utilize parallel to speed things up!

#### Index all the bam files in a directory using samtools
`ls *.bam | parallel samtools view -b -q 23 '{}'`
