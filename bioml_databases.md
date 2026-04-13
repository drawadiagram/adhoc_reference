# Open Databases for ML & High-Throughput Screening in Biomolecular Design

---

## 🧬 Protein Structure & Function

| Database | Description | URL |
|----------|-------------|-----|
| **PDB** (Protein Data Bank) | Experimentally determined 3D structures of proteins, nucleic acids, and complexes | rcsb.org |
| **Chemical Component Dictionary (CCD)** | Standardized definitions of all small molecules and residues in PDB structures | wwpdb.org/data/ccd |
| **AlphaFold DB** | Predicted protein structures for hundreds of millions of UniProt sequences | alphafold.ebi.ac.uk |
| **UniProt / Swiss-Prot** | Protein sequences, functional annotations, and variants | uniprot.org |
| **Pfam / InterPro** | Protein family classifications, domains, and motifs | ebi.ac.uk/interpro |
| **SCOP2 / CATH** | Hierarchical classification of protein structural domains | cathdb.info |
| **BioLiP** | Curated ligand–protein binding interactions derived from PDB | zhanggroup.org/BioLiP |

---

## 🔬 Small Molecules & Chemical Space

| Database | Description | URL |
|----------|-------------|-----|
| **PubChem** | Largest open chemical database; compounds, bioassays, and biological activities | pubchem.ncbi.nlm.nih.gov |
| **ChEMBL** | Manually curated bioactivity data for drug-like molecules | ebi.ac.uk/chembl |
| **ZINC** | Commercially available compounds in ready-to-dock formats | zinc.docking.org |
| **ChemSpider** | Chemical structure repository with links to properties and literature | chemspider.com |
| **COCONUT** | Natural products database for drug discovery | coconut.naturalproducts.net |
| **GDB Databases** | Enumerated virtual chemical space (GDB-11, GDB-17) | gdb.unibe.ch |
| **Enamine REAL** | Synthetically accessible compounds for virtual screening | enamine.net/real-compounds |

---

## 🌡️ Specialized Protein Subdomain Databases

| Database | Description | URL |
|----------|-------------|-----|
| **OPM** (Orientations of Proteins in Membranes) | Spatial positions of membrane proteins relative to the lipid bilayer | opm.phar.umich.edu |
| **mpstruc** (Membrane Proteins of Known 3D Structure) | Curated list of membrane protein structures from PDB | blanco.biomol.uci.edu/mpstruc |
| **PDBTM** | Transmembrane topology annotations for PDB proteins | pdbtm.unitmp.org |
| **ExtremeDB / ProKEDB** | Enzymes from extremophiles (thermophiles, halophiles, acidophiles, etc.) | — |
| **HotProt** | Thermostable proteins from thermophilic and hyperthermophilic organisms | hotprot.protdyn-database.org |
| **ProThermDB** | Thermodynamic data for protein mutations; useful for thermostability ML | web.iitm.ac.in/bioinfo2/prothermdb |
| **Extremophile proteomes (UniProt)** | Filtered UniProt proteomes for archaea and extremophilic bacteria (e.g., *T. maritima*, *P. furiosus*) | uniprot.org/proteomes |
| **BRENDA** | Enzyme function, kinetics, and organism-of-origin data including extremophiles | brenda-enzymes.org |
| **CAZy** | Carbohydrate-active enzymes classified by family; rich in thermostable variants | cazy.org |
| **MetalPDB** | Metal-binding sites in protein structures; relevant to metalloenzyme design | metalweb.cerm.unifi.it |
| **DisProt** | Experimentally characterized intrinsically disordered proteins (IDPs) | disprot.org |
| **MobiDB** | Integrated annotations of intrinsic disorder and conformational dynamics | mobidb.org |
| **GPCRDB** | Structure-based sequence alignments, variants, and mutagenesis data for GPCRs | gpcrdb.org |
| **Kinase.com / KinBase** | Comprehensive classification and annotation of the human kinome | kinase.com |
| **SAbDab** (Structural Antibody Database) | All antibody structures in PDB with curated annotations | opig.stats.ox.ac.uk/webapps/newsabdab |
| **IMGT** | Immunogenetics reference for antibody/TCR sequences and structures | imgt.org |
| **CoCoNat / APD3** (Antimicrobial Peptide Database) | Experimentally validated antimicrobial peptides | aps.unmc.edu/AP |
| **PepBDB** | Protein–peptide binding complexes from PDB | huanglab.phys.hust.edu.cn/pepbdb |
| **NucleicNet / RNAcentral** | Non-coding RNA sequences and structures | rnacentral.org |
| **Rfam** | RNA families, alignments, and covariance models | rfam.org |

