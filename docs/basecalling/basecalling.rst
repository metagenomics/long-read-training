Basecalling with dorado
-------------------------

Dorado is the newest version of the ONT basecaller, it replaced guppy which was the basecaller for quite a long time. Dorado is capable of multiple steps: demultiplexing, adapter removal, basecalling calling of modified bases (methylation), and other things. Look up the comprehensive documentation for further information.


The command we are using for for basecalling with dorado is::

  dorado basecaller
  
Let's have a look at the usage message for `dorado basecaller`::

  dorado basecaller --help
  
  Usage: dorado [--help] [--verbose]... [--device VAR] [--models-directory VAR] [--bed-file VAR] [--recursive] [--read-ids VAR] [--max-reads VAR] [--resume-from VAR] [--min-qscore VAR] [--emit-moves] [--emit-fastq] [--emit-sam] [--output-dir VAR] [--reference VAR] [--mm2-opts VAR] [--modified-bases VAR...] [--modified-bases-models VAR] [--modified-bases-threshold VAR] [--modified-bases-batchsize VAR] [--kit-name VAR] [--sample-sheet VAR] [--barcode-both-ends] [--barcode-arrangement VAR] [--barcode-sequences VAR] [--primer-sequences VAR] [--no-trim] [--trim VAR] [--estimate-poly-a] [--poly-a-config VAR] [--batchsize VAR] [--chunksize VAR] [--overlap VAR] model data
  
  Positional arguments:
    model                       Model selection {fast,hac,sup}@v{version} for automatic model selection including modbases, or path to existing model directory. 
    data                        The data directory or file (POD5/FAST5 format). 
  
  Optional arguments:
    -h, --help                  shows help message and exits 
    -v, --verbose               [may be repeated]
    -x, --device                Specify CPU or GPU device: 'auto', 'cpu', 'cuda:all' or 'cuda:<device_id>[,<device_id>...]'. Specifying 'auto' will choose either 'cpu', 'metal' or 'cuda:all' depending on the presence of a GPU device. [nargs=0..1] [default: "auto"]
    --models-directory          Optional directory to search for existing models or download new models into. [nargs=0..1] [default: "."]
    --bed-file                  Optional bed-file. If specified, overlaps between the alignments and bed-file entries will be counted, and recorded in BAM output using the 'bh' read tag. [nargs=0..1] [default: ""]
  
  Input data arguments (detailed usage):
    -r, --recursive             Recursively scan through directories to load FAST5 and POD5 files. 
    -l, --read-ids              A file with a newline-delimited list of reads to basecall. If not provided, all reads will be basecalled. [nargs=0..1] [default: ""]
    -n, --max-reads             Limit the number of reads to be basecalled. [nargs=0..1] [default: 0]
    --resume-from               Resume basecalling from the given HTS file. Fully written read records are not processed again. [nargs=0..1] [default: ""]
  
  Output arguments (detailed usage):
    --min-qscore                Discard reads with mean Q-score below this threshold. [nargs=0..1] [default: 0]
    --emit-moves                Write the move table to the 'mv' tag. 
    --emit-fastq                Output in fastq format. 
    --emit-sam                  Output in SAM format. 
    -o, --output-dir            Optional output folder, if specified output will be written to a calls file (calls_<timestamp>.sam|.bam|.fastq) in the given folder. 
  
  Alignment arguments (detailed usage):
    --reference                 Path to reference for alignment. [nargs=0..1] [default: ""]
    --mm2-opts                  Optional minimap2 options string. For multiple arguments surround with double quotes. 
  
  Modified model arguments (detailed usage):
    --modified-bases            A space separated list of modified base codes. Choose from: pseU, m6A_DRACH, m6A, 6mA, m5C, 5mC, 5mCG_5hmCG, 5mCG, 5mC_5hmC, inosine_m6A, 4mC_5mC. [nargs: 1 or more] 
    --modified-bases-models     A comma separated list of modified base model paths. [nargs=0..1] [default: ""]
    --modified-bases-threshold  The minimum predicted methylation probability for a modified base to be emitted in an all-context model, [0, 1]. 
    --modified-bases-batchsize  The modified base models batch size. 
  
  Barcoding arguments (detailed usage):
    --kit-name                  Enable barcoding with the provided kit name. Choose from: EXP-NBD103 EXP-NBD104 EXP-NBD114 EXP-NBD114-24 EXP-NBD196 EXP-PBC001 EXP-PBC096 SQK-16S024 SQK-16S114-24 SQK-LWB001 SQK-MAB114-24 SQK-MLK111-96-XL SQK-MLK114-96-XL SQK-NBD111-24 SQK-NBD111-96 SQK-NBD114-24 SQK-NBD114-96 SQK-PBK004 SQK-PCB109 SQK-PCB110 SQK-PCB111-24 SQK-PCB114-24 SQK-RAB201 SQK-RAB204 SQK-RBK001 SQK-RBK004 SQK-RBK110-96 SQK-RBK111-24 SQK-RBK111-96 SQK-RBK114-24 SQK-RBK114-96 SQK-RLB001 SQK-RPB004 SQK-RPB114-24 TWIST-16-UDI TWIST-96A-UDI VSK-PTC001 VSK-VMK001 VSK-VMK004 VSK-VPS001. [nargs=0..1] [default: ""]
    --sample-sheet              Path to the sample sheet to use. [nargs=0..1] [default: ""]
    --barcode-both-ends         Require both ends of a read to be barcoded for a double ended barcode. 
    --barcode-arrangement       Path to file with custom barcode arrangement. Requires --kit-name. 
    --barcode-sequences         Path to file with custom barcode sequences. Requires --kit-name and --barcode-arrangement. 
    --primer-sequences          Path to file with custom primer sequences. 
  
  Trimming arguments (detailed usage):
    --no-trim                   Skip trimming of barcodes, adapters, and primers. If option is not chosen, trimming of all three is enabled. 
    --trim                      Specify what to trim. Options are 'none', 'all', and 'adapters'. The default behaviour is to trim all detected adapters, primers, and barcodes. Choose 'adapters' to just trim adapters. The 'none' choice is equivelent to using --no-trim. Note that this only applies to DNA. RNA adapters are always trimmed. [nargs=0..1] [default: ""]
  
  Poly(A) arguments (detailed usage):
    --estimate-poly-a           Estimate poly(A)/poly(T) tail lengths (beta feature). Primarily meant for cDNA and dRNA use cases. 
    --poly-a-config             Configuration file for poly(A) estimation to change default behaviours [nargs=0..1] [default: ""]
  
  Advanced arguments (detailed usage):
    -b, --batchsize             The number of chunks in a batch. If 0 an optimal batchsize will be selected. [nargs=0..1] [default: 0]
    -c, --chunksize             The number of samples in a chunk. [nargs=0..1] [default: 10000]
    --overlap                   The number of samples overlapping neighbouring chunks. [nargs=0..1] [default: 500]
  

