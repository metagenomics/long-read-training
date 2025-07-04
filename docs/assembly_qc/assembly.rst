Assembly with flye
==================

Flye is a de novo assembler for single-molecule sequencing reads, such as those produced by PacBio and Oxford Nanopore Technologies. It is designed for a wide range of datasets, from small bacterial projects to large mammalian-scale assemblies. The package represents a complete pipeline: it takes raw PacBio / ONT reads as input and outputs polished contigs. Flye also has a special mode for metagenome assembly.
Get a usage message of canu on how to use the assembler::

	flye --help
	
	usage: flye (--pacbio-raw | --pacbio-corr | --pacbio-hifi | --nano-raw |
		     --nano-corr | --nano-hq ) file1 [file_2 ...]
		     --out-dir PATH
	
		     [--genome-size SIZE] [--threads int] [--iterations int]
		     [--meta] [--polish-target] [--min-overlap SIZE]
		     [--keep-haplotypes] [--debug] [--version] [--help] 
		     [--scaffold] [--resume] [--resume-from] [--stop-after] 
		     [--read-error float] [--extra-params] 
		     [--deterministic]
	
	Assembly of long reads with repeat graphs
	
	options:
	  -h, --help            show this help message and exit
	  --pacbio-raw path [path ...]
	                        PacBio regular CLR reads (<20% error)
	  --pacbio-corr path [path ...]
	                        PacBio reads that were corrected with other methods (<3% error)
	  --pacbio-hifi path [path ...]
	                        PacBio HiFi reads (<1% error)
	  --nano-raw path [path ...]
	                        ONT reads with odler chemistries, pre R9 Guppy5 (10-20% error)
	  --nano-corr path [path ...]
	                        ONT reads that were corrected with other methods (<3% error)
	  --nano-hq path [path ...]
	                        ONT R10 reads, aka Q20 (<3% error). For R9 Guppy5+, increase --read-error slightly
	  --subassemblies path [path ...]
	                        [deprecated] high-quality contigs input
	  -g size, --genome-size size
	                        estimated genome size (for example, 5m or 2.6g)
	  -o path, --out-dir path
	                        Output directory
	  -t int, --threads int
	                        number of parallel threads [1]
	  -i int, --iterations int
	                        number of polishing iterations [1]
	  -m int, --min-overlap int
	                        minimum overlap between reads [auto]
	  --asm-coverage int    reduced coverage for initial disjointig assembly [not set]
	  --hifi-error float    [deprecated] same as --read-error
	  --read-error float    adjust parameters for given read error rate (as fraction e.g. 0.03)
	  --extra-params extra_params
	                        extra configuration parameters list (comma-separated)
	  --plasmids            unused (retained for backward compatibility)
	  --meta                metagenome / uneven coverage mode
	  --keep-haplotypes     do not collapse alternative haplotypes
	  --no-alt-contigs      do not output contigs representing alternative haplotypes
	  --scaffold            enable scaffolding using graph [disabled by default]
	  --trestle             [deprecated] enable Trestle [disabled by default]
	  --polish-target path  run polisher on the target sequence
	  --resume              resume from the last completed stage
	  --resume-from stage_name
	                        resume from a custom stage
	  --stop-after stage_name
	                        stop after the specified stage completed
	  --debug               enable debug output
	  -v, --version         show program's version number and exit
	  --deterministic       perform disjointig assembly single-threaded

	Input reads can be in FASTA or FASTQ format, uncompressed
	or compressed with gz. Currently, PacBio (CLR, HiFi, corrected)
	and ONT reads (regular, HQ, corrected) are supported. Expected error rates are
	<15% for PB CLR/regular ONT; <3% for ONT R10, <3% for corrected, and <1% for HiFi. Note that Flye
	was primarily developed to run on uncorrected reads. You may specify multiple
	files with reads (separated by spaces). Mixing different read
	types is not yet supported. The --meta option enables the mode
	for metagenome/uneven coverage assembly.
	
	To reduce memory consumption for large genome assemblies,
	you can use a subset of the longest reads for initial disjointig
	assembly by specifying --asm-coverage and --genome-size options. Typically,
	40x coverage is enough to produce good disjointigs.
	
	You can run Flye polisher as a standalone tool using
	--polish-target option.


We will run the assembly on the small dataset, to save time. The assembly for the complete dataset will take about one hour.
We will perform the assembly as follows::

  flye --nano-raw basecall_small/basecall.fastq.gz --out-dir assembly_small --threads 14 --genome-size 2m

After that is done, inspect the results. We can get a quick view on the number of generated contigs with::

  grep '>' ~/workdir/assembly_small/assembly.fasta

**If there is time**, we start the actual assembly with all data now::

  flye --nano-raw basecall/basecall.fastq.gz --out-dir assembly --threads 14 --genome-size 2m

**Otherwise**, copy the precomputed assembly with the complete dataset into your working directory::

  cp -r ~/workdir/results/assembly_new/ ~/workdir/

and have a quick look on the number of contigs::

  grep '>' ~/workdir/assembly/assembly.fasta




References
^^^^^^^^^^

**Flye** https://github.com/mikolmogorov/Flye
  
