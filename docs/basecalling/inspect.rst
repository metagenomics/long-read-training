Inspect the output
------------------

The directory contains the following output::

  ls -l ~/workdir/basecall_tiny/
  
  total 4872
  -rw-rw-r-- 1 ubuntu ubuntu 4988554 Jun 25 19:45 calls_2025-06-25_T19-24-43.fastq


So we have one fastq file in our directory. IF there are several fastq files, we should merge them  into a single file and zip them::

  cat ~/workdir/basecall_tiny/*.fastq | gzip > ~/workdir/basecall_tiny/basecall.fastq.gz

In order to get the number of reads in our fastq file, we can count the number of lines and divide by 4::

  zcat ~/workdir/basecall_tiny/basecall_tiny.fastq.gz | wc -l | awk '{print $1/4}'
