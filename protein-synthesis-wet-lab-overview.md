# Protein Synthesis in the Wet Lab: A Pipeline Overview

*A conceptual map of how a designed amino acid sequence becomes a characterized protein, written for someone who works upstream of the bench. Emphasis on what each stage is for, what it hands to the next stage, and which steps port cleanly to a liquid handler.*

---

## 0. Orientation

The wet lab half of protein design is a build–test loop with a fairly rigid topology. Each stage consumes a physical product and emits another, and nearly every stage has a quality-control gate that can send you backwards.

| # | Stage | Input | Output | Typical wall time |
|---|-------|-------|--------|-------------------|
| 1 | DNA acquisition | Amino acid sequence | Linear dsDNA fragments | 3 days–2 weeks (vendor) |
| 2 | **Assembly / cloning** | Fragments + vector | Circular plasmid (in vitro) | 1 hr–overnight |
| 3 | Transformation + validation | Plasmid mix | Sequence-verified clone | 2–4 days |
| 4 | Expression | Verified plasmid | Cell pellet (or lysate) | 1–3 days |
| 5 | Lysis + purification | Cell pellet | Purified protein in buffer | 4 hr–2 days |
| 6 | Characterization | Purified protein | Numbers: yield, Tm, kcat, Kd | hours–weeks |
| 7 | Feedback | Measurements | Training/scoring data | — |

**Stages 2 and 3 are where "Gibson" and "Golden Gate" live.** They are both answers to one narrow problem: you have linear pieces of DNA and you need them joined, in a defined order and orientation, into a circular plasmid a cell will replicate. Everything else in this document is context around that joint.

A note on terminology: biochemists say "protein synthesis" to mean two different things. In the cell-biology sense it is translation — ribosomes making polypeptide. In the lab-workflow sense (the sense used here) it is the whole pipeline from gene to purified protein. When your colleagues talk about moving protocols to a robot, they almost always mean stages 1–5.

---

## 1. Getting Physical DNA

### Gene synthesis

Commercial de novo synthesis is the default entry point for designed sequences, since there is no natural template to amplify. The amino acid sequence is **reverse-translated** and **codon-optimized** for the expression host — choosing synonymous codons to match host tRNA abundance, avoid rare codon runs, flatten GC content, remove cryptic ribosome binding sites and splice sites, and (importantly for stage 2) remove internal restriction sites.

Formats you will hear named:

- **Linear fragments** (gBlocks, eBlocks, Gene Fragments) — 300 bp to ~3 kb, cheapest, arrives as dried dsDNA, must be cloned by you.
- **Clonal genes** — delivered already in a plasmid, sequence-verified by the vendor. More expensive, saves stages 2–3.
- **Oligo pools** — tens of thousands of short (~150–300 nt) sequences synthesized on a chip in one order. The economic basis of library-scale design. Individual members are low-abundance and error-prone; they are amplified and assembled into full genes in bulk.

Synthesis is still the dominant latency term in most design cycles and the usual reason the loop is measured in weeks rather than days.

### PCR

Polymerase chain reaction amplifies a defined region of DNA using two primers that bracket it. Thermal cycling of denature → anneal → extend, 25–35 times, doubling the target each round.

In this pipeline PCR mostly plays supporting roles:

- **Amplifying fragments** for assembly, often with **tails** on the primers that add the homology arms or restriction sites that stage 2 needs. This is the key point: the primer is where assembly information is physically written onto the DNA.
- **Overlap extension PCR (SOE-PCR)** — joining two fragments by giving them a shared overlap and letting them prime off each other. An older alternative to Gibson, still used for small jobs.
- **Colony PCR** — amplifying straight out of a bacterial colony to check whether the insert is present, before committing to a miniprep.
- **Error-prone PCR** — deliberately degraded fidelity (Mn²⁺, biased dNTPs) to generate a random mutant library for directed evolution.

High-fidelity polymerases (Q5, Phusion, KOD) are used wherever the product will become a final construct; Taq is fine for diagnostic colony PCR.

