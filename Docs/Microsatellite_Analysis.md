## Sawfish Structure Variation Detection
### Dependence           
1. Haplotagged BAM Files
```
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/Data/EwingSampleList.txt
```
### Individual Discover
```
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/IndividualCall/output/discover
```
### Individual Call (Results)
1. "genotyped.sv.vcf.gz" is our target file
```
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/IndividualCall/output/call
```
<img src="https://github.com/user-attachments/assets/cd7c5b7c-d235-4e8a-99ce-73ae413008ae" width="350">

2. Other outputs explaination
  * ${OUTPUT_DIR}/**sawfish.log** - High level logging output
  * ${OUTPUT_DIR}/**run_stats.json** - Run statistics and component timings
  * ${OUTPUT_DIR}/**contig.alignment.bam** - Contigs for assembled SV haplotypes aligned back to the reference genome

3. Structure Variation General Output (VCF) Interpretation
<img src="https://github.com/user-attachments/assets/218151e6-766c-47ee-9e3c-dc03c76215ae" width="500">



### Joint Call (Results)
1. Completed
```
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/JointCall/output/call
```

2. **Explanation**: The joint call step takes the output of the sawfish 'discover' step for one to many samples and provides jointly genotyped SV calls over the sample set. Joint calling includes the following operations:
    * Merge duplicate SV haplotypes
    * Associate deduplicated SV haplotypes with samples
    * Evaluate SV read support in each sample
    * Genotype quality assessment and VCF output
```
https://github.com/PacificBiosciences/sawfish/blob/main/docs/user_guide.md#getting-started
```

3. **Comments**: Since the major motivation of joint call is merging multiple samples together rather pedigree-based additional layer of analysis, I think it's worth for us to run it to have the whole picture inside one file. 

### Supported SV Types
1. Deletions
2. Insertions
3. Duplications
4. Breakpoints
5. Inversions
   
For details, please check
```
https://github.com/PacificBiosciences/sawfish/blob/main/docs/user_guide.md#getting-started
```
### Notice
1. The SV calling result has been phased
2. Check Tag "PS" for phasing info

### VCF Format Description
Each single event contains 4 major sections, including
  * FILTER
  * ALT
  * INFO
  * FORMAT

For details, please check the explanation below
```
1: FILTER
##FILTER=<ID=PASS,Description="All filters passed">
##FILTER=<ID=.,Description="Unknown filtration status">
##FILTER=<ID=InvBreakpoint,Description="Breakpoint represented as part of an inversion record (same EVENT ID)">
##FILTER=<ID=MinQUAL,Description="QUAL score is less than 10">
##FILTER=<ID=MaxScoringDepth,Description="SV candidate exceeds max scoring depth">
##FILTER=<ID=ConflictingBreakpointGT,Description="Genotypes of breakpoints in a multi-breakpoint event conflict in the majority of cases">

2: ALT
##ALT=<ID=DEL,Description="Deletion">
##ALT=<ID=INS,Description="Insertion">
##ALT=<ID=DUP:TANDEM,Description="Tandem Duplication">
##ALT=<ID=INV,Description="Inversion">

3: INFO
##INFO=<ID=END,Number=1,Type=Integer,Description="End position of the variant described in this record">
##INFO=<ID=EVENT,Number=A,Type=String,Description="ID of associated event">
##INFO=<ID=EVENTTYPE,Number=A,Type=String,Description="Type of associated event">
##INFO=<ID=HOMLEN,Number=.,Type=Integer,Description="Length of base pair identical micro-homology at event breakpoints">
##INFO=<ID=HOMSEQ,Number=.,Type=String,Description="Sequence of base pair identical micro-homology at event breakpoints">
##INFO=<ID=IMPRECISE,Number=0,Type=Flag,Description="Imprecise structural variation">
##INFO=<ID=INSLEN,Number=.,Type=Integer,Description="Insertion length">
##INFO=<ID=INSSEQ,Number=.,Type=String,Description="Insertion sequence (for symbolic alt alleles)">
##INFO=<ID=MATEID,Number=.,Type=String,Description="ID of mate breakends">
##INFO=<ID=SVLEN,Number=.,Type=Integer,Description="Difference in length between REF and ALT alleles">
##INFO=<ID=SVTYPE,Number=1,Type=String,Description="Type of structural variant">

4: FORMAT
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=GQ,Number=1,Type=Integer,Description="Genotype Quality">
##FORMAT=<ID=PL,Number=G,Type=Integer,Description="Phred-scaled genotype likelihoods rounded to the closest integer">
##FORMAT=<ID=AD,Number=R,Type=Integer,Description="Read depth for each allele">
##FORMAT=<ID=PS,Number=1,Type=Integer,Description="Phase set identifier">
```

### Reference
1. Sawfish version
   * v0.12.9
   * Notice: the old version doesn't work (e.g. v0.12.1, core dump error)
```
https://github.com/PacificBiosciences/sawfish/releases/tag/v0.12.9
```
2. Sawfish github
```
https://github.com/PacificBiosciences/sawfish
```
3. Directory
```
1: Working directory
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish

2: software
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/bin

3: sawfish git
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/sawfish

4: Running script
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/script

5: Input
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/Data

6: Running Results
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/IndividualCall
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/Sawfish/Run/JointCall
```


## TRGT Calling Result Phasing

### Phasing Results (Individual)
Completed
```
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/TRGT_Perfect_Match/Run/trgt
```

1. The subfolder "Hiphase" In each sample folder
<img src="https://github.com/user-attachments/assets/897da562-a5c5-4ebb-84f5-b12f2f5a0a4f" width="300">

2. Contain multipile files
   * "sorted.phased.vcf.gz" is what we want.
<img src="https://github.com/user-attachments/assets/8ccd4e68-ec75-433b-9351-7f22e8000a69" width="350">


3. The details of output

We have total 5 types of output, including
  * sorted.phased.vcf.gz and sorted.phased.vcf.gz.tbi: Phased vcf file
  * blocks.tsv: information about the the phase blocks
    
    <img src="https://github.com/user-attachments/assets/4a0ee307-6f33-4c1d-ae0e-90eebcbb2dc4" width="450">
    
  * haplotag.tsv: haplotag information for aligned reads
    
    <img src="https://github.com/user-attachments/assets/332adadc-d577-4881-b7de-a935abd466e5" width="350">
    
  * stats.csv: statistics regarding the performance of the underlying algorithms while running HiPhase
    
    <img src="https://github.com/user-attachments/assets/f9afbf16-71ce-43c5-ac31-ab04cd537bec" width="350">
    
  * summary.tsv: chromosome-level summary statistics for all phase blocks
    
    <img src="https://github.com/user-attachments/assets/98665631-18ac-4ab4-b4b3-503ea6d8ead9" width="800">
    
```
* Phase block file (--blocks-file)
This CSV/TSV file contains information about the the phase blocks that were output by HiPhase.

* Chromosome Summary File (--summary-file)
This CSV/TSV file contains chromosome-level summary statistics for all phase blocks on that chromosome. Additionally, a chromosome labeled "all" contains aggregate statistics for all phase blocks generated by HiPhase.

* Haplotag file (--haplotag-file)
This CSV/TSV file contains haplotag information for aligned reads. Note that while this contains the same information as the HP tag in the haplotagged output BAMs, generating those output BAMs is not required to generate this file.

* Algorithm Statistics File (--stats-file)
This CSV/TSV file contains statistics regarding the performance of the underlying algorithms while running HiPhase. This file is primarily for developers looking to improve HiPhase, but may be of use while identifying problematic phase blocks.

Details:
https://github.com/PacificBiosciences/HiPhase/blob/main/docs/user_guide.md
```

### Phasing Results (Joint)
Completed
```
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/TRGT_Perfect_Match/Run/joint_call_hiphase
```

### VCF Format Description
Each single event contains 4 major sections, including
  * FILTER
  * INFO
  * FORMAT

For details, please check the explanation below
```
1: FILTER
##FILTER=<ID=PASS,Description="All filters passed">

2: INFO
##INFO=<ID=TRID,Number=1,Type=String,Description="Tandem repeat ID">
##INFO=<ID=END,Number=1,Type=Integer,Description="End position of the variant described in this record">
##INFO=<ID=MOTIFS,Number=.,Type=String,Description="Motifs that the tandem repeat is composed of">
##INFO=<ID=STRUC,Number=1,Type=String,Description="Structure of the region">

3: FORMAT
##FORMAT=<ID=GT,Number=1,Type=String,Description="Genotype">
##FORMAT=<ID=AL,Number=.,Type=Integer,Description="Length of each allele">
##FORMAT=<ID=ALLR,Number=.,Type=String,Description="Length range per allele">
##FORMAT=<ID=SD,Number=.,Type=Integer,Description="Number of spanning reads supporting per allele">
##FORMAT=<ID=MC,Number=.,Type=String,Description="Motif counts per allele">
##FORMAT=<ID=MS,Number=.,Type=String,Description="Motif spans per allele">
##FORMAT=<ID=AP,Number=.,Type=Float,Description="Allele purity per allele">
##FORMAT=<ID=AM,Number=.,Type=Float,Description="Mean methylation level per allele">
##FORMAT=<ID=PS,Number=1,Type=Integer,Description="Phase set identifier">
##FORMAT=<ID=PF,Number=1,Type=String,Description="Phasing flag">
```

### Reference
1. HiPhase
   * Verson: v1.4.5
   * Github
```
https://github.com/PacificBiosciences/HiPhase/tree/main
```

2. Directory
```
1: Script
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/Pacbio/WGS_WDL_OUTPUT/TRGT/script/Microsatellite_Analysis

2: Output
/DCEG/Projects/Ped_Studies/Ewings/PacBio/CustomizeAnalysis/TRGT_Perfect_Match/Run
```

