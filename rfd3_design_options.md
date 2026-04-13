# RFDiffusion3 Design Options

## Section 1 — Overview of input methods

Three ways to pass inputs to rfd3:

- **JSON/YAML file** (`inputs=path/to/file.json` or `.yaml`): A dict where each key is a design ID and each value is a `DesignInputSpecification` dict. Supports a top-level `global_args` key whose entries are merged into every design spec in the file.
- **Direct PDB/CIF file** (`inputs=path/to/input.pdb`): Single design; the spec is built from the `input` key only. Further per-design params can be set via `specification` overrides in the engine config.
- **CLI** (`rfd3 design inputs=...`): Hydra overrides; per-design args go in the input JSON/YAML, engine-level args are set via CLI as shown in Section 3.

---

## Section 2 — Per-design specification (`DesignInputSpecification`)

Source: `rfd3/src/rfd3/inference/input_parsing.py`

| Field | Type | Default | Description |
|---|---|---|---|
| `input` | str | None | Path to input PDB or CIF file |
| `contig` | str / dict | None | Contig string (e.g. `A1-10,50,B5-15`) or residue dict |
| `ligand` | str | None | Comma-separated ligand code(s) to include (e.g. `"BCA"` or `"NAD,MG"`) |
| `select_fixed_atoms` | str / dict | None (all fixed by default) | Residue/ligand → comma-sep atom names; `True`=fix all, `False`=fix none, contig string, or dict |
| `select_unfixed_sequence` | str / dict | False | Components to diffuse sequence for; same formats as `select_fixed_atoms`. Excludes ligands (always fixed sequence). |
| `length` | str | None | Design length constraint as `"min-max"` or single int. Constrains length of contig if provided. |
| `unindex` | str / dict | None | Components to include without index constraint (positional but not sequence-specific) |
| `redesign_motif_sidechains` | bool / str | False | If True, diffuse sidechains of scaffold residues (fixed backbone, free sequence) |
| `partial_t` | float | None | Angstroms of noise for partial diffusion (≤15 recommended); turns on partial diffusion mode |
| `select_buried` | str / dict | None | RASA buried conditioning selection |
| `select_partially_buried` | str / dict | None | RASA partially-buried conditioning selection |
| `select_exposed` | str / dict | None | RASA exposed conditioning selection |
| `select_hbond_acceptor` | str / dict | None | Atom-wise H-bond acceptor conditioning |
| `select_hbond_donor` | str / dict | None | Atom-wise H-bond donor conditioning |
| `select_hotspots` | str / dict | None | Atom-level or token-level hotspots (PPI use case) |
| `plddt_enhanced` | bool | True | Enable pLDDT global conditioning |
| `is_non_loopy` | bool | None | Non-loopy secondary structure conditioning |
| `ori_token` | list[float] | None | Explicit 3D origin coordinates |
| `infer_ori_strategy` | str | None | `"com"` or `"hotspots"` — how to infer origin if not explicitly set |
| `symmetry` | dict | None | Symmetry specification (see docs/symmetry.md) |
| `extra` | dict | {} | Extra metadata passed through to outputs (not used for inference) |
| `dialect` | int | 2 | Input dialect version; 1=legacy, 2=release |

> **Note:** `DesignInputSpecification` uses `extra="forbid"` — unrecognized keys raise a validation error.

### `select_fixed_atoms` dict format

Also applies to `select_unfixed_sequence`, `select_hotspots`, etc.:

```yaml
select_fixed_atoms:
  A136: "NE,CD,CZ"          # protein residue: chain+resid → comma-sep atom names
  A137: BKBN                 # special keyword: backbone atoms only
  A138: []                   # empty list = fix no atoms
  ACP: "O3G,PG,O1G"         # ligand: 3-letter code → comma-sep atom names
  MG: MG                     # single-atom ligand
```

### Contig string format

| Token | Meaning |
|---|---|
| `A10` | Single scaffold residue (chain A, resid 10) |
| `A10-20` | Scaffold residue range (chain A, resid 10–20) |
| `50` | Exactly 50 diffused (free) residues |
| `50-100` | 50–100 diffused residues (length-sampled) |
| `/0` | Chain break (start new output chain) |
| `0` | Zero-length gap (chain break within output chain) |

Examples:
- `A1-10,50,B5-15` — scaffold from A1-10, 50 free residues, scaffold from B5-15
- `100-100,/0,B1-150` — 100 free residues (chain 1), chain break, scaffold B1-150 (chain 2)

### `global_args` in JSON/YAML

A top-level key in the input file; entries are merged into every design spec:

