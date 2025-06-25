The Tutorial Data Set
================================

As you have started the VM with a volume attached, this volume needs to be given to the ubuntu user for easy access::

  sudo chown ubuntu:ubuntu /mnt/
  
Create a link in your home directory to the mounted volume::

  ln -s /mnt/ workdir 

The tutorial dataset is located in our object store. We have also prepared some precomputed results.You can get both here::

  cd ~/workdir
  wget https://s3.bi.denbi.de/cmg/mgcourses/longread2025/Data.tar.gz
  wget https://s3.bi.denbi.de/cmg/mgcourses/longread2025/Results.tar.gz

Then, unpack the tar archive::

  tar -xzvf Data.tar.gz
  tar -xzvf Results.tar.gz

and remove the tar archives::

  rm Data.tar.gz
  rm Results.tar.gz  

Have a short look, on what is contained within the data directory::



  ls -l ~/workdir/data/
  -rw-r--r-- 1 ubuntu ubuntu   6849457 Aug 30  2019 Reference.fna  
  drwxr-xr-x 2 ubuntu ubuntu     20480 Aug 30  2019 fast5
  drwxrwxr-x 2 ubuntu ubuntu      4096 Sep  5  2019 fast5_small
  drwxrwxr-x 2 ubuntu ubuntu      4096 Sep 12  2019 fast5_tiny
  drwxr-xr-x 2 ubuntu ubuntu      4096 Aug 30  2019 illumina
  -rw-rw-r-- 1 ubuntu ubuntu 117496419 Jun 24 10:28 pb_cov_20.fq.gz
  -rw-rw-r-- 1 ubuntu ubuntu 293425053 Jun 24 10:27 pb_cov_50.fq.gz

There are three folders with Nanopore fast5 data, a Reference genome for later comparison, some illumina data and simulated PacBio data.

If you want to disable system beep sounds::

  xset -b
