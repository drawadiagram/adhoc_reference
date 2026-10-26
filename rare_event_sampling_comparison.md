# Comparing Rare-Event Sampling Methods: TPS, Milestoning, and Umbrella Sampling

All three methods below address the same core problem: molecular events (conformational
transitions, folding, binding, chemical reactions) that are dynamically rare and thus
inaccessible to brute-force molecular dynamics. Each solves this differently — by
resampling trajectory space, by decomposing the pathway into short segments, or by
biasing the potential. Shooting is discussed within the TPS section below rather than
as a fourth peer method, since it's the Monte Carlo move that generates TPS's
trajectory samples, not a method with its own separate output.

---

## 1. Transition Path Sampling (TPS)

**Core idea:** Sample an ensemble of reactive trajectories directly using Monte Carlo
moves in trajectory space, with no bias on the dynamics itself.

- Start from one known transition path connecting reactant state A and product state B.
- Generate new candidate trajectories via **shooting moves**: pick a time slice on the
  current path, perturb it, and re-integrate the dynamics from there. Two common
  variants:
  - *One-way shooting* — perturb momenta (or noise, in Langevin dynamics) at the
    chosen point and re-integrate in only one direction, splicing onto the untouched
    rest of the old path. Cheaper per move, but decorrelates from the old path more
    slowly.
  - *Two-way shooting* — re-integrate both forward and backward from the perturbed
    point, discarding the entire old path. Decorrelates faster but costs more per
    move and can have lower acceptance for stiff/high-barrier systems.
- Accept/reject candidates via a Metropolis criterion so the surviving ensemble is
  correctly weighted by its true dynamical probability.
- Repeat over many MC steps to build a statistically valid ensemble of pathways.

**What you get:** An unbiased ensemble of physical transition pathways. Combined with
transition interface sampling (TIS), this yields rate constants; committor analysis on
the ensemble locates the transition state region.

**Costs/challenges:**
- Each shooting move requires integrating a full or partial trajectory segment —
  expensive per move, and the reason TPS is costlier per sample than the other methods.
- Needs a reasonable initial guess trajectory to seed sampling.
- Convergence can be slow for complex or multi-pathway barriers (though this is also
  a strength: it can reveal unexpected mechanisms).
- Rate constants require additional machinery beyond raw TPS (TIS, or the reactive
  flux formula).
- Shooting-point selection strategy (uniform vs. biased toward high-curvature regions)
  strongly affects acceptance rate and how quickly path space is explored — this is
  a key tuning knob for TPS efficiency.

---

## 2. Milestoning

**Core idea:** Decompose the transition into a series of interfaces ("milestones")
along a progress coordinate, and estimate short trajectory statistics between adjacent
milestones instead of sampling full A→B paths.

- Place milestones (hypersurfaces) between reactant and product states, typically
  along a chosen reaction coordinate or collective variable.
- Run many short, independent simulations that start on one milestone and terminate
  on hitting an adjacent milestone.
- Reconstruct overall kinetics (rate constants, mean first-passage times) from the
  local transition statistics using a Markov (or semi-Markov) renewal framework.

**What you get:** Rate constants and a coarse-grained kinetic network, with excellent
parallelizability since each milestone-to-milestone run is short and independent.

**Costs/challenges:**
- Requires a good progress coordinate and milestone placement up front; poor choices
  introduce non-Markovian error if the memory-loss assumption breaks down.
- Less mechanism-agnostic than TPS — a reaction coordinate is baked in.
- Exact milestoning (which accounts for memory effects) is more complex than the
  standard Markovian version.

---

## 3. Umbrella Sampling (US)

**Core idea:** Bias the potential energy along a chosen reaction coordinate so
high-energy (rare) regions are adequately sampled, then remove the bias statistically
to recover the true free energy profile.

- Choose a reaction coordinate/collective variable and divide it into windows.
- In each window, add a harmonic (or other) biasing potential centered there to force
  sampling of that region.
- Simulate each window independently, typically for a long time to ensure good local
  equilibrium sampling.
- Stitch the biased distributions back together and remove the bias via WHAM or MBAR
  to obtain the potential of mean force (PMF).

**What you get:** An equilibrium free energy profile (PMF) — the thermodynamics of the
transition (barrier heights, relative state stabilities). Kinetics require an
additional dynamical model (e.g., Kramers'/Smoluchowski theory) layered on top.

**Costs/challenges:**
- Fundamentally a thermodynamic method — no direct dynamics or mechanism.
- Strongly dependent on reaction coordinate quality; a poor coordinate or hidden
  orthogonal slow motion can produce a misleading PMF even when sampling is converged.
- Extracting rates reintroduces Markovian/diffusive assumptions that TPS avoids and
  milestoning manages explicitly.
- Windows need sufficient overlap for WHAM/MBAR to stitch correctly.

---

## Comparison Table

| Aspect | TPS (shooting-driven) | Milestoning | Umbrella Sampling |
|---|---|---|---|
| Primary output | Trajectory ensemble; rates via TIS | Rate constants, MFPTs | Free energy profile (PMF) |
| Reaction coordinate needed? | No | Yes | Yes |
| Dynamics preserved? | Yes — fully unbiased | Yes, within each segment | No — biased; recovered post hoc |
| Trajectory length per sample | Full A→B path, built via shooting moves | Short, local segments | Long, per-window equilibration |
| Parallelizability | Moderate (sequential MC chain of shooting moves) | High (independent short runs) | High (independent windows) |
| Mechanistic insight | Rich — can reveal unknown pathways | Constrained by milestone/coordinate choice | Limited to the chosen coordinate |
| Best suited for | Discovering mechanism when coordinate is unknown/untrusted | Efficient rate/kinetics estimation with a trusted coordinate | Thermodynamics/free energy landscape along a trusted coordinate |

---

## How the Methods Relate

A useful mental model: **umbrella sampling gives you thermodynamics** (how deep are
the wells, how high is the barrier), while **TPS and milestoning give you kinetics**
(how fast do you cross, and by what path) — TPS being coordinate-free and
mechanism-discovering, milestoning being coordinate-dependent but far more
parallel-friendly. Within TPS, the **shooting move** is what governs practical
efficiency and cost — its design (one-way vs. two-way, shooting-point bias) determines
how quickly the trajectory ensemble converges.

In practice, these are usually combined rather than treated as competitors:

1. **Umbrella sampling** is often used first to map the free energy landscape and
   locate metastable states/barriers.
2. **Milestone locations** are then frequently chosen based on that landscape (e.g.,
   placed around the identified basins and barrier region).
3. **TPS/TIS** can validate or discover the actual transition mechanism when there's
   doubt that the coordinate used for US or milestoning captures the true reaction
   coordinate — the classic "is my collective variable good enough?" problem, where a
   poor CV can produce a qualitatively wrong PMF even when the sampling itself has
   converged.

## HPC / Practical Note

For workflows built around independent, short-lived task dispatch (e.g., Globus
Compute-style HPC dispatch), **umbrella sampling** and **milestoning** are the more
natural fits — both decompose into embarrassingly parallel independent runs (windows
or milestone segments). **TPS** is inherently more sequential: each new trajectory
in the Monte Carlo chain (generated by a shooting move) depends on the previous
accepted one, which limits parallelization to running multiple independent TPS chains
rather than parallelizing within a single chain.
