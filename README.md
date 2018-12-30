# IMgc

*IMgc* reads in an aligned FASTA file, and finds and returns the largest non-recombining block of DNA sequence. This is necessary for downstream analyses that require datasets containing no evidence of recombination. *IMgc* maximizes the amount of information (as defined by the user) in the final dataset. Specifically, the user can favor retention of segregating sites versus individuals. The default is equal weighting of individuals and segregating sites.

*IMgc* was developed and reported in:

Woerner AE, MP Cox and MF Hammer. 2007. [Recombination-filtered genomic datasets by information maximization](https://doi.org/10.1093/bioinformatics/btm253). *Bioinformatics* 23: 1851-1853.

Much of this code was written by [August Woerner](https://www.unthsc.edu/bios/woerner/).

When running *IMgc*, the software first identifies segregating sites (we currently include indels in this definition). Sequence data must be haplotypic and fully phased (*i.e.*, no ambiguity codes are allowed). The following characters are permitted:

```
GATCN–
```

where *N* signifies missing data and – indicates a known indel.

Complex multi-base indels are sometimes observed; for instance:

```
ATT --T --- ATT --T
ATT --T ACA ATT --T
ATT --T ACA ATT --T
ATT --T TGT ATT --T
ATT --T TGT ATT --T
```

*IMgc* treats adjacent indels as a single unit.  For infinite sites violations, *IMgc* changes all but the two highest frequency character states at a site violating the infinite sites model to *N*. For instance, because the three adjacent positions containing indels are infinite site violations, the example above becomes:

```
ATT -T NNN ATT -T
ATT -T ACA ATT -T
ATT -T ACA ATT -T
ATT -T TGT ATT -T
ATT -T TGT ATT -T
```


INSTALLATION

*IMgc* requires a standard working Perl installation and has been confirmed to run with Perl versions up to 5.18.2.


USAGE

Assuming a standard installation (*i.e.,* with *IMGC.pl* aliased to *IMgc*), usage information can be found by running the command:

```
IMgc --help
```

The basic usage is:

```
IMgc FASTA_file(s)
```

There are, however, a number of user options that can be set:

```
-w    removes individuals; maintains original sequence length (fast)
-s    removes both individuals and sites; makes the largest block possible (slow)
-i #  weights keeping individuals versus keeping sites (default 1.1)
-o #  omits the nth ID from the FASTA file, but tacks them back on in the output (default 0)
-p #  preserves the nth ID from the FASTA file no matter what (default 0)
-f    prints output in FASTA format, not IM format
```

The *–o* option is particularly useful if the file contains outgroups.  For instance, a file of human sequences may contain a chimpanzee outgroup.  Using the *–o* option would cause *IMgc* to analyze the human sequences, excluding the chimpanzee sequence, but later append the chimpazee sequence back to the output file to allow downstream analyses.


EXAMPLE

Example [input and output files](examples) are included with this package.

[3.0cMperMb_example.fasta](examples/3.0cMperMb_example.fasta) represents an aligned DNA dataset of 96 chromosomal copies simulated under the standard *n*-coalescent with a recombination rate of 3.0 cM/Mb. Non-segregating sites have been changed to *N* for clarification, but this is not a requirement of *IMgc*.

*IMgc* can be run with the default parameters:

```
IMgc 3.0cMperMb_example.fasta
```

The output file is shown in [3.0cMperMb_example.fasta.out](examples/3.0cMperMb_example.fasta.out). This contains the largest non-recombining block obtainable from [3.0cMperMb_example.fasta](examples/3.0cMperMb_example.fasta), under the condition that individuals and segregating sites are jointly optimized.

The output format represents a file body for [Jody Hey](https://bio.cst.temple.edu/~hey/)'s [IM](https://bio.cst.temple.edu/~hey/software) class of software, but other output formats, including FASTA, are also possible.