### Site-directed mutagenesis

When you want a handful of point mutations in an existing plasmid rather than a new gene, synthesis is overkill. Common approaches:

- **Around-the-horn / KLD** — primers pointing outward from the mutation site amplify the entire plasmid as a linear product, which is then **K**inase-treated, **L**igated, and **D**pnI-digested. DpnI cleaves methylated DNA, destroying the original plasmid template while sparing the PCR product. (That DpnI step appears constantly; whenever a protocol amplifies from a plasmid, DpnI is what stops the parent from contaminating the result.)
- **QuikChange-style** — back-to-back complementary mutagenic primers, whole-plasmid amplification, DpnI.
- **Degenerate-codon libraries** (NNK, NNS, or tailored sets like 22c-trick) — the same chemistry with mixed bases at the target codon, generating a site-saturation library in one reaction. This is the standard way to build the variant panels that deep mutational scanning reads out.

---

## 2. Assembly: The Cloning Step

### Why it exists

A linear gene is useless to a cell. It must be joined to a **vector** — a circular plasmid carrying an origin of replication, a selection marker (usually an antibiotic resistance gene), a promoter, a ribosome binding site, and typically an affinity tag and terminator. Assembly is the operation that joins insert(s) to vector in the right order, right orientation, and right reading frame.

Classical cloning did this with Type II restriction enzymes (EcoRI, BamHI…) that cut *inside* palindromic recognition sites, plus T4 ligase, in two separate steps. It works but is slow, leaves the recognition site as a scar in the product, requires the chosen sites to be absent from your insert, and does not scale past two or three fragments. Gibson and Golden Gate are the two modern replacements, and they solve the problem in opposite ways.

---

### Gibson Assembly

**Purpose.** Join two or more linear DNA fragments that share identical sequence at their ends, seamlessly, in a single isothermal reaction, with no dependence on restriction sites.

**Action.** One tube, three enzymes, 50 °C for 15–60 minutes:

1. **T5 exonuclease** chews back the 5′ ends of every fragment, exposing single-stranded 3′ overhangs.
2. Fragments whose ends are **homologous** (typically 15–40 bp of shared sequence) find each other and anneal.
3. A **high-fidelity polymerase** (Phusion) fills the remaining gaps.
4. **Taq DNA ligase** seals the nicks, yielding covalently closed circular DNA.

The elegance is that T5 is heat-labile and dies during the incubation, so chew-back is self-limiting and does not run past the homology region.

**What goes before.** Fragments must be given matching ends. In practice: PCR each fragment with primers whose 5′ tails carry ~20–30 bp of the neighboring fragment's sequence, or order synthetic fragments with the overlaps designed in. Then DpnI-digest (if amplified from plasmid), clean up (column or magnetic bead), quantify, and mix at roughly equimolar ratios — commonly 2–3× molar excess of insert over vector.

**What goes after.** Transformation directly from the reaction mix.

**Strengths.** Scarless — the junction is exactly the sequence you designed, which matters for protein fusions where a few stray codons would be inserted into the chain. Sequence-agnostic: no need to find or avoid restriction sites. Handles long fragments well.

**Weaknesses.** Every new construct needs its own primer pair with custom tails, which means design work and oligo ordering per construct. Degrades with GC-rich ends, repeats, or hairpins. Reliable to ~4–6 fragments; beyond that, misassembly climbs. Poorly suited to combinatorial libraries, because each pairwise junction is bespoke rather than drawn from a reusable set.

---

### Golden Gate Assembly

**Purpose.** Join many fragments in one pot, in a defined order, using a reusable grammar of standardized junctions — ideal for modular and combinatorial construction.

**Action.** It rests on **Type IIS restriction enzymes** (BsaI, BsmBI/Esp3I, BbsI, SapI, PaqCI), which cut at a fixed distance *outside* their recognition sequence rather than within it. Two consequences follow:

