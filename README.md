# Enclosed volume, not area, selects the apparent horizon

Code and data for the paper

> F. Buffoli, *Enclosed volume, not area, selects the apparent horizon: a variational principle and its Grover structure* (2026).

An apparent horizon can be found without an initial guess as the global minimizer of
`C_λ = −W + λ ∫ θ_(ℓ)² dA`, provided the weight `W` prefers the outermost MOTS.
The paper proves that any weight monotone under enclosure (e.g. the enclosed coordinate
volume) does so for every λ, and shows with Brill–Lindquist data that the area
(the Bekenstein–Hawking choice) does not. It then derives the Grover structure of
one-hot QAOA on the resulting grid and counts the Toffoli gates of a coherent cost oracle.

Every number, table and figure in the paper is produced by the scripts in `code/`
from the data in `data/`. The figures are pgfplots/TikZ files written by `code/make_figs.py`.

## Layout

```
code/    Python scripts (reference MOTS finder, grids, quantum simulations, oracle count, figures)
data/    JSON outputs of the scripts, as used in the paper
paper/   LaTeX source (ahsel.tex, refs.bib, colors.tex, figs/*.tex) and compiled PDF
```

## Requirements

Python ≥ 3.10 with NumPy, SciPy and SymPy:

```
pip install -r requirements.txt
```

Run every script from inside `code/` (paths are relative to it), e.g.

```
cd code
python3 pairwise.py
```

## Script → result map

| Paper item | Script(s) | Output in `data/` |
|---|---|---|
| Brill–Lindquist MOTS branches, fold `d* = 1.5319`, near-fold area differences (Sec. III, Figs. 1–2) | `bl.py`, `bl_branch2.py`, `bl_inner2.py` | `bl_branches.json`, `bl_inner.json`, `bl_mots60.json` |
| Mode convergence (Table 2) | `convergence.py` | `convergence.json` |
| Newton initialization sensitivity (Sec. III) | `nr_basins.py` | `nr_basins.json` |
| Unequal masses, continuation in q (Fig. 3, Table 3) | `blq_core.py`, `blq_ray.py`, `blq_cont.py`, `blq_check.py` | `blq_ray_r0.7.json`, `blq_cont_dM0.73.json`, `blq_cont_dM0.75.json`, `blq_check.json` |
| Displacement of the minimizer, single BH and Gauss–Newton on BL (Table 1, Prop. 2) | `displacement.py`, `derive_H.py` (symbolic) | `displacement.json` |
| Reduced cost on (c0, c2) grids (Fig. 4) | `reduced.py`, `run_res.sh` | `reduced_d*_N*.json` |
| Volume vs area selection (Tables 4, 5) | `select.py`, `select_N.py` | `select_d*.json` |
| λ window over 1e-2…1e7 and thresholds λ* for both weights (Sec. IV.B–C, Table 5) | `lamscan.py` | `lamscan.json` |
| Three-block tensor, pairwise surrogates, refinement of their choices (Table 6) | `reduced3.py`, `pairwise.py`, `refine3.py` | `reduced3_d1.5_N8.json`, `pairwise.json`, `refine3.json` |
| Grover structure, exact reflection vs product mixer (Fig. 5) | `qsearch.py`, `q_experiments.py`, `grover_check.py` | `q_experiments.json`, `grover_check.json` |
| Mixer / softmax ablation (Fig. 6) | `xmixer.py` | `xmixer.json`, `xmixer_raw.json` |
| Expansion pretracking (Table 7) | `pretrack.py` | `pretrack.json` |
| Query count of Dürr–Høyer minimum finding (Sec. V.B, Tables 7–8) | `dh_queries.py` | `dh_queries.json` |
| Oracle operation inventory, Toffoli count, break-even sensitivity, runtime example (Tables 8, 9, Fig. 7) | `oracle_count.py`, `oracle_cost.py` | `oracle_count.json`, `oracle_cost.json` |
| All figures | `make_figs.py` | `paper/figs/*.tex` |

The remaining scripts (`bl_branch.py`, `bl_inner.py`, `blq.py`, `blq_arc.py`, `blq_inner.py`,
`blq_qcont.py`, `explore_grid.py`) are earlier exploration steps kept for completeness; so is
`data/blq_cont_dM0.7.json`, a third continuation branch (d/M = 0.70) that stops at q = 0.969 and is not used in the paper.

The hardware runs of Appendix C (IQM Garnet and Emerald) are documented by their job identifiers in the paper;
no hardware data or code are part of this repository.

Runtimes on a laptop: `reduced.py` with N = 16 about 30 s; `reduced3.py` with N = 8 a few minutes;
`nr_basins.py` about 5 min; `lamscan.py` a few minutes; the unequal-mass continuations several minutes each;
everything else, including `dh_queries.py`, seconds to a minute.

## Units and conventions

Brill–Lindquist data with bare masses `m1 = m2 = 1` (total `M = 2`), punctures at `z = ±d/2`.
In the grid tests the area weight is normalized as `A/(16π)`, the squared irreducible mass,
which equals `W = A/4` with λ rescaled by 4π.

## Building the paper

```
cd paper
latexmk -pdf ahsel.tex
```

## License

Code: MIT (see `LICENSE`). Paper text and figures: © the author.

## Citation

See `CITATION.cff`.
