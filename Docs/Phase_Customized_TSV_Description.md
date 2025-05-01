# The description of file "phased.customize.tsv"
## Background
### Motivation
The calling results and related infomation are spreaded in multiple files. We extract some types of important information from multiple files and summarize them into one file.

### The original outputs from HiPhase that are used to generate "phased.customize.tsv"
1. merged.blocks.tsv

<img src="https://github.com/user-attachments/assets/79bd2c8b-72a5-4cdf-a98a-7a9474116db6" width="350">

2. merged.phased.vcf.gz
   
![image](https://github.com/user-attachments/assets/d17bd9ec-4a5a-4d82-b520-6a3c61424e97)


3. merged.haplotag.tsv

<img src="https://github.com/user-attachments/assets/bd5bb94e-69f0-421a-a7c8-2c87d0accd1a" width="350">


## Introduction of "phased.customize.tsv"
### Total number of caption fields
16

### An example of "phased.customize.tsv"

![image](https://github.com/user-attachments/assets/a4f70ce6-d887-4f51-ae5a-555f8f82a888)


### The descrioption of each caption field
| Caption Field | Description | Original file | Related TAG | 
| :--  | :--      | :--   | :-- |
| TRID | Tandem repeat ID | merged.phased.vcf.gz | TRID |
| Region | The event's region from start to end | merged.phased.vcf.gz | POS, END |
| Sample | Sample name | - | - |
| REF | Reference sequence | merged.phased.vcf.gz | REF |
| Allele | Reference sequence or Alternative sequence| merged.phased.vcf.gz | REF, ALT |
| Motif | Motifs that the tandem repeat is composed of | merged.phased.vcf.gz | MOTIFS |
| Purity | Allele purity per allele | merged.phased.vcf.gz | AP |
| Motif_Count | Motif counts per allele | merged.phased.vcf.gz | MC |
| Motif_Span | Motif spans per allele | merged.phased.vcf.gz | MS |
| Allele_Length | Length of each allele | merged.phased.vcf.gz | AL |
| Length_Range_Per_Allele | Length range per allele | merged.phased.vcf.gz | ALLR |
| Num_Spanning_Reads_Supporting_Per_Allele | Number of spanning reads supporting per allele | merged.phased.vcf.gz | SD |
| Phase_set_ID | Phase set identifier | merged.phased.vcf.gz | PS |
| Hap1/Hap2 | Number of reads from Hap1 VS Hap2 from same phasing block | merged.haplotag.tsv | phase_block_id, read_name, haplotag |
| Haplotag | Based on phased genotype ("Hap1" \| "Hap2") | merged.phased.vcf.gz | GT |
| Num_Var_In_Phasing_Block | number of variant in current phasing block | merged.blocks.tsv | phase_block_id, num_variants |

                                                              



