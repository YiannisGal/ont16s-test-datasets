# ont16s-test-datasets

Small test data for the [ont16s](https://github.com/YiannisGal/ont16s) Nextflow pipeline
(CI and `-profile test`). Not for scientific use.

## zymo_msplus/zymo_msplus_mab114_sup_rep1_sub1pct.bam

| | |
|---|---|
| Reads | 546 (unaligned BAM, all `barcode04`) |
| Size | 698,203 bytes |
| sha256 | `bb292e0583b74a6d0b9d4f04cf79f29e6303dcd0416c0edbfb08f2e03a16835b` |
| Sample | ZymoBIOMICS Microbial Community DNA Standard + 4 spiked gDNAs ("MSPlus") |
| Chemistry | R10.4.1, SQK-MAB114-24, dorado 1.1.1 `dna_r10.4.1_e8.2_400bps_sup@v5.2.0` |
| Source | ONT open data, `s3://ont-open-data/zymo_16s_2025.09/basecalls/MSPlus/MAB114/sup/rep1/FBB32095_bam_pass_15aa3986_bbffde72_0.bam` (sha256 `470db4e5…44b2d1b1`) |

Made with (samtools 1.24, `quay.io/biocontainers/samtools:1.24--h9dcdb79_1`):

```bash
samtools view -b -s 42.01 -o zymo_msplus_mab114_sup_rep1_sub1pct.bam FBB32095_bam_pass_15aa3986_bbffde72_0.bam
```

Check: Emu 3.6.2 with its default DB detects all 12 expected bacteria in ~52 s on 4 threads.

## Licence

The data are a subset of the Oxford Nanopore Technologies Benchmark Datasets, released
under **CC BY-NC 4.0** (https://creativecommons.org/licenses/by-nc/4.0/).
Credit: Oxford Nanopore Technologies, ONT Open Data (https://registry.opendata.aws/ont-open-data/).
Use is limited to non-commercial purposes.

## emu_db_zymo_genera/ (Emu-format test database)

| File | Size (bytes) | sha256 |
|---|---|---|
| `species_taxid.fasta` | 19,532,946 | `9c5f7c8186d5b19da4a93b18e0561bf1aed3fd523d35458dfc57cf9b91e11ab8` |
| `taxonomy.tsv` | 120,438 | `245ddbc20f9487c8a4b2a4b33ed48f87ae040ad03c39f3331fb6ef2083cdb75c` |

Subset of the **Emu default database** (rrnDB v5.6 + NCBI 16S RefSeq, 2020-09-17;
OSF project `56uf7`, `emu-prebuilt/emu.tar`, sha256 `75e93064…5e4f4132`): all 955 species
(10,170 sequences) of the 15 genera in the Zymo MSPlus mock (Pseudomonas, Escherichia,
Salmonella, Limosilactobacillus, Lactobacillus, Enterococcus, Staphylococcus, Listeria,
Bacillus, Bifidobacterium, Borrelia, Borreliella, Chlamydia, Gardnerella, Shigella).
Keeping whole genera keeps realistic near-neighbour confusion. **For tests only** — not
a general-purpose database.

Made with:

```bash
G='^(Pseudomonas|Escherichia|Salmonella|Limosilactobacillus|Lactobacillus|Enterococcus|Staphylococcus|Listeria|Bacillus|Bifidobacterium|Borrelia|Borreliella|Chlamydia|Gardnerella|Shigella)$'
awk -F'\t' -v g="$G" 'NR==1 || $3 ~ g' emu/taxonomy.tsv > taxonomy.tsv
awk -F'\t' 'NR>1{print $1}' taxonomy.tsv > ids.txt
awk 'NR==FNR{k[$1];next} /^>/{split(substr($0,2),a,":"); keep=(a[1] in k)} keep' ids.txt emu/species_taxid.fasta > species_taxid.fasta
```

Check: Emu 3.6.2 on the 546-read BAM subset detects 12/12 expected bacteria (~50 s, 4 threads).

Please cite when using it: Stoddard et al. 2015 (rrnDB), O'Leary et al. 2016 (RefSeq),
Schoch et al. 2020 (NCBI Taxonomy), Curry et al. 2022 (Emu).