---

## 💊 Drug Discovery & Targets

| Database | Description | URL |
|----------|-------------|-----|
| **DrugBank** | Comprehensive drug, target, and mechanism data | drugbank.com |
| **ChEMBL** | Drug-target bioactivity (also listed above) | ebi.ac.uk/chembl |
| **DTC** (Drug Target Commons) | Curated drug–target interaction data | dtc.zju.edu.cn |
| **BindingDB** | Measured binding affinities for protein–ligand pairs | bindingdb.org |
| **PDSP Ki Database** | Psychoactive drug binding affinities at CNS receptors | pdsp.unc.edu/databases/kidb |
| **TTD** (Therapeutic Target Database) | Approved and clinical-stage drug targets and pathways | db.idrblab.net/ttd |
| **OpenTargets** | Genetics-based evidence linking targets to diseases | opentargets.org |

---

## ⚗️ Protein–Ligand Interactions & Docking

| Database | Description | URL |
|----------|-------------|-----|
| **PDBbind** | Experimentally measured binding affinities for PDB complexes | pdbbind-cn.org |
| **MOAD** (Mother of All Databases) | Curated protein–ligand complexes with binding data | bindingmoad.org |
| **CrossDocked2020** | Large-scale cross-docking dataset for ML-based scoring functions | github.com/gnina/crossdocked2020 |
| **Astex Diverse Set** | Benchmark dataset for fragment-based drug design | — |

---

## 🧪 Biochemical Assays & HTS Data

| Database | Description | URL |
|----------|-------------|-----|
| **PubChem BioAssay** | HTS results, dose-response data, and assay metadata | pubchem.ncbi.nlm.nih.gov/assay |
| **ChEMBL Assays** | Curated activity data from literature and deposited datasets | ebi.ac.uk/chembl |
| **NCATS OpenData** | HTS datasets from NIH NCATS screening campaigns | opendata.ncats.nih.gov |
| **ChemIDplus** | Toxicity and regulatory data for chemicals | — |
| **Tox21** | Toxicology in the 21st Century HTS dataset (~10K compounds) | tox21.gov |
| **hERG Central** | Experimental hERG channel blockade data (cardiac safety) | hergcentral.org |

---

## 🧫 Genomics, Proteomics & Omics

| Database | Description | URL |
|----------|-------------|-----|
| **NCBI GEO** | Gene expression datasets (microarray, RNA-seq) | ncbi.nlm.nih.gov/geo |
| **ClinVar** | Clinically interpreted genomic variants | ncbi.nlm.nih.gov/clinvar |
| **gnomAD** | Population-scale human variant frequencies | gnomad.broadinstitute.org |
| **STRING** | Protein–protein interaction networks | string-db.org |
| **BioGRID** | Curated genetic and protein interactions | thebiogrid.org |
| **PRIDE** | Mass spectrometry proteomics datasets | ebi.ac.uk/pride |

---

## 🚦 ADMET & Drug Safety