```json
{
  "global_args": {"infer_ori_strategy": "hotspots"},
  "design_A": {"input": "...", "contig": "..."},
  "design_B": {"input": "...", "contig": "..."}
}
```

---

## Section 3 — Engine-level options (`RFD3InferenceConfig` / CLI)

Source: `rfd3/src/rfd3/engine.py :: RFD3InferenceConfig`
Defaults from: `rfd3/configs/inference_engine/rfdiffusion3.yaml`

| Arg | CLI override | Default | Description |
|---|---|---|---|
| `ckpt_path` | `ckpt_path=rfd3` | `"rfd3"` | Checkpoint path or alias |
| `inputs` | `inputs=path.json` | required | Input file(s), comma-separated, or direct PDB |
| `out_dir` | `out_dir=./outputs` | required | Output directory |
| `n_batches` | `n_batches=4` | 1 | Number of designs per input spec |
| `diffusion_batch_size` | `diffusion_batch_size=8` | 8 | GPU batch size |
| `specification` | `specification.key=val` | `{}` | Global per-spec overrides merged into all design specs |
| `skip_existing` | `skip_existing=false` | True | Skip already-generated designs |
| `json_keys_subset` | `json_keys_subset=A,B` | null | Subset of JSON keys to run |
| `global_prefix` | `global_prefix=run1_` | null | Prefix for output filenames |
| `prevalidate_inputs` | `prevalidate_inputs=true` | False | Validate all specs before running |
| `low_memory_mode` | `low_memory_mode=true` | False | Memory-efficient tokenization |
| `dump_trajectories` | `dump_trajectories=true` | False | Save denoising trajectories |
| `cleanup_guideposts` | `cleanup_guideposts=false` | True | Remove guidepost atoms from output |
| `cleanup_virtual_atoms` | `cleanup_virtual_atoms=false` | True | Remove virtual atoms from output |
| `seed` | `seed=42` | null | Random seed |
| `verbose` | `verbose=true` | False | Verbose logging |

---

## Section 4 — Inference sampler options (`inference_sampler.*`)

Source: `rfd3/src/rfd3/model/inference_sampler.py :: SampleDiffusionConfig`
Set via YAML under the `inference_sampler:` key or as `inference_sampler.key=val` on CLI.

| Arg | Default | Description |
|---|---|---|
| `num_timesteps` | 200 | Diffusion steps |
| `step_scale` | 1.5 | Step size scale (higher = less diverse, more designable) |
| `noise_scale` | 1.003 | Noise scale |
| `gamma_0` | 0.6 | Stochasticity at t=0; set to 0.0 for ODE (deterministic) sampling |
| `gamma_min` | 1.0 | Min stochasticity |
| `p` | 7 | Noise schedule exponent |
| `center_option` | `"all"` | Centering strategy: `"all"`, `"motif"`, or `"diffuse"` |
| `s_trans` | 1.0 | Translational noise scale for augmentation |
| `s_jitter_origin` | 0.0 | Gaussian jitter sigma for origin token position |
| `use_classifier_free_guidance` | False | Enable CFG |
| `cfg_scale` | 1.5 | CFG guidance scale (1.5 in default yaml; 2.0 in dataclass) |
| `cfg_t_max` | null | Max timestep to apply CFG guidance |
| `allow_realignment` | False | Allow motif realignment during sampling |

---

## Section 5 — Input format examples

### Minimal JSON (active site scaffolding)

```json
{
  "my_design": {
    "input": "./structures/target.pdb",
    "ligand": "NAD",
    "contig": "A10,50,A25,30,A40",
    "select_fixed_atoms": {
      "A10": "N,CA,C,CB",
      "A25": "OE1,CD",
      "NAD": "C1D,C2D"
    }
  }
}
```

### PPI YAML (with global_args and hotspots)

```yaml
global_args:
  infer_ori_strategy: hotspots

binder_design:
  input: target.pdb
  contig: 40-120,/0,B1-150
  length: 190-270
  select_hotspots:
    B64: CD2,CZ
    B88: CG,CZ
  select_fixed_atoms:
    B25: []
    B26: BKBN
    B27: CA,CB,OG
  is_non_loopy: true
```

### CLI invocation

```bash
rfd3 design \
  inputs=mcsa_41_rfd3.json \
  out_dir=./outputs \
  n_batches=10 \
  ckpt_path=rfd3
```

### Using `specification` to set global per-design overrides via CLI

```bash
rfd3 design \
  inputs=mcsa_41_rfd3.json \
  out_dir=./outputs \
  n_batches=4 \
  specification.partial_t=5.0 \
  specification.redesign_motif_sidechains=true
```
