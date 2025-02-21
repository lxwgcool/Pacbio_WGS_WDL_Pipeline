## Sawfish Structure Variation Detection
### Dependence           
1. Haplotagged BAM Files
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/Data/EwingSampleList.txt
```
### Individual Discover
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/IndividualCall/output/discover
```
### Individual Call (Results)
"genotyped.sv.vcf.gz" is our target file
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/IndividualCall/output/call
```
<img src="https://github.com/user-attachments/assets/cd7c5b7c-d235-4e8a-99ce-73ae413008ae" width="350">


### Joint Call (Results)
1. Still running
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/JointCall/output/call
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
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/script

5: Input
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/Data

6: Running Results
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/IndividualCall
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/JointCall
```


## TRGT Calling Result Phasing

### Phasing Results (Individual)
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/Pacbio/WGS_WDL_OUTPUT/TRGT/output/trgt
```

1. The subfolder "Hiphase" In each sample folder
<img src="https://github.com/user-attachments/assets/897da562-a5c5-4ebb-84f5-b12f2f5a0a4f" width="300">

2. Contain multipile files
   * "sorted.phased.vcf.gz" is what we want.
<img src="https://github.com/user-attachments/assets/524da7a1-5d4f-4705-b07a-897ee8c9d1a6" width="350">


3. The details of output
```
https://github.com/PacificBiosciences/HiPhase/blob/main/docs/user_guide.md
```

### Phasing Results (Joint)
1. It still running
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/Pacbio/WGS_WDL_OUTPUT/TRGT/output/Hiphase
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
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/Pacbio/WGS_WDL_OUTPUT/TRGT/output
```

