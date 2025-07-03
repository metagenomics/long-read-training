Mapping of Illumina reads to assembly 
-------------------------------------

We are mapping the Illumina reads to the largest contig of our assembly with BWA. BWA is a software package for mapping low-divergent sequences against a large reference genome. It consists of three algorithms: BWA-backtrack, BWA-SW and BWA-MEM. The first algorithm is designed for Illumina sequence reads up to 100bp, while the rest two for longer sequences ranged from 70bp to 1Mbp. BWA-MEM and BWA-SW share similar features such as long-read support and split alignment, but BWA-MEM, which is the latest, is generally recommended for high-quality queries as it is faster and more accurate.

The first step is to create an index on the assembly, to allow mapping::
  
  bwa index ~/workdir/assembly/assembly.fasta
  
Then we are mapping all reads to the contig. Note that we are shortening the process of creating a sorted and indexed bam file by piping the output of bwa directly to samtools, thereby avoiding temporary files::

  cd ~/workdir/
  mkdir illumina_mapping

  bwa mem -t 14 ~/workdir/assembly/assembly.fasta ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz | samtools view - -Sb | samtools sort - -@14 -o ~/workdir/illumina_mapping/mapping.sorted.bam
  
  samtools index ~/workdir/illumina_mapping/mapping.sorted.bam

  minimap2 -a  data/Reference.fna pilon/pilon_round4

We will use a genome browser to look at the mappings (to get an impression on our assembly quality). Start IGV::

  igv
  
  
Now let's look at the mapped contigs:

1. Load the reference genome into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 


References
^^^^^^^^^^

**BWA** http://bio-bwa.sourceforge.net/

**samtools** http://www.htslib.org

**IGV** http://www.broadinstitute.org/igv/

