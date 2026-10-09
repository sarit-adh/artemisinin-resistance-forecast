# Pf8

MalariaGEN **Pf8** release: 33,325 samples worldwide (collection years
1966–2022) — larger and more recent than Pf7, and the release that covers the
East-African artemisinin emergences. Shared conventions (genotype notation
`gene_codon[REF]`, mixed-call/`-`/`*` rules, copy-number codes, gene list) are
in [`../README.md`](../README.md).

## Files

| File | Rows | Keyed on | What it adds |
|---|---:|---|---|
| `Pf8_samples.txt` | 33,325 | `Sample` | sample metadata |
| `Pf8_drug_resistance_marker_genotypes.tsv` | 24,409 | `Sample` | point-mutation genotypes |
| `Pf8_inferred_resistance_status_classification.tsv` | 24,409 | `sample` | per-drug resistance verdicts |
| `Pf8_fws.tsv` | 24,409 | `Sample` | within-sample diversity |
| `Pf8_cnv_calls.tsv` | 24,409 | `Sample` | gene amplification/deletion (CNV) calls |
| `Pf8_tandem_duplication_breakpoints.tsv` | 65 | — | reference catalogue of known amplification breakpoints |
| `Pf8_resistance_classification-1.pdf` | — | — | documents the classification rules |