| Database | Description | URL |
|----------|-------------|-----|
| **ADMETlab 2.0 DB** | Curated ADMET properties for drug-like molecules with ML-ready exports | admetmesh.scbdd.com |
| **Tox21** | HTS toxicology panel (~10K compounds, 12 assays) from NIH/EPA/FDA collaboration | tox21.gov |
| **ToxCast** | EPA in vitro HTS data across 1,000+ assays for thousands of chemicals | epa.gov/comptox-tools/toxcast |
| **ClinTox (MoleculeNet)** | Clinical trial toxicity failures and FDA approval labels; standard ML benchmark | moleculenet.org |
| **hERG Central** | Experimental hERG channel blockade data for cardiac safety prediction | hergcentral.org |
| **DILI-rank** | Drug-induced liver injury severity rankings for FDA-approved drugs | — |
| **DILIst** | Curated DILI annotations used widely in hepatotoxicity model training | — |
| **SuperCYP** | CYP450 interaction data (substrates, inhibitors, inducers) | bioinformatics.charite.de/supercyp |
| **DrugMatrix** | Rat in vivo toxicogenomics dataset from NTP; gene expression + pathology | ntp.niehs.nih.gov/data/drugmatrix |
| **Open TG-GATEs** | Human and rat hepatocyte transcriptomic responses to ~170 compounds | goo.gl/opentggates |
| **SIDER** | Side effects and indications extracted from drug package inserts | sideeffects.embl.de |
| **FAERS** (FDA Adverse Event Reporting System) | Post-market pharmacovigilance reports; large-scale signal mining resource | fda.gov/faers |
| **ChEMBL ADMET Annotations** | Curated absorption, permeability, metabolic stability, and clearance data | ebi.ac.uk/chembl |
| **Caco-2 / PAMPA datasets (TDC)** | Membrane permeability benchmarks in Therapeutics Data Commons | tdcommons.ai |
| **PPBR / VDss datasets (TDC)** | Plasma protein binding and volume of distribution benchmarks | tdcommons.ai |
| **BBB datasets (B3DB)** | Blood–brain barrier permeability data for CNS drug design | github.com/theochem/B3DB |

---

## 🌿 Natural Products & Metabolomics

| Database | Description | URL |
|----------|-------------|-----|
| **HMDB** (Human Metabolome Database) | Human metabolites with spectral and biological data | hmdb.ca |
| **METLIN** | Metabolite and tandem MS spectral database | metlin.scripps.edu |
| **LOTUS** | Annotated natural product–organism pairs | lotus.naturalproducts.net |
| **NP Atlas** | Microbial natural products with literature links | npatlas.org |

---

## 🤖 ML-Ready Benchmarks & Curated Splits

| Dataset | Description | URL |
|---------|-------------|-----|
| **MoleculeNet** | Benchmark suite for molecular ML (ESOL, BACE, HIV, etc.) | moleculenet.org |
| **TAPE** | Protein sequence representation benchmarks | github.com/songlab-cal/tape |
| **PEER** | Comprehensive protein ML benchmark | github.com/DeepGraphLearning/PEER_Benchmark |
| **FS-Mol** | Few-shot learning benchmark for drug discovery | github.com/microsoft/FS-Mol |
| **TDC** (Therapeutics Data Commons) | Unified ML benchmarks across ADMET, interactions, and generation | tdcommons.ai |

---

---

## 🛠️ Open Source Software for Biomolecular Data Manipulation & Scoring

### Molecular Design & Cheminformatics

| Software | Language | Description |
|----------|----------|-------------|
| **RDKit** | Python/C++ | De facto standard for cheminformatics: molecule parsing, fingerprints, descriptors, reaction handling, 2D/3D depiction | 
| **OpenBabel** | Python/C++ | Interconversion of 100+ chemical file formats; protonation, tautomers, conformer generation |
| **CDK** (Chemistry Development Kit) | Java/Python | Cheminformatics toolkit for descriptor calculation, SMARTS matching, and ring perception |
| **Indigo** | Python/C++/Java | Molecule and reaction processing, standardization, and rendering |
| **molvs** | Python | Molecule validation and standardization built on RDKit |
| **Datamol** | Python | High-level RDKit wrapper for ML-friendly molecular pipelines |

---

### Molecular Mechanics & Force Fields

| Software | Language | Description |
|----------|----------|-------------|
| **OpenMM** | Python/C++ | GPU-accelerated MD and energy minimization; highly scriptable for ML workflows |
| **GROMACS** | C/Python | High-performance MD simulation suite; widely used for protein and membrane systems |
| **AMBER** (AmberTools, open) | Python/C/Fortran | MD and free energy calculations; AmberTools is freely available |
| **NAMD** | C++/Tcl | Scalable MD for large biomolecular systems; VMD integration |
| **OpenFF Toolkit** | Python | Open Force Field Initiative toolkit for SMIRNOFF-format force field parameterization |
| **Parmed** | Python | Cross-engine parameter file manipulation (AMBER, GROMACS, CHARMM) |
| **CHARMM** (open academic) | Fortran/Python | Comprehensive MM/MD suite with rich scripting; free for academic use |

