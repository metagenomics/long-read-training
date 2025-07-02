Merge fastqs
------------

And again, we are merging all fastq files::

  cat ~/workdir/basecall/*.fastq | gzip > ~/workdir/basecall/basecall.fastq.gz
  cat ~/workdir/basecall_small/*.fastq > ~/workdir/basecall_small/basecall.fastq.gz
  
If you want, you can check again for the number of reads::

  zcat ~/workdir/basecall_small/basecall.fastq.gz | wc | awk '{print $1/4}'
  or 
  zcat ~/workdir/basecall/basecall.fastq.gz | wc | awk '{print $1/4}'