- The overhang left behind is **arbitrary sequence you choose** — usually 4 nt, so 256 possible overhangs — not dictated by the enzyme.
- The recognition site sits on the piece that gets discarded, so once two fragments ligate, **the junction can no longer be cut**. The reaction ratchets forward.

That means restriction and ligation can run simultaneously in one tube. A typical program cycles 37 °C (cut) and 16 °C (ligate) around 30 times, then a 60 °C enzyme inactivation and 80 °C ligase kill. Fragments assemble in exactly one order, because only complementary overhangs ligate.

**What goes before.**
- **Domestication** — internal copies of the enzyme's recognition site must be removed from every part, usually by silent mutation at the codon-optimization stage. This is a one-time cost per part, done computationally.
- **Overhang set design** — the 4-nt junctions must be mutually orthogonal, or ligase will cross-join mismatched overhangs. T4 ligase fidelity has been profiled exhaustively for all 256 overhangs, and overhang sets are now chosen from those empirical data rather than by intuition; with good sets, 20+ fragment assemblies in a single reaction are routine (Potapov et al., 2018).
- Parts are supplied as plasmids from a library, or as synthetic fragments with flanking recognition sites.

**What goes after.** Transformation, same as Gibson. Golden Gate vectors typically carry a dropout cassette (lacZα for blue/white, or a fluorescent protein, or a counter-selection marker like ccdB) in the position the insert replaces, so correct clones are visually distinguishable on the plate.

**Strengths.** One-pot, many fragments, very high efficiency. Parts are **reusable** — once a scaffold, a linker, a promoter, a tag is in the library with standard overhangs, it combines with everything else. Naturally combinatorial: mix n variants of part A with m variants of part B and get n×m constructs in one reaction. Hierarchical standards (**MoClo**, **GoldenBraid**, **Loop**) formalize this into levels — parts → transcription units → multigene constructs.

**Weaknesses.** Domestication overhead. Leaves a 4-bp scar at each junction, though this can be hidden in a codon boundary or a linker. Requires the upfront investment of building a standardized part library, which only pays off at volume.

---

### Choosing between them

| | Gibson | Golden Gate |
|---|---|---|
| Junction defined by | 15–40 bp homology, designed per construct | 4 nt overhang, drawn from a standard set |
| Scar | None | 4 bp per junction |
| Internal sequence constraints | None | No internal Type IIS sites (domestication) |
| Fragments per reaction | ~2–6 | 5–24+ |
| Reaction | Isothermal, 50 °C, 15–60 min | Thermocycled 37/16 °C, ~1 hr |
| Per-construct design cost | High (new primers each time) | Low, once the part library exists |
| Best for | One-off constructs, arbitrary sequence, long inserts | Modular libraries, combinatorial variants, high throughput |
| Automation fit | Good | Excellent |

**The practical rule:** Gibson for bespoke, Golden Gate for repeated. A lab testing 300 scaffold × linker × tag combinations wants Golden Gate; a lab making one chimera from two natural genes wants Gibson.

### Other assembly methods worth recognizing

- **Restriction–ligation (classical)** — still used, still taught, still the vocabulary of plasmid maps.
- **In-Fusion / SLIC / CPEC** — commercial or homebrew variants of homology-based assembly, functionally similar to Gibson with different enzymology.
- **LIC (ligation-independent cloning)** — T4 polymerase's 3′→5′ exonuclease activity in the presence of a single dNTP creates long defined overhangs; fragments anneal without ligase, and the cell repairs the nicks. Popular in structural genomics for standardized expression vectors.
- **Gateway** — λ phage site-specific recombination (attB/attP). Very robust, highly parallelizable, but leaves 25 bp recombination scars that translate into extra residues — generally disqualifying for designed proteins.
- **TOPO cloning** — topoisomerase-mediated, fast, mostly for capturing PCR products rather than building constructs.

---

## 3. Transformation and Clone Validation

