# Tools and running options used by Pacbio WGS WDL Pipeline
The primary 3rd-party tools and parameters that are used for generating the deliverable results. 
The list below shows all sections contained by current online documentation. 
1. [Small_Variant](#small_variant)
2. [Phasing](#phasing)
3. [Structure_Variation](#structure_variation)
4. [Haplotype_Phasing_Info](#haplotype_phasing_info)
5. [Haplotagged_BAM](#haplotagged_bam)
6. [Copy_Number_Variation](#copy_number_variation)
7. [Tandem_Repeats](#tandem_repeats)
8. [Medically_Relevant_Genes](#medically_relevant_genes)
9. [Methylation](#methylation)
10. [Quality_Control](#quality_control)
11. [(Cohort) Small_variant](#cohort_small_variant)
12. [(Cohort)_Structure_Variation](#cohort_structure_variation)
13. [(Cohort)_Haplotype_Phasing_Info](#cohort_haplotype_phasing_info)
14. [(Tertiary)_Small_variant](#tertiary_small_variant)
15. [(Tertiary)_Structure_Variation](#tertiary_structure_variation)

# Sample Analysis
## Small_Variant
1. Deepvairant
   * $${\color{blue}Version\ Number: }$$ v1.5.0
   * Running command line and parameters
```
(1) make_examples
seq ~{task_start_index} ~{task_end_index} \
	| parallel \
		--jobs ~{tasks_per_shard} \
		--halt 2 \
		/opt/deepvariant/bin/make_examples \
			--norealign_reads \
			--vsc_min_fraction_indels 0.12 \
			--pileup_image_width 199 \
			--track_ref_reads \
			--phase_reads \
			--partition_size=25000 \
			--max_reads_per_partition=600 \
			--alt_aligned_pileup=diff_channels \
			--add_hp_channel \
			--sort_by_haplotypes \
			--parse_sam_aux_fields \
			--min_mapping_quality=1 \
			--mode calling \
			--ref ~{reference} \
			--reads ~{sep="," aligned_bams} \
			--examples example_tfrecords/~{sample_id}.examples.tfrecord@~{total_deepvariant_tasks}.gz \
			--gvcf nonvariant_site_tfrecords/~{sample_id}.gvcf.tfrecord@~{total_deepvariant_tasks}.gz \
			--task {}

(2) call_variants
/opt/deepvariant/bin/call_variants \
			--outfile ~{sample_id}.~{reference_name}.call_variants_output.tfrecord.gz \
			--examples "example_tfrecords/~{sample_id}.examples.tfrecord@~{total_deepvariant_tasks}.gz" \
			--checkpoint "${deepvariant_model_path}"

(3) postprocess_variants
/opt/deepvariant/bin/postprocess_variants \
			--vcf_stats_report=false \
			--ref ~{reference} \
			--infile ~{tfrecord} \
			--outfile ~{sample_id}.~{reference_name}.deepvariant.vcf.gz \
			--nonvariant_site_tfrecord_path "nonvariant_site_tfrecords/~{sample_id}.gvcf.tfrecord@~{total_deepvariant_tasks}.gz" \
			--gvcf_outfile ~{sample_id}.~{reference_name}.deepvariant.g.vcf.gz   
```

2. bcftools_roh
   * $${\color{blue}Version\ Number: }$$ bcftools 1.14 using htslib 1.14
   * Running command line and parameters
```
bcftools roh \
		--threads ~{threads - 1} \
		--AF-dflt 0.4 \
		~{vcf} \
	> ~{vcf_basename}.bcftools_roh.out

```

## Phasing
1. HiPhase
   * $${\color{blue}Version\ Number: }$$ v1.0.0-f1bc7a8
   * Running command line and parameters
```
hiphase --threads ~{threads} \
  ~{sep=" " prefix("--sample-name ", sample_ids)} \
  ~{sep=" " prefix("--vcf ", vcfs)} \
  ~{sep=" " prefix("--output-vcf ", phased_vcf_names)} \
  ~{sep=" " prefix("--bam ", bams)} \
  ~{true="--output-bam " false="" length(haplotagged_bam_names) > 0} ~{sep=" --output-bam " haplotagged_bam_names} \
  --reference ~{reference} \
  --summary-file ~{id}.~{refname}.hiphase.stats.tsv \
  --blocks-file ~{id}.~{refname}.hiphase.blocks.tsv \
  ~{haplotags_param} \
  --global-realignment-cputime 300
```

## Structure_Variation
1. pbsv
   * $${\color{blue}Version\ Number: }$$ v2.9.0 (commit v2.9.0-2-gce1559a)
   * Running command line and parameters
```
(1) pbsv_call
pbsv call \
	--hifi \
	--min-sv-length 20 \
	--log-level INFO \
	--num-threads ~{threads} \
	~{reference} \
	svsigs.fofn \
	"~{output_basename}.vcf"

(2) pbsv discover
pbsv discover \
	--log-level INFO \
	--hifi \
	--tandem-repeats ~{reference_tandem_repeat_bed} \
	~{aligned_bam} \
	~{prefix}.svsig.gz
```

## Haplotype_Phasing_Info
All of results come from the hiphase in the step of "Phasing".

## Haplotagged_BAM
All of results come from the hiphase in the step of "Phasing".

## Copy_Number_Variation
1. hificnv
   * $${\color{blue}Version\ Number: }$$ v0.1.7-70e9988
   * Running command line and parameters
```
hificnv \
	--threads ~{threads} \
	--bam ~{bam} \
	--ref ~{reference} \
	--maf ~{phased_vcf} \
	--exclude ~{exclude_bed} \
	--expected-cn ~{expected_bed} \
	--output-prefix ~{output_prefix}
```

## Tandem_Repeats
1. trgt
   * $${\color{blue}Version\ Number: }$$ v0.5.0-a70fa16
   * Running command line and parameters
```
trgt \
	--threads ~{threads} \
	--karyotype ~{karyotype} \
	--genome ~{reference} \
	--repeats ~{tandem_repeat_bed} \
	--reads ~{bam} \
	--output-prefix ~{bam_basename}.trgt
```

## Medically_Relevant_Genes
1. paraphase
   * $${\color{blue}Version\ Number: }$$ v2.2.3
   * Running command line and parameters:
```
paraphase \
	--threads ~{threads} \
	--bam ~{bam} \
	--reference ~{reference} \
	--out ~{out_directory}
```

## Methylation
1. pb-CpG-tools
   * $${\color{blue}Version\ Number: }$$ v2.3.2
   * Running command line and parameters:
```
aligned_bam_to_cpg_scores \
	--threads ~{threads} \
	--bam ~{bam} \
	--ref ~{reference} \
	--output-prefix ~{output_prefix} \
	--min-mapq 1 \
	--min-coverage 10 \
	--model "$PILEUP_MODEL_DIR"/pileup_calling_model.v1.tflite
```

## Quality_Control
1. datamash
   * $${\color{blue}Version\ Number: }$$ v1.3
   * Running command line and parameters:
```
(1) read_length_and_quality
extract_read_length_and_qual.py \
	~{bam} \
> ~{sample_id}.~{movie}.read_length_and_quality.tsv

(2) read_length_summary.tsv
awk '{{ b=int($2/1000); b=(b>39?39:b); print 1000*b "\t" $2; }}' \
	~{sample_id}.~{movie}.read_length_and_quality.tsv \
	| sort -k1,1g \
	| datamash -g 1 count 1 sum 2 \
	| awk 'BEGIN {{ for(i=0;i<=39;i++) {{ print 1000*i"\t0\t0"; }} }} {{ print; }}' \
	| sort -k1,1g \
	| datamash -g 1 sum 2 sum 3 \
> ~{sample_id}.~{movie}.read_length_summary.tsv

(3) read_quality_summary.tsv
awk '{{ print ($3>50?50:$3) "\t" $2; }}' \
		~{sample_id}.~{movie}.read_length_and_quality.tsv \
	| sort -k1,1g \
	| datamash -g 1 count 1 sum 2 \
	| awk 'BEGIN {{ for(i=0;i<=60;i++) {{ print i"\t0\t0"; }} }} {{ print; }}' \
	| sort -k1,1g \
	| datamash -g 1 sum 2 sum 3 \
> ~{sample_id}.~{movie}.read_quality_summary.tsv
```

# Cohort Analysis
## Cohort_Small_variant
1. glnexus
   * $${\color{blue}Version\ Number: }$$ vv1.4.3-0-
   * Running command line and parameters:
```
# glneux_cli has no version option
glnexus_cli --help 2>&1 | grep -Eo 'glnexus_cli release v[0-9a-f.-]+'

glnexus_cli \
	--threads ~{threads} \
	--mem-gbytes ~{mem_gb} \
	--dir ~{cohort_id}.~{reference_name}.GLnexus.DB \
	--config DeepVariant_unfiltered \
	~{"--bed " + regions_bed} \
	~{sep=' ' gvcfs} \
> ~{cohort_id}.~{reference_name}.deepvariant.glnexus.bcf

bcftools view \
	--threads ~{threads} \
	--output-type z \
	--output-file ~{cohort_id}.~{reference_name}.deepvariant.glnexus.vcf.gz \
	~{cohort_id}.~{reference_name}.deepvariant.glnexus.bcf
```

## Cohort_Structure_Variation
1. pbsv (same as the structure variation in smaple_analysis)
   * $${\color{blue}Version\ Number: }$$ v2.9.0 (commit v2.9.0-2-gce1559a)
2. concat_vcf
   * $${\color{blue}Version\ Number: }$$ v1.14 using htslib 1.14
Running command line and parameters
```
(1) pbsv_call
pbsv call \
	--hifi \
	--min-sv-length 20 \
	--log-level INFO \
	--num-threads ~{threads} \
	~{reference} \
	svsigs.fofn \
	"~{output_basename}.vcf"

(2) bcftools concat
bcftools concat \
	--allow-overlaps \
	--threads ~{threads - 1} \
	--output-type z \
	--output ~{output_vcf_name} \
	--file-list vcf.list

```

## Cohort_Haplotype_Phasing_Info
1. hiphase (same as the structure variation in smaple_analysis)
   * $${\color{blue}Version\ Number: }$$ v1.0.0-f1bc7a8
Running command line and parameters
```
hiphase --threads ~{threads} \
	~{sep=" " prefix("--sample-name ", sample_ids)} \
	~{sep=" " prefix("--vcf ", vcfs)} \
	~{sep=" " prefix("--output-vcf ", phased_vcf_names)} \
	~{sep=" " prefix("--bam ", bams)} \
	~{true="--output-bam " false="" length(haplotagged_bam_names) > 0} ~{sep=" --output-bam " haplotagged_bam_names} \
	--reference ~{reference} \
	--summary-file ~{id}.~{refname}.hiphase.stats.tsv \
	--blocks-file ~{id}.~{refname}.hiphase.blocks.tsv \
	~{haplotags_param} \
	--global-realignment-cputime 300
```

# Tertiary Analysis
## Tertiary_Small_variant
1. slivar
   * $${\color{blue}Version\ Number: }$$ v0.2.2 186b862063ce50ee1d282bc610196630c4ecac61
   * Parameters
```
max_gnomad_af = 0.03
max_hprc_af = 0.03
max_gnomad_nhomalt = 4
max_hprc_nhomalt = 4
max_gnomad_ac = 4
max_hprc_ac = 4
min_gq = 5
```
   
   * Running command line and parameters
     
```
(1) pslivar
pslivar \
	--processes ~{threads} \
	--fasta ~{reference} \
	--pass-only \
	--js ~{slivar_js} \
	--info '~{sep=" && " info_expr}' \
	--family-expr '~{sep=" && " family_recessive_expr}' \
	--family-expr '~{sep=" && " family_x_recessive_expr}' \
	--family-expr '~{sep=" && " family_dominant_expr}' \
	--sample-expr '~{sep=" && " sample_expr}' \
	--gnotate ~{gnomad_af} \
	--gnotate ~{hprc_af} \
	--vcf ~{vcf_basename}.norm.bcf \
	--ped ~{pedigree} \
	| bcftools csq \
	--local-csq \
	--samples - \
	--ncsq 40 \
	--gff-annot ~{gff} \
	--fasta-ref ~{reference} \
	- \
	--output-type z \
	--output ~{vcf_basename}.norm.slivar.vcf.gz

(2) slivar (norm.slivar.compound_hets.vcf.gz)
slivar \
	compound-hets \
	--skip ~{sep=',' skip_list} \
	--vcf ~{vcf_basename}.norm.slivar.vcf.gz \
	--sample-field comphet_side \
	--ped ~{pedigree} \
	--allow-non-trios \
| add_comphet_phase.py \
| bcftools view \
	--output-type z \
	--output ~{vcf_basename}.norm.slivar.compound_hets.vcf.gz

(3) slivar (norm.slivar.tsv)
slivar tsv \
	--info-field ~{sep=' --info-field ' info_fields} \
	--sample-field dominant \
	--sample-field recessive \
	--sample-field x_recessive \
	--csq-field BCSQ \
	--gene-description ~{lof_lookup} \
	--gene-description ~{clinvar_lookup} \
	--gene-description ~{phrank_lookup} \
	--ped ~{pedigree} \
	--out /dev/stdout \
	~{vcf_basename}.norm.slivar.vcf.gz \
| sed '1 s/gene_description_1/lof/;s/gene_description_2/clinvar/;s/gene_description_3/phrank/;' \
> ~{vcf_basename}.norm.slivar.tsv

(4) slivar (norm.slivar.compound_hets.tsv)
slivar tsv \
	--info-field ~{sep=' --info-field ' info_fields} \
	--sample-field slivar_comphet \
	--info-field slivar_comphet \
	--csq-field BCSQ \
	--gene-description ~{lof_lookup} \
	--gene-description ~{clinvar_lookup} \
	--gene-description ~{phrank_lookup} \
	--ped ~{pedigree} \
	--out /dev/stdout \
	~{vcf_basename}.norm.slivar.compound_hets.vcf.gz \
| sed '1 s/gene_description_1/lof/;s/gene_description_2/clinvar/;s/gene_description_3/phrank/;' \
> ~{vcf_basename}.norm.slivar.compound_hets.tsv
```

## Tertiary_Structure_Variation
1. svpack
   * $${\color{blue}Version\ Number: }$$ git head (36180ae6f271b93964250bd05f7e61842ffb6cb3)
2. slivar
   * $${\color{blue}Version\ Number: }$$ v0.2.2 186b862063ce50ee1d282bc610196630c4ecac61

Running command line and parameters

```
(1) svpack
svpack \
	filter \
	--pass-only \
	--min-svlen 50 \
	~{sv_vcf} \
~{sep=' ' prefix('| svpack match -v - ', population_vcfs)} \
| svpack \
	consequence \
	- \
	~{gff} \
| svpack \
	tagzygosity \
	- \
> ~{sv_vcf_basename}.svpack.vcf

(2) slivar tsv
slivar tsv \
	--info-field ~{sep=' --info-field ' info_fields} \
	--sample-field hetalt \
	--sample-field homalt \
	--csq-field BCSQ \
	--gene-description ~{lof_lookup} \
	--gene-description ~{clinvar_lookup} \
	--gene-description ~{phrank_lookup} \
	--ped ~{pedigree} \
	--out /dev/stdout \
	~{filtered_vcf} \
| sed '1 s/gene_description_1/lof/;s/gene_description_2/clinvar/;s/gene_description_3/phrank/;' \
> ~{filtered_vcf_basename}.tsv
```
