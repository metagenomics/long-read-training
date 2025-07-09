
Generating Error Profiles
-------------------------

Mapping the data
^^^^^^^^^^^^^^^^

Minimap2 is a versatile sequence alignment program that aligns DNA or mRNA sequences against a large reference database. Typical use cases include: (1) mapping PacBio or Oxford Nanopore genomic reads to the human genome; (2) finding overlaps between long reads with error rate up to ~15%; (3) splice-aware alignment of PacBio Iso-Seq or Nanopore cDNA or Direct RNA reads against a reference genome; (4) aligning Illumina single- or paired-end reads; (5) assembly-to-assembly alignment; (6) full-genome alignment between two closely related species with divergence below ~15%.

We now use minimap2 to align the ONT read sets to the reference::

  cd ~/workdir
  mkdir ~/workdir/map_to_ref
  minimap2 -a -x map-ont data/Reference.fna data/basecall/basecall.fastq.gz > ~/workdir/map_to_ref/nanoporeToRef.sam

For the illumina reads we will use another aligner, as this one is more suited for this kind of data. But before we can do so, we need to create an index structure on the reference::
  
  bwa index ~/workdir/data/Reference.fna
  bwa mem -t 14 ~/workdir/data/Reference.fna ~/workdir/data/illumina/Illumina_R1.fastq.gz ~/workdir/data/illumina/Illumina_R2.fastq.gz > ~/workdir/map_to_ref/illumina.bwa.sam
  
Inferring error profiles using samtools
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After mapping the reads on the reference Genome, we can infer various statistics as e.g., number of succesful aligned reads and bases, or number of mismatches and indels, and so on. For this you could easily use the tool collection **samtools**, which offers a range of simple CLI modules all operating on mapping output (SAM and BAM format). We will use the ``stats`` module now::
 
  samtools stats -d -@ 14 ~/workdir/map_to_ref/nanoporeToRef.sam > ~/workdir/map_to_ref/nanoporeToRef.sam.stats
  samtools stats -d -@ 14 ~/workdir/map_to_ref/illumina.bwa.sam > ~/workdir/map_to_ref/illumina.bwa.sam.stats

We can inspect these results now by simply view at the top 40 lines of the output::
  
  head -n 40 ~/workdir/map_to_ref/nanoporeToRef.sam.stats
  head -n 40 ~/workdir/map_to_ref/illumina.bwa.sam.stats

Enhanced mapping statistics
^^^^^^^^^^^^^^^^^^^^^^^^^^^

To get a more in depth info on the actual accuracy of the data at hand, including the genome coverage, we're going to use a more comprehensive and interactive software comparable to FastQC which is called **Qualimap**.

First, we convert the SAM files into BAM format and sort them::

  cd ~/workdir
  samtools view -@ 4 -bS  ~/workdir/map_to_ref/nanopore.graphmap.sam | samtools sort - -@ 8 -o ~/workdir/map_to_ref/nanoporeToRef.sam.stats
  samtools view -@ 4 -bS ~/workdir/map_to_ref/illumina.bwa.sam | samtools sort - -@ 8 -o ~/workdir/map_to_ref/illumina.bwa.sorted.bam

Then we can run **qualimap** on those BAM files now::
  
  qualimap bamqc -bam ~/workdir/map_to_ref/nanopore.graphmap.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/map_to_ref/nanoporeToRef.sam.stats
  qualimap bamqc -bam ~/workdir/map_to_ref/illumina.bwa.sorted.bam -nw 5000 -nt 14 -c -outdir ~/workdir/map_to_ref/illumina.graphmap

Qualimap can also be run interactively.

References
^^^^^^^^^^

**minimap2** https://github.com/lh3/minimap2

**BWA** http://bio-bwa.sourceforge.net/

**Samtools** http://samtools.sourceforge.net/

**QualiMap** http://qualimap.bioinfo.cipf.es/doc_html/index.html
