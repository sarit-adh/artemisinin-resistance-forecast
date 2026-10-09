# Data

Per-sample genomic drug-resistance data for the malaria parasite *Plasmodium
falciparum* (MalariaGEN **Pf7** and **Pf8** releases). This is the genetic
**outcome layer** for the artemisinin-resistance forecasting project: which
resistance mutations each sequenced parasite carries, and where/when it was
collected. Predictor layers (drug pressure, transmission, connectivity) are
not in this repository.

**Source:** MalariaGEN — https://www.malariagen.net/resource/34/
Cite the MalariaGEN Pf7 and Pf8 data releases and honour the MalariaGEN terms
of use.

## Layout

| Folder | Release | Samples (total / QC-pass) | Years | Files | Field docs |
|---|---|---|---|---:|---|
| [`pf7/`](pf7/) | Pf7 (2023) | 20,864 / 16,203 | 1984–2018 | 5 | [`pf7/README.md`](pf7/README.md) |
| [`pf8/`](pf8/) | Pf8 (newer) | 33,325 / 24,409 | 1966–2022 | 7 | [`pf8/README.md`](pf8/README.md) |

**Pf8 supersedes Pf7**: every Pf7 sample (all 20,864) is included in Pf8, which
adds 12,461 new ones. Prefer Pf8 for current work; Pf7 is kept for
reproducibility and because its `samples` file carries the `Sample was in Pf6`
link.

Each row in every per-sample file = **one sequenced parasite isolate**, keyed
on `Sample`. `.txt`/`.tsv` files are all **tab-separated**. (One exception:
`pf8/Pf8_tandem_duplication_breakpoints.tsv` is a reference catalogue, one row
per known breakpoint, not per sample.)

## Conventions shared by all files (read once)

**Genotype column names** — `gene_codon[REF]`, e.g. `crt_76[K]`: gene *crt*,
codon **76**, bracket = **reference (wild-type) amino acid**. Cell value =
observed amino acid:

- single letter = allele called (differs from `[REF]` → mutant; equals it → wild-type);
- comma-separated (`T,K`) = **mixed call**, multiple co-infecting strains carried different alleles (not an error);
- `-` = uncallable/missing; trailing `*` = non-standard call.

**Copy-number / deletion calls** (`*_dup_call`, `*_final_amplification_call`,
`*_final_deletion_call`): `1` = amplified/deleted, `0` = normal single copy /
no deletion, `-1` = uncallable.

**Genes:** `crt` chloroquine; `dhfr` pyrimethamine; `dhps` sulfadoxine; `mdr1`
multidrug/mefloquine; `pm2`(`pm2_pm3`) plasmepsin-2/3 (piperaquine); `gch1`
GTP-cyclohydrolase-1 (antifolate background); `hrp2`/`hrp3` rapid-test antigens;
**`kelch13` artemisinin** — the forecasting target; `exo`, `arps10`, `fd`,
`mdr2` resistance-background markers.

## Caveats (read before modelling)

- **Pf7 ends in 2018**; the confirmed East-African artemisinin emergences were
  reported ~2019–2021. **Pf8 extends to 2022** and is the release that covers
  them.
- **Sampling is non-random and West-Africa-skewed**; East-African hotspots are
  sparse. Observed frequencies are biased by where sequencing was done.
- **Validated markers (R561H, C469Y, A675V, P441L)** are concentrated in
  Southeast-Asian samples in Pf7; `C580Y` (Mekong) dominates. Check Pf8 for the
  more recent African signal.
- **Location is admin-1 resolution** (centroids), not precise coordinates.
- **Resistance status is rule-derived from genotype**, not clinical phenotype.

When aggregating to place-year frequencies, keep the numerator and denominator
(`n_mutant`, `n_tested`) separately — never collapse to a percentage.