Assembly produces a tube of DNA molecules, most of which are correct and some of which are not. The cell is used as both a cloning machine and a purification device: one cell takes up one plasmid, and a colony derived from it is clonal.

**Transformation.** Plasmid DNA is introduced into competent *E. coli* — either chemically competent cells (CaCl₂-treated, ice incubation, 42 °C heat shock for 30–45 s, ice, then recovery in rich medium at 37 °C for an hour) or electrocompetent cells (high-voltage pulse, much higher efficiency, essential for large libraries). A **cloning strain** (DH5α, NEB 5-alpha, Mach1, TOP10) is used here: these are recombination- and endonuclease-deficient so the plasmid stays intact, but they are not expression strains.

**Plating and selection.** The recovery culture is spread on agar containing the antibiotic matching the vector's resistance marker. Only transformed cells grow. Each colony is one clone.

**Validation ladder**, cheapest first:
1. **Colony PCR** — primers flanking the insert; a band of the right size says "something of roughly the right length is in there."
2. **Miniprep** — alkaline lysis plasmid purification from a small overnight culture, yielding a few micrograms of plasmid.
3. **Diagnostic digest** — restriction pattern check. Increasingly skipped.
4. **Sequencing** — Sanger of the insert region, or increasingly **whole-plasmid nanopore sequencing**, which is now cheap enough to verify the entire construct including the backbone. This is the real gate; nothing goes downstream unverified.

**Glycerol stock.** The verified clone is mixed with glycerol and frozen at −80 °C. This is the durable artifact of the whole upstream pipeline — the point at which a designed sequence becomes a reproducible physical asset.

---

## 4. Expression

**Subcloning or direct assembly into an expression vector.** If the verified construct is in a cloning vector, it is moved into one with a strong inducible promoter. Common *E. coli* systems:

- **pET series** — T7 promoter, requires a host carrying T7 RNA polymerase under lac control. Induced with **IPTG**.
- **pBAD** — araBAD promoter, arabinose-inducible, tighter control and tunable.
- Vectors generally encode an **affinity tag** (His₆/His₈, MBP, GST, SUMO, Strep-tag II) at N- or C-terminus, often with a protease cleavage site between tag and protein.

**Host strain choice** is a real design variable:

| Strain | Why |
|---|---|
| BL21(DE3) | Workhorse; protease-deficient (lon, ompT) |
| Rosetta / CodonPlus | Supplies tRNAs for codons rare in *E. coli* |
| SHuffle / Origami | Oxidizing cytoplasm — allows disulfide bond formation |
| ArcticExpress | Cold-adapted chaperones for low-temperature folding |
| C41/C43 | Tolerates toxic and membrane proteins |

**Culture and induction.** Grow at 37 °C to mid-log (OD₆₀₀ ≈ 0.6), then induce. A near-universal trick: **drop the temperature to 16–20 °C at induction** and express overnight. Slower translation gives the chain time to fold and keeps it out of inclusion bodies. **Autoinduction media** (Studier, 2005) removes the need to monitor OD and add inducer at the right moment — the culture induces itself as it exhausts glucose — which is why it is popular in automated and high-throughput settings.

**The main failure mode** is insoluble expression: the protein accumulates in **inclusion bodies**. Remedies are lower temperature, weaker induction, a solubility tag (MBP, SUMO, thioredoxin), a chaperone co-expression plasmid, a different strain, or refolding from denaturant. For designed proteins this is also a signal — poor solubility often means the design is wrong, not just the conditions.

**Non-bacterial hosts.** *Pichia pastoris* and *S. cerevisiae* (secretion, glycosylation), **Sf9/baculovirus** (large eukaryotic complexes), **HEK293/Expi293 and CHO** (mammalian PTMs, antibodies, secreted proteins). Slower and costlier, used when *E. coli* cannot fold or modify the target.

