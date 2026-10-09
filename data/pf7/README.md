# Pf7

MalariaGEN **Pf7** release: 20,864 samples worldwide (2023). Shared conventions
(genotype notation, gene list, caveats) are in [`../README.md`](../README.md).
Pf8 supersedes this release — see [`../pf8/`](../pf8/).

## Files

| File | Rows | Keyed on |
|---|---:|---|
| `Pf7_samples.txt` | 20,864 | `Sample` |
| `Pf7_drug_resistance_marker_genotypes.txt` | 16,203 | `Sample` (row index) |
| `Pf7_inferred_resistance_status_classification.txt` | 16,203 | `Sample` |
| `Pf7_fws.txt` | 16,203 | `Sample` |
| `Pf7_resistance_classification.pdf` | — | documents the classification rules |

`samples` lists all 20,864 samples; the other three cover only the **16,203
that passed QC** (`QC pass == True`). Join them on `Sample`.

## `Pf7_samples.txt` — sample metadata

| Field | Meaning |
|---|---|
| `Sample` | Unique sample ID (primary key). |
| `Study` | Source study code (82 studies). |
| `Country` | Collection country (33). |
| `Admin level 1` | Province/region of collection. |
| `Country latitude` / `Country longitude` | Country centroid. |
| `Admin level 1 latitude` / `Admin level 1 longitude` | Admin-1 centroid (finest location given). |
| `Year` | Collection year (1984–2018). |
| `ENA` | European Nucleotide Archive accession for raw reads. |
| `All samples same case` | ID grouping samples from one clinical case. |
| `Population` | Region code: `AF-W/C/E/NE` Africa; `AS-S-*`/`AS-SE-*` Asia; `OC-NG` Oceania; `SA` S. America. |
| `% callable` | Fraction of genome confidently genotyped. |
| `QC pass` | `True`/`False`; only `True` rows have genotype & classification. |
| `Exclusion reason` | `Analysis_set` if retained; QC reason otherwise. |
| `Sample type` | Library prep: `sWGA`, `gDNA`, `MDA`. |
| `Sample was in Pf6` | Also present in the earlier Pf6 release. |

## `Pf7_drug_resistance_marker_genotypes.txt` — genotype calls

Row index = `Sample`. Column-name and value notation: see parent README.

| Field | Meaning |
|---|---|
| `crt_76[K]` | Chloroquine resistance (CRT K76T). |
| `crt_72-76[CVMNK]` | CRT 72–76 haplotype (`CVMNK` wild-type, `CVIET` resistant). |
| `dhfr_51[N]` `dhfr_59[C]` `dhfr_108[S]` `dhfr_164[I]` | Pyrimethamine resistance codons (DHFR). |
| `dhps_437[G]` `dhps_540[K]` `dhps_581[A]` `dhps_613[A]` | Sulfadoxine resistance codons (DHPS). |
| `kelch13_349-726_ns_changes` | **Artemisinin marker:** non-synonymous changes in the *Pfkelch13* propeller (349–726). Blank = none; else mutation(s) e.g. `C580Y`, `R561H`. |
| `mdr1_dup_call` | *Pfmdr1* copy number (`1`/`0`/`-1`) — mefloquine. |
| `mdr1_breakpoint` | Named *mdr1* amplification breakpoint (when amplified). |
| `pm2_dup_call` | Plasmepsin-2 copy number (`1`/`0`/`-1`) — piperaquine. |
| `pm2_breakpoint` | Named plasmepsin-2 amplification breakpoint. |

## `Pf7_inferred_resistance_status_classification.txt` — per-drug verdicts

Rule-based calls derived from genotype (rules in
`Pf7_resistance_classification.pdf`). Drug fields take
`Sensitive`/`Resistant`/`Undetermined`; *hrp* fields take
`nodel`/`del`/`uncallable`.

| Field | Meaning |
|---|---|
| `Sample` | Sample ID. |
| `Chloroquine` `Pyrimethamine` `Sulfadoxine` `Mefloquine` `Artemisinin` `Piperaquine` | Single-drug resistance status. |
| `SP (uncomplicated)` / `SP (IPTp)` | Sulfadoxine–pyrimethamine: treatment vs. preventive-use context. |
| `AS-MQ` | Artesunate–mefloquine (combination). |
| `DHA-PPQ` | Dihydroartemisinin–piperaquine (combination). |
| `HRP2` / `HRP3` / `HRP2 and HRP3` | *hrp2/hrp3* deletions — cause false-negative rapid diagnostic tests. |

## `Pf7_fws.txt` — within-sample diversity

| Field | Meaning |
|---|---|
| `Sample` | Sample ID. |
| `Fws` | Within-sample diversity, 0.15–1.00. ~1.0 = single clone; lower = multiple co-infecting strains (high multiplicity of infection). |
