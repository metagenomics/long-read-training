Inspect the output
------------------

The directory contains the following output::

  ls -l ~/workdir/basecall_tiny/
  
  total 4872
  -rw-rw-r-- 1 ubuntu ubuntu 4988554 Jun 25 19:45 calls_2025-06-25_T19-24-43.fastq


So we have one fastq file in our directory - since we started with one fast5 file. Ususally, we should merge all resulting fastq files into a single file::

  cat ~/workdir/basecall_tiny/*.fastq > ~/workdir/basecall_tiny/basecall.fastq

In order to get the number of reads in our fastq file, we can count the number of lines and divide by 4::

  cat ~/workdir/basecall_tiny/basecall.fastq | wc -l | awk '{print $1/4}'