**Cell-free protein synthesis (CFPS).** Transcription and translation run directly in a cell extract or a reconstituted system (PURE), with linear or plasmid DNA added straight to the reaction. No transformation, no culture, no growth — protein in a few hours at 10–100 µL scale. Because it skips all the living-cell steps, **CFPS is by far the most automation-friendly expression route**, and it is the usual choice when hundreds of variants must be expressed in parallel on a deck (Silverman et al., 2020). Yields and folding fidelity are lower than good cell-based expression, so it is typically a screening tool, with hits re-expressed conventionally.

---

## 5. Lysis and Purification

**Harvest.** Centrifuge the culture; discard medium; freeze the pellet (freezing also aids lysis).

**Lysis.** Break the cells open:
- **Sonication** — standard at bench scale, poorly automatable (probe cleaning, heat, aerosols).
- **High-pressure homogenization** (microfluidizer, French press) — for larger volumes.
- **Chemical/enzymatic lysis** (lysozyme + detergent cocktails such as BugBuster or B-PER) — lower yield per cell but requires only pipetting, so **this is what automated workflows use**.
- Add protease inhibitors, DNase/benzonase (to cut viscosity from released genomic DNA).

**Clarification.** Centrifuge or filter to remove debris. The supernatant is "soluble lysate" — the first point at which you learn whether your protein is soluble at all.

**Affinity capture** — the main purification step, exploiting the tag:

| Tag | Resin | Elution |
|---|---|---|
| His₆/His₈ | Ni-NTA or Co²⁺ IMAC | Imidazole gradient |
| GST | Glutathione | Reduced glutathione |
| MBP | Amylose | Maltose |
| Strep-tag II | Strep-Tactin | Desthiobiotin |

**Tag removal.** If the tag would interfere with function or structure, a site-specific protease cuts it off — **TEV**, **3C/PreScission**, or **SUMO protease (Ulp1)**. These proteases are themselves His-tagged, so a second pass over the IMAC column ("reverse IMAC") retains the protease and the cleaved tag while the product flows through.

**Polishing.**
- **Size exclusion chromatography (SEC)** — separates by hydrodynamic radius. Doubles as a diagnostic: the elution profile tells you monomer vs. dimer vs. aggregate, which for a designed protein is often the first real evidence of whether the design behaved.
- **Ion exchange (IEX)** and **hydrophobic interaction (HIC)** — for higher purity or when the tag approach is insufficient.

**Finishing.** Buffer exchange by dialysis, desalting column, or spin concentrator; concentration measurement; aliquot and flash-freeze.

**High-throughput variant.** Replace gravity columns with **magnetic Ni beads in 96-well plates** and the entire capture step becomes pipetting plus a magnet — fully robot-compatible. Chromatography (FPLC) does not miniaturize as gracefully, so parallel workflows usually stop at affinity-purified material and reserve SEC for the hits.

---

## 6. Characterization

What "worked" means, quantitatively.

**Quantity and purity**
- **A₂₈₀** with an extinction coefficient computed from the sequence (Trp/Tyr/Cys content); fast, non-destructive, the default.
- **Bradford / BCA** colorimetric assays — plate-readable, ideal for automation.
- **SDS-PAGE** — denaturing gel; the canonical purity and size readout. Capillary electrophoresis instruments (LabChip, Bioanalyzer) give the same information in plate format.

**Identity**
- **Intact mass LC-MS** — confirms the exact expected molecular weight, catching truncations, missed cleavages, unexpected modifications, and retained initiator methionine. Increasingly the real identity gate for designed proteins.
- **Peptide mapping** (trypsin digest + MS/MS) for sequence coverage.

**Folding and stability**
- **Circular dichroism (CD)** — far-UV spectrum reports secondary structure content; a thermal melt reports cooperativity and Tm.
- **nanoDSF / differential scanning fluorimetry (DSF, "thermal shift")** — Tm from intrinsic Trp fluorescence or an environment-sensitive dye (SYPRO Orange). Plate-based, cheap, fast; the standard high-throughput stability screen and also a buffer-optimization tool (Niesen et al., 2007).
- **SEC-MALS** — absolute molar mass and oligomeric state, independent of shape.
- **DLS** — rapid aggregation check.

