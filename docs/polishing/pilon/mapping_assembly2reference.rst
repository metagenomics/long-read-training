Mapping the polished assembly to the reference
==============================================

To evaluate the polishing of the first assembly we will now map
the polished contigs to the reference genome using LAST. 
We will re-use the index we generated earlier for the reference.
  
Now that we have an index, we can map the assembly to the reference,
convert the output to SAM and finally to BAM format::

  cd ~/workdir/pilon
  minimap2 -a  data/Reference.fna pilon/pilon_round4.fasta > pilon4toref.sam
  samtools view -b  pilon4toref.sam > pilon4toref.bam
  samtools sort pilon4toref.bam > pilon4toref.sorted.bam
  samtools index pilon4toref.sorted.bam 

  
To look at the BAM file use::

  samtools view pilon4toref.bam | less
  

We will use a genome browser to look at the mappings. Start IGV::

  igv
  
  
Now let's look at the mapped contigs:

1. Load the reference genome into IGV. Use the menu ``Genomes->Load Genome from File...`` 
2. Load the BAM file into IGV. Use menu ``File->Load from File...`` 


References
^^^^^^^^^^

**minimap2** https://github.com/lh3/minimap2

**samtools** http://www.htslib.org

**IGV** http://www.broadinstitute.org/igv/
