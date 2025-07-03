Quality control by mapping
==========================

In this part of the tutorial we will look at the assemblies by mapping
the contigs of our first assembly to the reference genome using minimap2::

  cd ~/workdir/assembly/
  minimap2 -a  ../data/Reference.fna assembly.fasta > assemblytoref.sam

Now we can convert the SAM file into the binary BAM format and add an appropriate header to the BAM
file. After that we need to sort the alignments in the BAM file by starting position (``samtools sort``)
and index the file for fast access (``samtools index``)::

  samtools view -b  assemblytoref.sam > assemblytoref.bam
  samtools sort assemblytoref.bam > assemblytoref.sorted.bam
  samtools index assemblytoref.sorted.bam
  
To look at the BAM file use::

  samtools view assemblytoref.sorted.bam | less
  
We will use a genome browser to look at the mappings. Start IGV::

  igv
  
Now let's look at the mapped contigs:

1. Load the reference genome into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 


References
^^^^^^^^^^

**LAST** http://last.cbrc.jp/

**samtools** http://www.htslib.org

**IGV** http://www.broadinstitute.org/igv/