**Function**
- **Enzyme kinetics** — kcat, KM, kcat/KM via continuous spectrophotometric or fluorogenic assays, or endpoint plate assays.
- **Binding** — **SPR** (Biacore), **BLI** (Octet), **ITC**, **MST**, fluorescence polarization. These give kon/koff/Kd.

**Structure**
- **X-ray crystallography**, **cryo-EM**, **NMR**. Slow and selective, but for a de novo design the structure is the only direct test of whether the predicted fold is the actual fold. Increasingly the comparison is made against a predicted model rather than to a homolog.

---

## 7. Screening and Selection at Scale

When the variant count exceeds what can be individually purified, the lab switches from *measure each one* to *let the population sort itself*.

- **Yeast surface display** — each cell displays one variant fused to a cell-wall protein; fluorescently labeled target plus **FACS** sorts by binding affinity. The workhorse for affinity maturation.
- **Phage display** — variants on phage coat proteins, enriched by panning against immobilized target.
- **mRNA / ribosome display** — fully in vitro, library sizes up to 10¹³.
- **Growth-coupled selection** — the designed activity is made essential for survival, so the plate does the screening.
- **Deep mutational scanning (DMS)** — a comprehensive variant library passed through a selection, with **NGS** counting each variant before and after. Produces a fitness value for essentially every single mutation in a protein (Fowler & Fields, 2014). This is the data type that most directly feeds back into computational models.

---

## 8. The Return Path

Measurements flow back as: training data for sequence–function models, validation of design methods, constraints for the next round, and — frequently the most valuable output — the surprises, where the model confidently predicted something the protein did not do.

The practical bottleneck here is rarely the measurement. It is **provenance**: keeping a reliable join between a design identifier, a plasmid, a well, a culture, a purification fraction, and a measured number. Automation makes this better (every transfer is logged) only if the plate maps are machine-readable from the start.

---

## 9. Notes on Automation

Since the point of this exercise is moving these protocols onto a liquid handler, it is worth being explicit about which parts port cleanly.

**Ports well (pipetting-limited):**
- PCR and assembly reaction setup, including master mixes and molar-ratio calculations
- Serial dilutions and **DNA concentration normalization** — tedious and error-prone by hand, trivially scripted
- Magnetic bead cleanups (PCR purification, plasmid prep, protein capture)
- Transformation, including heat shock, with an on-deck thermal block
- Plate-based expression (especially cell-free) and plate-reader assays
- Golden Gate in particular, because the thermal program is identical for every construct and the junction grammar is fixed — no construct-specific reagents

**Needs dedicated hardware:**
- Colony picking (requires an imaging colony picker)
- Centrifugation (integrated plate centrifuge, or avoid by using filter plates)
- Sonication (substitute chemical lysis)
- Chromatography beyond bead capture (FPLC does not sit on a deck)
- Incubation and shaking (plate incubator/shaker modules)

**Design consequences worth flagging to your colleagues:**
- **Golden Gate rewards automation more than Gibson does.** Gibson needs a fresh primer pair per construct, so the oligo ordering step stays manual and serial. Golden Gate draws from a fixed part library with fixed overhangs — the robot runs the same program regardless of what is being built.
- **Cell-free expression removes the single worst automation bottleneck**, which is the transformation → overnight outgrowth → colony picking → overnight culture chain. Four days collapse to four hours, at the cost of yield.
- **Errors propagate multiplicatively.** A 10% error in a DNA concentration measurement becomes a 10% error in molar ratio becomes a skewed assembly. Normalization accuracy matters more on a robot than at the bench, because nobody is watching the individual reaction.

**Common deck failure modes:** evaporation from edge wells over long incubations (use a humidified chamber or sacrificial edge rows); viscous reagents (glycerol-containing enzyme stocks, PEG-containing ligase buffers) mis-aspirating; inadequate mixing at small volumes; tip dead volume at the bottom of low-volume reactions; cross-contamination from insufficient tip changes in multi-template PCR.

