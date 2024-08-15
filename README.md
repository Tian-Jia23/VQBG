# VQBG ：De Novo Reconstruction of Viral Quasispecies from Bubble Graphs


Manual
===========

Table of Contents
-----------------

1. [About VStrains](#sec1) </br>
2. [Updates](#sec2) </br>
3. [Installation](#sec3) </br>
   3.1. [Option 1. Quick Install](#sec3.1) </br>
   3.2. [Option 2. Manual Install](#sec3.2) </br>
   3.3. [Download & Install VStrains](#sec3.3) </br>
4. [Running VStrains](#sec4) </br>
   4.1. [Quick Usage](#sec4.1) </br>
   4.2. [Support SPAdes](#sec4.2) </br>
   4.3. [Output](#sec4.3) </br>
5. [Stand-alone binaries](#sec5) </br>
6. [Experiment](#sec6) </br>
7. [Citation](#sec7) </br>
8. [Feedback and bug reports](#sec8)</br>

<a name="sec1"></a>
# About VQBG

VStrains is a de Novo reconstruction of viral quasispecies from bubble graphs.

<!-- Please refer to our [paper](NULL) and [supplementary Material](NULL) for details methodology. -->

<a name="sec2"></a>
# Updates

## VStrains 1.1.0 Release (03 Feb 2023)


<a name="sec3"></a>
# Installation

VStrains requires a 64-bit Linux system or Mac OS and python (supported versions are python3: 3.2 and higher).

<a name="sec3.3"></a>
## Download & Install VStrains

After successfully setup the environment and dependencies, clone the VStrains into your desirable place.

```bash
git clone https://github.com/Tian-Jia23/VQBG.git
```

Install the VQBG via `Pip`

```bash
cd VQBG; pip install .
```

Run the following commands to ensure VStrains is correctly setup & installed.

```bash
vqbg -h
```

## Quick Usage

```
usage: VStrains [-h] -a {spades} -g GFA_FILE [-p PATH_FILE] [-o OUTPUT_DIR] -fwd FWD -rve RVE

Construct full-length viral strains under de novo approach from contigs and assembly graph, currently supports
SPAdes

optional arguments:
        --reads/-i <string>: the name of the file containing reads
        ** Optional :
        --kmer_length/-k <int> : length of kmer, default: 25.
        --fasta/-a : input reads file is in fasta format.
        --fastq/-q : input reads file is in fastq format.
        --output_filename/-o <string> : Name of the output file, default: paths.fasta.
        --help/-h : display the help information.


VStrains takes as input an assembly graph in Graphical Fragment Assembly (GFA) Format and associated contig information, together with the raw reads in paired-end format (e.g., forward.fastq, reverse.fastq).

<a name="sec4.2"></a>
## Support SPAdes

When running SPAdes, we recommend to use `--careful` option for more accurate assembly results. Do not modify any contig/node name from the SPAdes assembly results for consistency. Please refer to [SPAdes](https://github.com/ablab/spades) for further guideline. Example usage as below:

```bash
# SPAdes assembler example, pair-end reads
python spades.py -1 forward.fastq -2 reverse.fastq --careful -t 16 -o output_dir
```

Both assembly graph (`assembly_graph_after_simplification.gfa`) and contig information (`contigs.paths`) can be found in the output directory after running SPAdes assembler. Please use them together with raw reads as inputs for VStrains, and set `-a` flag to `spades`. Example usage as below:

```bash
vqbg -k 25 -q -o path.fasta -i forward.fastq -i reverse.fastq
```

<a name="sec4.3"></a>
## Output


VStrains stores all output files in `<output_dir>`, which is set by the user.

* `<output_dir>/aln/` directory contains paired-end (PE) linkage information, which is stored in `pe_info` and `st_info`.
* `<output_dir>/gfa/` directory contains iteratively simplified assembly graphs, where `graph_L0.gfa` contains the assembly graph produced by SPAdes after Strandedness Canonization, `split_graph_final.gfa` contains the assembly graph after Graph Disentanglement, and `graph_S_final.gfa` contains the assembly graph after Contig-based Path Extraction, the rests are intermediate results. All the assembly graphs are in [GFA 1.0 format](https://github.com/GFA-spec/GFA-spec/blob/master/GFA1.md).
* `<output_dir>/paf/` and `<output_dir>/tmp/` are temporary directories, feel free to ignore them.
* `<output_dir>/strain.fasta` contains resulting strains in `.fasta`, the headers for each strain has the form `NODE_<strain name>_<sequence length>_<coverage>` which is compatiable to SPAdes contigs format.
* `<output_dir>/strain.paths` contains paths in the assembly graph (input `GFA_FILE`) corresponding to `strain.fasta` using [Bandage](https://github.com/rrwick/Bandage) for further downstream analysis.
* `<output_dir>/vstrains.log` contains the VStrains log.
<!-- <a name="sec3.3"></a> -->
<!-- ## Parameters -->

<!-- ### Minimum Node Coverage

This sets the minimum node coverage for filtering the inaccurate nodes from initial assembly graph. By default, the node coverage is automatically set based on coverage distribution, which demonstrates good result among all tested datasets. Please use `-mc` flag to input the customized minimum node coverage if needed.

### Minimum Contig Length

Since SPAdes normally output all the nodes from assembly graph as contigs, short or low coverage contig may lead to less accuracy and confidence. By default, single node contig with length less than 250bp or coverage less then `--mc` (defined above) is filtered out. Please use `-ml` flag to input the customized minimum contig length if needed. -->

<a name="sec5"></a>
# Stand-alone binaries

`evals/quast_evaluation.py` is a wrapper script for strain-level experimental result analysis using [MetaQUAST](https://github.com/ablab/quast).

```
usage: quast_evaluation.py [-h] -quast QUAST [-cs FILES [FILES ...]] [-d IDIR] -ref REF_FILE -o OUTPUT_DIR

Use MetaQUAST to evaluate assembly result

options:
  -h, --help            show this help message and exit
  -quast QUAST, --path_to_quast QUAST
                        path to MetaQuast python script, version >= 5.2.0
  -cs FILES [FILES ...], --contig_files FILES [FILES ...]
                        contig files from different tools, separated by space
  -d IDIR, --contig_dir IDIR
                        contig files from different tools, stored in the directory, .fasta format
  -ref REF_FILE, --ref_file REF_FILE
                        ref file (single)
  -o OUTPUT_DIR, --output_dir OUTPUT_DIR
                        output directory
```

<a name="sec6"></a>

