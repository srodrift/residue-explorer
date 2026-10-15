# Residue explorer

Type a UniProt accession or gene name. The page fetches the sequence, annotations
and best available structure, computes geometry **in your browser**, and links a
sequence track to an interactive 3D view.

No server, no build step, no API keys. One HTML file.

## What it computes locally

| quantity | method |
|---|---|
| Distance to ligand | minimum atom-to-atom distance to the nearest non-water heteroatom group |
| Surface exposure | Shrake–Rupley SASA, 1.4 Å probe, 92 points/atom, normalised to Gly-X-Gly maxima (Tien et al. 2013) |
| Known variants | UniProt "Natural variant" annotations |
| Hydropathy | Kyte–Doolittle |

## Data sources

| source | used for | CORS |
|---|---|---|
| `rest.uniprot.org` | sequence, binding sites, variants, helices | `*` |
| `www.ebi.ac.uk/pdbe` | SIFTS best-structure mapping and numbering offset | `*` |
| `files.rcsb.org` | experimental coordinates | `*` |
| `alphafold.ebi.ac.uk` | predicted structure fallback | `*` |

## Why the numbering offset matters

UniProt and PDB number residues differently — signal peptides, initiator
methionines and expression constructs all shift the frame. The SIFTS mapping
supplies the exact offset per structure, so no protein needs special-casing:

| protein | structure | offset |
|---|---|---|
| Haemoglobin β (P68871) | 1DXT chain B | 0 |
| p53 (P04637) | 9R2Q chain K | +24 |
| Lysozyme (P00698) | 2VB1 chain A | −18 |

## Interaction

- Click a residue in the track, or click the structure — selection is bidirectional.
- `←` `→` move one residue, `↑` `↓` jump ten, `1`–`4` switch colour mode.
- The URL always encodes accession, residue and mode, so any view is shareable.
- "How is this measured?" under each number opens its method and source.
- Chains longer than 340 residues render as a compact track instead of letters.

## Limits

One structure is one conformation. Exposure is computed on the whole deposited
assembly, so interface residues read as buried. Variant counts track research
attention, not just biology. Solvent accessibility is skipped above 14,000 atoms
for speed. Nothing here is a clinical or functional claim.

## Deploy

Static. Drop `index.html` at the root of any host.
