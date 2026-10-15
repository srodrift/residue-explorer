# Changelog

All notable changes to this project are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.2.0]

### Added
- Light theme, with the preference stored locally and the system setting respected on first visit.
- Keyboard navigation: any amino-acid letter jumps to the next residue of that type
  (shift for previous), arrows step one residue, up/down jump ten, `1`–`4` switch colour mode,
  `Home`/`End` go to the chain ends.
- CI that extracts the inline script and runs `node --check` on it, and verifies that every
  element id referenced in JavaScript exists in the document.
- A coverage warning when a structure supplies coordinates for less than 80% of the sequence.

### Fixed
- **Structures are now verified against the sequence before use.** Candidate entries are checked
  residue-by-residue; anything below 90% agreement is rejected and the next candidate tried,
  falling back to the AlphaFold model. Without this, p53 loaded PDBe's top-ranked hit
  (`9R2Q` chain K), which agrees on only 4.5% of residues, and displayed confident numbers for
  the wrong molecule.
- Solvent-accessibility sphere-point count now scales with structure size, and the computation
  yields a frame first, so large proteins no longer appear to hang the tab.

## [0.1.0]

### Added
- Initial release: accession or gene-name input, UniProt sequence and annotations, best mapped
  experimental structure via the PDBe SIFTS mapping with AlphaFold fallback, in-browser ligand
  distances and Shrake–Rupley solvent accessibility, four linked colour modes, bidirectional
  sequence/structure selection, shareable URL state, and per-number provenance popovers.