Beside the path of our fast5 files (-i), the basecaller requires an output path (-s) and a model file. In order to get a list of possible models, we use::

  dorado download --list

We are interested in the simplex models only::

[2025-06-25 19:15:34.532] [info] > simplex models
[2025-06-25 19:15:34.532] [info]  - dna_r9.4.1_e8_fast@v3.4
[2025-06-25 19:15:34.532] [info]  - dna_r9.4.1_e8_hac@v3.3
[2025-06-25 19:15:34.532] [info]  - dna_r9.4.1_e8_sup@v3.3
[2025-06-25 19:15:34.532] [info]  - dna_r9.4.1_e8_sup@v3.6
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_fast@v3.5.2
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_hac@v3.5.2
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_sup@v3.5.2
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_fast@v3.5.2
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_hac@v3.5.2
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_sup@v3.5.2
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_fast@v4.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_hac@v4.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_sup@v4.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_fast@v4.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_hac@v4.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_sup@v4.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_fast@v4.1.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_hac@v4.1.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_260bps_sup@v4.1.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_fast@v4.1.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_hac@v4.1.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_sup@v4.1.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_fast@v4.2.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_hac@v4.2.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_sup@v4.2.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_fast@v4.3.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_hac@v4.3.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_sup@v4.3.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_fast@v5.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_hac@v5.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_400bps_sup@v5.0.0
[2025-06-25 19:15:34.532] [info]  - dna_r10.4.1_e8.2_apk_sup@v5.0.0
[2025-06-25 19:15:34.532] [info]  - rna002_70bps_fast@v3
[2025-06-25 19:15:34.532] [info]  - rna002_70bps_hac@v3
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_fast@v3.0.1
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_hac@v3.0.1
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_sup@v3.0.1
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_fast@v5.0.0
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_hac@v5.0.0
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_sup@v5.0.0
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_fast@v5.1.0
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_hac@v5.1.0
[2025-06-25 19:15:34.532] [info]  - rna004_130bps_sup@v5.1.0


Our dataset was generated using an r9.4.1 flowcell, so we use the dna_r9.4.1_e8_hac@v3.3 model (hac = high accuracy).

We download the model with::

  cd ~/workdir
  dorado download --model dna_r9.4.1_e8_hac@v3.3


We need to specify the following options:

+------------------------------------------------------------------------+-------------------------+--------------------------------+
| What?                                                                  | parameter               | Our value                      |
+========================================================================+=========================+================================+
| The model file for our flowcell/kit combination                        | positional 1            | dna_r9.4.1_e8_hac@v3.3         |
+------------------------------------------------------------------------+-------------------------+--------------------------------+ 
| fastq output                                                           | --emit-fastq                                             |
+------------------------------------------------------------------------+-------------------------+--------------------------------+
| The full path to the directory where the raw read files are located    | positional 2            | ~/workdir/data/fast5_tiny      |
+------------------------------------------------------------------------+-------------------------+--------------------------------+
| The full path to the directory where the basecalled files will be saved| -o                      | ~/workdir/basecall_tiny/       |
+------------------------------------------------------------------------+-------------------------+--------------------------------+

Our complete command line is::

  cd ~/workdir
  dorado basecaller dna_r9.4.1_e8_hac@v3.3/ data/fast5_tiny --emit-fastq -o basecall_tiny



References
^^^^^^^^^^
**dorado** https://github.com/nanoporetech/dorado

