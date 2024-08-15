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

## VQBG 1.1.0 Release (03 Feb 2023)


<a name="sec3"></a>
# Installation

VQBG requires a 64-bit Linux system or Mac OS and python (supported versions are python3: 3.2 and higher).

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

<!-- ## Parameters -->

<!-- ### Minimum Node Coverage

This sets the minimum node coverage for filtering the inaccurate nodes from initial assembly graph. By default, the node coverage is automatically set based on coverage distribution, which demonstrates good result among all tested datasets. Please use `-mc` flag to input the customized minimum node coverage if needed.

### Minimum Contig Length

Since SPAdes normally output all the nodes from assembly graph as contigs, short or low coverage contig may lead to less accuracy and confidence. By default, single node contig with length less than 250bp or coverage less then `--mc` (defined above) is filtered out. Please use `-ml` flag to input the customized minimum contig length if needed. -->

<a name="sec5"></a>
# Stand-alone binaries

`evals/quast_evaluation.py` is a wrapper script for strain-level experimental result analysis using [MetaQUAST](https://github.com/ablab/quast).

```
usage: metaquast.py ../he/bubble_graph/3path_str.fasta ../b-result/3ZIKV/SPAdes.fasta ../b-result/3ZIKV/VStrain.fasta ../b-result/3ZIKV/Haploflow.fa -o ../he/bubble_graph/3 -r ../A-data/3Zika-20000x/3ZIKV/1.fasta,../A-data/3Zika-20000x/3ZIKV/2.fasta,../A-data/3Zika-20000x/3ZIKV/3.fasta --unique-mapping

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

