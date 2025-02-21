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
Still running
```
/DCEG/Projects/Exome/SequencingData/DAATeam/Xin/ad_hoc/SawFish/Run/JointCall/output/call
```

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
### Reference
1. HiPhase
   * Verson: 
