# What's this?

Here we document the methods for assembling and analysing version 3 of the *Pochonia chlamydosporia* strain 123 genome sequence.

This genome sequence assembly has been deposited in GenBank here: https://www.ncbi.nlm.nih.gov/assembly/GCA_000411695.3 and in BioProject PRJNA68669 here: https://www.ncbi.nlm.nih.gov/bioproject/PRJNA68669/.

This work is described in the following manuscript:

Christine Sambles, Marta Suarez-Fernandez, Federico Lopez-Moya, Luis Vicente Lopez-Llorca, David J. Studholme.
Chitosan induces differential transcript usage of chitosanase 3 encoding gene (csn3) in the biocontrol fungus *Pochonia chlamydosporia* 123. Submitted.

# Genome assembly of *Pochonia chlamydosporia* strain 123
We previously generated the [PcB1v2 version draft genome assembly](https://www.ncbi.nlm.nih.gov/assembly/GCA_000411695.2) based only 
on short (Illumina sequence reads) as described in [this manuscript](10.1016/j.fgb.2014.02.002) and [this manuscript](https://doi.org/10.1111/1462-2920.15408).

Here, we describe how we generated [version PcB1v3 of the genome assembly](https://www.ncbi.nlm.nih.gov/assembly/GCA_000411695.3) and performed gene-calling on this assembly. In short, we used long reads (PacBio) to scaffold the existing contigs from the PcB1v2 assembly.
Scaffolding was performed by [SSPACE-LongRead](https://doi.org/10.1186/1471-2105-15-211). 

The SSPACE-LongRead software is described in the following publication:

Boetzer, M., & Pirovano, W. (2014). SSPACE-LongRead: scaffolding bacterial draft genomes using long read sequence information. *BMC Bioinformatics* **15**: 211. https://doi.org/10.1186/1471-2105-15-211. 
The link provided in the "Availability and requirements" section of the that paper is: http://www.baseclear.com/bioinformatics-tools/.
Unfortunately, that link is no longer valid; it gives a 404 page-not-found error (as of 1st September 2021).

So, we used the version of SSPACE-LongRead archived here: https://github.com/Runsheng/sspace_longread.
Note that you might need to modify line 104 of SSPACE-LongRead Perl script, which makes a system call to execute [Blasr](https://doi.org/10.1186/1471-2105-13-238) ```system("$Bin/blasr ...)```. 
Replace ```$Bin/blasr``` with the path to your [Blasr](https://doi.org/10.1186/1471-2105-13-238) executable.

So, we used the following command line to scaffold the assembly:

```
perl SSPACE-LongRead.pl  -c GCA_000411695.2_PcB1v2_genomic.fna -p 1-Lyophilized_subreads.fasta
```
This generated the following output:
```
Thu Jun 10 10:59:25 2021: Formatting contig sequences...
Thu Jun 10 10:59:25 2021: Aligning PacBio reads to contigs...
Executing: blasr 1-Lyophilized_subreads.fasta Longreads_scaf/intermediate_files/contigs.fa --minMatch 5 --bestn 10 --noSplitSubreads --advanceExactMatches 1 --nCandidates 1 --maxAnchorsPerPosition 1 --sdpTupleSize 7 --nproc 8 --out Longreads_scaf/intermediate_files/BLASR_results.txt
[INFO] 2021-06-10T10:59:27 [blasr] started.
Warning: resetting nCandidates to nBest 10
[INFO] 2021-06-11T00:01:11 [blasr] ended.
Fri Jun 11 00:01:12 2021: Processing alignment output...
Fri Jun 11 00:01:26 2021: Filtering contig links...
Fri Jun 11 00:01:27 2021: Scaffolding...
Fri Jun 11 00:01:50 2021: Finishing
Scaffolded 956 contigs into 312 scaffolds
```



















