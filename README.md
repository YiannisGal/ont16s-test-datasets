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
