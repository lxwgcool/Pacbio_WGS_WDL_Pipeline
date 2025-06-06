# TRGT Algorithm Interpretation
## Questions 1: if you are able to provide me with details from TRGT to clarify what the algorithm calls a microsatellite?
There are a total of three steps in how TRGT calls microsatellites
### Step 1: TRGT locates reads that span a given repeat region and clusters them:
1. To cluster the reads, TRGT first calculates the edit distances between all pairs of reads and then performs agglomerative clustering using Ward linkage.
2. It filters out any cluster containing fewer than 10% of the total number of spanning reads.
3. Two cases are considered:
   * Diploid repeats: TRGT assigns the two largest clusters to each allele.
   * Haploid repeats: The largest cluster is assigned to the allele.

### Step 2: Reconstructing the consensus sequence of each repeat allele:
1. A read of the median length is selected from the corresponding cluster and used as the alignment backbone.
2. All reads in the cluster are aligned against this backbone sequence.
3. The consensus sequence is determined by scanning the backbone and incorporating bases through a majority vote based on the alignment operations.

### Step 3: Annotating occurrences of individual repeat motifs within each consensus allele (final step of calling repeats):
TRGT considers two cases separately:
1. Case 1: Simple tandem repeats (TRs)
   * Definition
      * Repetitions of one or multiple fixed-sized motifs.
      * Regions whose population structure can be described as a series of TRs, possibly separated by interrupting sequences.
   * **Algorithm used to find repeats: Acyclic graph**
      * TRGT finds the longest path in the graph.
2. Case 2: Complex repeats
   * Definition
      * The opposite of case 1 (i.e., not describable by case 1 (simple tandem repeats)).
   * **Algorithm used to find repeats: Hidden Markov Model (HMM)**
      * TRGT builds HMMs that capture sequences made up of repeated occurrences of specific motifs.

### Conclusion
If we exclude the data preprocessing step, there are 2 main algorithms used for repeat detection in TRGT
* The acyclic graph (for simple TRs)
* HMM (for complex repeats).

## Question 2: I assume this comes from the library information we feed the algorithm
Yes, the library information we provide to the algorithm is the most important dependency of TRGT, as it is used to locate repeat loci and related repeats.

## Question 3: If there is a minimum size threshold used to define the microsatellite regions in the library.
Yes, we do have some thresholds that were used to define the microsatellite regions in the library. Please see the details below.
### 1. The original library used to generate ggaa_repeats.bed is variation_clusters_and_isolated_TRs_v1.hg38.TRGT.bed.gz:
1. This file was provided by the Broad Institute.
```
GitHub repository:
https://github.com/broadinstitute/tandem-repeat-catalog/
```
2. It contains a genome-wide tandem repeat (TR) catalog with approximately 4.9 million loci.
   * The catalog stratifies TRs into two groups:
   * Isolated TRs, suitable for traditional repeat copy number analysis using short-read or long-read data.
   * TRs embedded within broader polymorphic regions (i.e., variation clusters), which are better studied through sequence-level analysis.

### 2. Regarding the genome-wide TR catalog
1. Includes all perfect repeats in hg38 that span ≥ 9 bp.
2. Consists of at least 3 repeat units of any motif with a size between 1 and 1000 bp.
3. These repeats were identified using ColabRepeatFinder:
   * ColabRepeatFinder is a tool developed and maintained by the Broad Institute.
```
GitHub repository:
https://github.com/broadinstitute/colab-repeat-finder?tab=readme-ov-file
```

### 3. About "generate ggaa_repeats.bed" 
1. Egor helped us write a script to extract the GGAA-repeat-related regions (i.e., rebuild the relevant motifs) from variation_clusters_and_isolated_TRs_v1.hg38.TRGT.bed.gz to generate ggaa_repeats.bed
   * No new thresholds were introduced.
   * Everything is fully based on the original BED file provided by the Broad Institute.

### Conclusion
The thresholds (ColabRepeatFinder) used to define the microsatellite regions in the library are as follows:
```
--min-span: 9
--min-repeats: 3
--min-motif-size: 1
--max-motif-size: 1000
```