---

### Structure Preparation & Manipulation

| Software | Language | Description |
|----------|----------|-------------|
| **Rosetta** | C++/Python (PyRosetta) | Comprehensive suite for protein structure prediction, design, docking, and energy scoring; PyRosetta exposes full API |
| **Biopython** | Python | PDB parsing, sequence alignment, structure superposition, and biological data I/O |
| **MDAnalysis** | Python | Trajectory analysis and atom selection for MD simulation outputs |
| **MDTraj** | Python | Fast trajectory analysis with broad format support; integrates with ML pipelines |
| **ProDy** | Python | Protein structure, dynamics, and sequence analysis; normal mode analysis |
| **biotite** | Python | Unified framework for sequence and structure analysis; PDB/mmCIF I/O |
| **PDBFixer** | Python | Automated structure preparation: missing residues/atoms, protonation (OpenMM ecosystem) |
| **HTMD** | Python | High-throughput MD preparation and analysis; built-in parameterization workflows |
| **Molmass** | Python | Molecular mass and elemental composition from chemical formulas/SMILES |

---

### Docking & Scoring

| Software | Language | Description |
|----------|----------|-------------|
| **AutoDock Vina** | C++/Python | Fast and widely benchmarked rigid/flexible receptor docking |
| **AutoDock4 / AutoDockTools** | Python/C | Classic docking suite; used extensively in HTS virtual screening |
| **Gnina** | C++/Python | Molecular docking with CNN-based scoring; extends Vina with learned scoring functions |
| **SMINA** | C++ | Vina fork with customizable scoring functions and improved usability |
| **Glide (Schrödinger, free tier)** | — | Industry-standard docking; limited free academic tier via Maestro |
| **rDock / rxDock** | C++/Python | Cavity-based docking for HTS; fast and scriptable |
| **PLANTS** | C++ | Ant-colony optimization-based docking for fragment and lead screening |

---

### Free Energy & Binding Affinity

| Software | Language | Description |
|----------|----------|-------------|
| **MBAR / pymbar** | Python | Multistate Bennett Acceptance Ratio for free energy estimation from simulations |
| **alchemlyb** | Python | End-to-end alchemical free energy analysis; wraps GROMACS, AMBER, NAMD outputs |
| **FESetup** | Python | Automated setup of alchemical free energy simulations |
| **OpenFE** | Python | Open Free Energy consortium toolkit for relative and absolute binding FE calculations |
| **ProFit** | C | Protein structure fitting and RMSD calculation; used in benchmarking pipelines |

---

### Protein Sequence & Property Analysis

| Software | Language | Description |
|----------|----------|-------------|
| **BioPython (SeqIO / PDB)** | Python | Sequence parsing, BLAST interfaces, PDB structure analysis |
| **pyhmmer** | Python | Cython bindings for HMMER; fast profile HMM search for domain annotation |
| **muscle / MAFFT** | C/CLI | Multiple sequence alignment; MAFFT available via Python wrappers |
| **SignalP / NetSurf (CLI)** | CLI | Signal peptide prediction and surface accessibility (DTU Bioinfo tools) |
| **DSSP / mkdssp** | C++/Python | Secondary structure and solvent accessibility assignment from 3D coordinates |
| **FreeSASA** | C/Python | Solvent-accessible surface area calculation |
| **NACCESS** | Fortran/CLI | Residue-level accessibility calculation from PDB structures |

---

### File Format & Data Utilities

| Software | Language | Description |
|----------|----------|-------------|
| **gemmi** | Python/C++ | Fast mmCIF/PDB/MTZ parsing; ideal for large-scale PDB database processing |
| **CIF tools / PDBx** | Python | wwPDB reference tools for CCD and mmCIF format manipulation |
| **Pint / unitpy** | Python | Unit-aware quantities useful for physical property pipelines |
| **PyMOL (open-source)** | Python | Molecular visualization with a scriptable Python API for batch structure analysis |
| **NGLView** | Python | Interactive 3D structure visualization in Jupyter notebooks |
| **py3Dmol** | Python | Lightweight 3D viewer for notebooks using 3Dmol.js |

---

*Last reviewed: 2025. Access conditions vary; most are freely available for academic use. Always verify licensing before commercial applications.*
