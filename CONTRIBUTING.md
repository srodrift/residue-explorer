# Contributing

Thanks for looking. This is a single static HTML file with no build step — clone it, open
`index.html` in a browser, and you are running the whole thing.

## Ground rules for this codebase

1. **Every number on screen traces to a fetched file.** If a value cannot be computed from the
   sequence or the coordinates, it does not go on the page. No illustrative figures.
2. **State what the method does not establish.** The footer and the provenance popovers exist to
   carry caveats. A new measurement needs its caveat written at the same time.
3. **Reject data that fails a check rather than displaying it.** The structure verifier exists
   because PDBe's top-ranked hit for p53 agrees with the sequence on 4.5% of residues. Prefer a
   visible rejection to a confident-looking wrong answer.
4. **No runtime dependencies.** The only external resource is the 3D rendering library, loaded
   from a CDN. Keep it that way.

## Testing a change

The app cannot work from `file://`-less sandboxes or inside embedded preview panes, because it
fetches from UniProt, EBI and RCSB at load time. Test it as a real page.

Load these five before opening a pull request — each exercises a different path:

| protein | accession | what it exercises |
|---|---|---|
| Haemoglobin β | `P68871` | ligands present, zero offset |
| Lysozyme | `P00698` | negative numbering offset (−18) |
| p53 | `P04637` | long chain, compact track, structure rejected by the verifier |
| PI3Kα | `P42336` | large structure, reduced SASA point count |
| Myoglobin | `P02144` | single chain, near-complete coverage |

CI runs `node --check` on the inline script and verifies that every element id referenced in
JavaScript exists in the document. It does not and cannot check that the page looks right.

## Reporting a problem

Open an issue with the accession you loaded and the browser console output. "It didn't work" with
no accession is not actionable — the behaviour differs a lot between proteins.
