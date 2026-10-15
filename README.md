# Residue explorer

**Type any UniProt accession. Get a sequence track linked to a 3D structure, where every number was computed in your browser from the fetched files.**

[![License: MIT](https://img.shields.io/github/license/srodrift/residue-explorer)](LICENSE)
[![CI](https://img.shields.io/github/actions/workflow/status/srodrift/residue-explorer/ci.yml?branch=main&label=CI)](https://github.com/srodrift/residue-explorer/actions/workflows/ci.yml)
![Runtime dependencies](https://img.shields.io/badge/runtime%20dependencies-0-brightgreen)
![Build step](https://img.shields.io/badge/build%20step-none-brightgreen)

**[Open the app](https://srodrift.github.io/residue-explorer/)** · no install, no account, no backend.

One HTML file. It resolves the accession, pulls the sequence and annotations from UniProt, finds
the best experimental structure through the PDBe SIFTS mapping, fetches coordinates from RCSB,
falls back to the AlphaFold model when nothing experimental verifies, and then computes ligand
distances and solvent accessibility locally. Nothing is precomputed and no server is involved.

## Try these

Each link opens a specific residue and colour mode, because the app keeps its state in the URL.

| protein | link | why it is interesting |
|---|---|---|
| Haemoglobin β | [residue 92, exposure](https://srodrift.github.io/residue-explorer/#acc=P68871&res=92&mode=rsa) | the proximal histidine: 2.15 Å from the haem and 0% exposed |
| Haemoglobin β | [residue 6, hydropathy](https://srodrift.github.io/residue-explorer/#acc=P68871&res=6&mode=kd) | the sickle-cell position — 49% exposed, 21 Å from the haem |
| Lysozyme | [residue 35](https://srodrift.github.io/residue-explorer/#acc=P00698&res=35&mode=lig) | numbering offset of −18 between UniProt and the structure |
| Myoglobin | [residue 93](https://srodrift.github.io/residue-explorer/#acc=P02144&res=93&mode=lig) | the same haem chemistry in a single chain |
| p53 | [DNA-binding core](https://srodrift.github.io/residue-explorer/#acc=P04637&res=248&mode=var) | 393 residues, compact track, and a rejected structure |
| PI3Kα | [catalytic region](https://srodrift.github.io/residue-explorer/#acc=P42336&res=942&mode=rsa) | 1,068 residues, reduced SASA sampling |

## What it computes, locally

| quantity | method |
|---|---|
| Distance to ligand | minimum atom-to-atom distance to the nearest non-water heteroatom group |
| Surface exposure | Shrake–Rupley SASA, 1.4 Å probe, 24–92 sphere points per atom by size, normalised to Gly-X-Gly maxima (Tien et al. 2013) |
| Known variants | UniProt "Natural variant" annotations |
| Hydropathy | Kyte–Doolittle |

## Structures are verified before use

PDBe's top-ranked structure is not always the right molecule. Every candidate is checked
residue-by-residue against the UniProt sequence; anything below 90% agreement is rejected and the
next candidate tried. The agreement percentage is printed on the page.

| protein | structure | offset | sequence agreement |
|---|---|---|---|
| Haemoglobin β | 1DXT chain B | 0 | 100% |
| Lysozyme | 2VB1 chain A | −18 | 100% |
| PI3Kα | 4JPS chain A | 0 | 100% |
| Myoglobin | 3RGK chain A | −1 | 98.7% |
| p53 | 9R2Q chain K | +24 | **4.5% → rejected** |

Signal peptides, initiator methionines and expression constructs all shift numbering, which is why
the offset comes from the SIFTS mapping rather than being assumed.

## Interaction

- Click a residue in the track, or click the structure — selection runs both ways.
- Any amino-acid letter jumps to the next residue of that type; `shift` for the previous one.
- `←` `→` step one residue, `↑` `↓` jump ten, `Home` / `End` for the chain ends, `1`–`4` for colour modes.
- "How is this measured?" under each number opens its method and source.
- Light and dark themes; the system preference is used on first visit.
- Chains longer than 340 residues switch to a compact track.

## Data sources

| source | used for | CORS |
|---|---|---|
| `rest.uniprot.org` | sequence, binding sites, variants, helices | `*` |
| `www.ebi.ac.uk/pdbe` | SIFTS best-structure mapping and numbering offset | `*` |
| `files.rcsb.org` | experimental coordinates | `*` |
| `alphafold.ebi.ac.uk` | predicted structure fallback | `*` |

All four send `Access-Control-Allow-Origin: *`, so the app runs from any origin — but it needs
live internet, and it cannot work inside embedded preview panes that block `fetch()`.

## Limits

One structure is one conformation. Exposure is computed on the whole deposited assembly, so a
residue buried at a chain interface reads as buried — correct for the complex, not for an isolated
chain. Variant counts track research attention as much as biology. Hydropathy is a property of the
amino acid alone, not of its context. Solvent accessibility is skipped above 20,000 atoms. Nothing
here is a clinical or functional claim.

## Development

No build step. Open `index.html` in a browser. See [CONTRIBUTING.md](CONTRIBUTING.md) for the five
proteins to test before a pull request, and [CHANGELOG.md](CHANGELOG.md) for what has changed.

## License

MIT — see [LICENSE](LICENSE).