**Protocol representation** is worth deciding early. Opentrons' Python API, Autoprotocol, PyLabRobot, and SiLA2 all exist as ways to express a protocol as code rather than as a PDF. Whichever is chosen, the useful property is that the protocol and the plate map are the same artifact — that is what makes the measurement traceable back to the design.

---

## Glossary

| Term | Meaning |
|---|---|
| **Vector / backbone** | The plasmid providing origin, selection marker, promoter, tag |
| **Insert** | The sequence of interest being cloned in |
| **Construct** | Vector + insert, the finished plasmid |
| **Scar** | Extra bases left at a junction by the cloning method |
| **Domestication** | Removing internal restriction sites from a part by silent mutation |
| **Competent cells** | Bacteria chemically or electrically prepared to take up DNA |
| **Miniprep** | Small-scale plasmid purification from an overnight culture |
| **Induction** | Triggering expression from an inducible promoter |
| **Inclusion bodies** | Insoluble aggregates of misfolded overexpressed protein |
| **IMAC** | Immobilized metal affinity chromatography (His-tag purification) |
| **Tm** | Melting temperature; the standard stability metric |
| **DpnI** | Enzyme that destroys methylated (template) DNA, sparing PCR product |
| **Type IIS enzyme** | Restriction enzyme cutting outside its recognition site |

---

## References

Core methods papers, all peer-reviewed:

- Gibson DG, Young L, Chuang R-Y, Venter JC, Hutchison CA, Smith HO. Enzymatic assembly of DNA molecules up to several hundred kilobases. *Nature Methods*. 2009;6(5):343–345. doi:10.1038/nmeth.1318
- Engler C, Kandzia R, Marillonnet S. A one pot, one step, precision cloning method with high throughput capability. *PLoS ONE*. 2008;3(11):e3647. doi:10.1371/journal.pone.0003647
- Engler C, Gruetzner R, Kandzia R, Marillonnet S. Golden Gate shuffling: a one-pot DNA shuffling method based on type IIs restriction enzymes. *PLoS ONE*. 2009;4(5):e5553. doi:10.1371/journal.pone.0005553
- Weber E, Engler C, Gruetzner R, Werner S, Marillonnet S. A modular cloning system for standardized assembly of multigene constructs. *PLoS ONE*. 2011;6(2):e16765. doi:10.1371/journal.pone.0016765
- Potapov V, Ong JL, Kucera RB, et al. Comprehensive profiling of four-base overhang ligation fidelity by T4 DNA ligase and application to DNA assembly. *ACS Synthetic Biology*. 2018;7(11):2665–2674. doi:10.1021/acssynbio.8b00333
- Studier FW. Protein production by auto-induction in high density shaking cultures. *Protein Expression and Purification*. 2005;41(1):207–234. doi:10.1016/j.pep.2005.01.016
- Rosano GL, Ceccarelli EA. Recombinant protein expression in *Escherichia coli*: advances and challenges. *Frontiers in Microbiology*. 2014;5:172. doi:10.3389/fmicb.2014.00172
- Silverman AD, Karim AS, Jewett MC. Cell-free gene expression: an expanded repertoire of applications. *Nature Reviews Genetics*. 2020;21(3):151–170. doi:10.1038/s41576-019-0186-3
- Niesen FH, Berglund H, Vedadi M. The use of differential scanning fluorimetry to detect ligand interactions that promote protein stability. *Nature Protocols*. 2007;2(9):2212–2221. doi:10.1038/nprot.2007.321
- Fowler DM, Fields S. Deep mutational scanning: a new style of protein science. *Nature Methods*. 2014;11(8):801–807. doi:10.1038/nmeth.3027

The Gibson, Engler, and Potapov entries above were verified against their published records. The remainder are standard citations in this literature but are worth confirming before they go into a manuscript.