`samples` lists all 33,325; the four per-sample data files cover only the
**24,409 that passed QC** (`QC pass == True`). Join them on `Sample` (note the
classification file's key is lowercase `sample`).

## `Pf8_samples.txt` — sample metadata

Same schema as Pf7 except the last column. Finest location is admin-1.

| Field | Meaning |
|---|---|
| `Sample` | Unique sample ID (primary key). |
| `Study` | Source study code (99 studies). |
| `Country` | Collection country (34). |
| `Admin level 1` | Province/region of collection. |
| `Country latitude` / `Country longitude` | Country centroid. |
| `Admin level 1 latitude` / `Admin level 1 longitude` | Admin-1 centroid. |
| `Year` | Collection year (1966–2022). |
| `ENA` | European Nucleotide Archive accession for raw reads. |
| `All samples same case` | ID grouping samples from one clinical case. |
| `Population` | Region code: `AF-W/C/E/NE` Africa; `AS-S-*`/`AS-SE-*` Asia; `OC-NG` Oceania; `SA` S. America. |
| `% callable` | Fraction of genome confidently genotyped. |
| `QC pass` | `True`/`False`; only `True` rows have genotype/classification/fws/CNV. |
| `Exclusion reason` | `Analysis_set` if retained; QC reason otherwise. |
| `Sample type` | Library prep (`gDNA`, `sWGA`, `MDA`). |
| `Sample was in Pf7` | Whether the sample also appeared in Pf7 (20,864 are shared). |

## `Pf8_drug_resistance_marker_genotypes.tsv` — point-mutation genotypes

Column-name and value notation: see parent README.

| Field | Meaning |
|---|---|
| `crt_72[C]` `crt_74[M]` `crt_75[N]` `crt_76[K]` | CRT codons 72–76 (chloroquine; 76 is the key K76T call). |
| `crt_72-76[CVMNK]` | CRT 72–76 haplotype (`CVMNK` wild-type, `CVIET` resistant). |
| `crt_93[T]` `crt_97[H]` `crt_218[I]` `crt_220[A]` `crt_271[Q]` `crt_326[N]` `crt_333[T]` `crt_353[G]` `crt_356[I]` `crt_371[R]` | Additional CRT codons (incl. piperaquine-associated changes). |
| `dhfr_16[N]` `dhfr_51[N]` `dhfr_59[C]` `dhfr_108[S]` `dhfr_164[I]` `dhfr_306[S]` | DHFR codons — pyrimethamine resistance. |
| `dhps_436[S]` `dhps_437[G]` `dhps_540[K]` `dhps_581[A]` `dhps_613[A]` | DHPS codons — sulfadoxine resistance. |
| `exo_415[E]` | Exonuclease E415G — piperaquine resistance background. |
| `mdr1_86[N]` `mdr1_184[Y]` `mdr1_1034[S]` `mdr1_1042[N]` `mdr1_1226[F]` `mdr1_1246[D]` | *Pfmdr1* codons — multidrug/mefloquine, modulates ACT partner-drug response. |
| `arps10_127-128[VD]` | *arps10* V127M/D128 — resistance genetic background. |
| `fd_193[D]` | Ferredoxin D193Y — resistance background. |
| `mdr2_484[T]` | *Pfmdr2* T484I — resistance background. |
| `kelch13_349-726_ns_changes` | **Artemisinin marker:** non-synonymous changes in the *Pfkelch13* propeller (349–726). Blank = none; else mutation(s) e.g. `C580Y`, `R561H`. |
| `mdr1_dup_call` | *Pfmdr1* copy number (`1`/`0`/`-1`) — mefloquine. |
| `pm2_dup_call` | Plasmepsin-2 copy number (`1`/`0`/`-1`) — piperaquine. |

## `Pf8_inferred_resistance_status_classification.tsv` — per-drug verdicts

Rule-based calls derived from genotype (rules in
`Pf8_resistance_classification-1.pdf`). All drug fields take
`Sensitive`/`Resistant`/`Undetermined`. (Unlike Pf7, there are no *hrp*
columns here — HRP2/HRP3 deletions are in the CNV file.)

| Field | Meaning |
|---|---|
| `sample` | Sample ID (lowercase key). |
| `Chloroquine` `Pyrimethamine` `Sulfadoxine` `Mefloquine` `Artemisinin` `Piperaquine` | Single-drug resistance status. |
| `SP (uncomplicated)` / `SP (IPTp)` | Sulfadoxine–pyrimethamine: treatment vs. preventive-use context. |
| `AS-MQ` | Artesunate–mefloquine (combination). |
| `DHA-PPQ` | Dihydroartemisinin–piperaquine (combination). |

## `Pf8_fws.tsv` — within-sample diversity

| Field | Meaning |
|---|---|
| `Sample` | Sample ID. |
| `Fws` | Within-sample diversity, ~0–1. Near 1 = single clone; lower = multiple co-infecting strains (high multiplicity of infection). |

## `Pf8_cnv_calls.tsv` — copy-number (amplification / deletion) calls

Detailed CNV evidence for four amplified genes (**CRT, GCH1, MDR1, PM2_PM3**)
and two deletable antigens (**HRP2, HRP3**). Each gene has several evidence
columns plus one `final_*_call` summary; the final call is normally the field
to use (`1` = amplified/deleted, `0` = normal, `-1` = uncallable).

| Field pattern | Meaning |
|---|---|
| `Sample` | Sample ID. |
| `<GENE>_uncurated_coverage_only` | Raw copy-number call from read-depth alone. |
| `<GENE>_curated_coverage_only` | Coverage call after manual curation. |
| `<GENE>_faceaway_only` | Call from "face-away" read-pair orientation evidence (duplication signature). |
| `<GENE>_breakpoint` | Named breakpoint detected (see breakpoints catalogue); `-` if none. |
| `<GENE>_final_amplification_call` | **Final amplification verdict** for CRT/GCH1/MDR1/PM2_PM3 (`1`/`0`/`-1`). |
| `HRP2_deletion_type` / `HRP3_deletion_type` | Nature of the deletion when present (e.g. `Telomere healing`); `-` if none. |
| `HRP2_final_deletion_call` / `HRP3_final_deletion_call` | **Final deletion verdict** for the rapid-test antigen genes (`1` = deleted, `0` = present, `-1` = uncallable). |

Gene set: **CRT** (chloroquine/piperaquine), **GCH1** (antifolate background),
**MDR1** (mefloquine/ACT partner), **PM2_PM3** (plasmepsin-2/3, piperaquine),
**HRP2/HRP3** (deletion → false-negative rapid diagnostic tests).

## `Pf8_tandem_duplication_breakpoints.tsv` — breakpoint reference catalogue

Not per-sample: one row per **known amplification breakpoint** (65 total),
referenced by name from the CNV file's `<GENE>_breakpoint` columns.

| Field | Meaning |
|---|---|
| `chrom` | Chromosome, 3D7 reference (e.g. `Pf3D7_05_v3`). |
| `first_breakpoint_start` / `first_breakpoint_end` | Coordinate window of the first (proximal) breakpoint. |
| `second_breakpoint_start` / `second_breakpoint_end` | Coordinate window of the second (distal) breakpoint. |
| `start_gene_id` / `end_gene_id` | Genes flanking the duplicated segment (PlasmoDB IDs). |
| `breakpoint_name` | Human-readable name (e.g. `42kb around MDR1`). |
| `breakpoint_id` | Stable ID referenced by the CNV file (e.g. `PfMDR1_dup_1`). |
| `target_gene` | Resistance gene amplified: `MDR1`, `GCH1`, `CRT`, or `plasmepsin`. |
| `use` | `1` = used in calling, `0` = not used. |
| `note` | Free-text annotation (often empty). |
