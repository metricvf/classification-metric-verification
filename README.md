# Formal Verification of Published Classification Metrics

This repository contains machine-verified mathematical proofs (Lean 4)
demonstrating that specific performance metrics reported in six published
academic papers are arithmetically impossible given the datasets used.

## What This Proves

Classification metrics (accuracy, precision, recall, F1) are computed as
ratios k/n where k = correctly classified samples and n = total test samples.
For a given n, only specific percentage values are possible.

Example: With 7 test samples, the only possible accuracy values are
0%, 14.29%, 28.57%, 42.86%, 57.14%, 71.43%, 85.71%, and 100%.
A claimed accuracy of 96.37% on 7 samples is arithmetically impossible.

We use number theory (GCD analysis) and the Lean 4 theorem prover to
formally verify that claimed metrics cannot arise from any classification
on the stated datasets. These proofs are machine-checked — if they
compile, they are correct by construction.

## Papers Analyzed

| Paper DOI | Dataset | Samples | Impossible Values |
|-----------|---------|---------|-------------------|
| 10.1109/ACCESS.2020.2986013 | CICIDS 2017 | 36 (Infiltration) | 1 value + 2 implausible |
| 10.1155/2022/7853604 | Cleveland (UCI) | 303 | 29/30 in comparison table |
| 10.3390/s22020476 | Cleveland (UCI) | 303 | 15/15 |
| 10.1016/j.asoc.2023.110292 | Wisconsin (UCI) | 198-699 | 3/3 |
| 10.1155/2023/8509433 | Breast cancer imaging | <780 | 10/10 |
| 10.3389/frai.2024.1394363 | CICIDS 2017 | Large | F1 inconsistency (18.44pp) |

All six papers share common authors from the same institution.

## How to Verify

1. Install Lean 4: `curl -sSf https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh | sh -s -- -y`
2. Clone this repo
3. Run `lake build`
4. If it compiles with zero errors, every proof is correct.

Build time: <2 seconds. Zero dependencies beyond Lean 4.

## Proof Files

| File | What it proves |
|------|---------------|
| `Basic.lean` | Infiltration 96.37% impossible for n≤9999; Bayes constraints on Bot/WebAttack |
| `Rebuttals.lean` | Pre-closes 10 possible author rebuttals (cross-validation, SMOTE, rounding, etc.) |
| `Sensors2022.lean` | All 15 Cleveland metrics impossible for n≤303 |
| `MRIPPER2022.lean` | 29/30 comparison values impossible — including baseline methods |
| `AppliedSoftComputing2023.lean` | 3 accuracy values impossible for respective datasets |
| `USENet2023.lean` | 10 values impossible; 98.31% recycled from different paper |
| `Frontiers2024.lean` | F1=92% inconsistent with P=62%, R=90.43% (actual F1=73.56%) |

## The Pizza Analogy

Imagine a pizza cut into 7 slices. Someone claims they ate 96.37% of it.
That's impossible — you can eat 0, 1, 2, 3, 4, 5, 6, or 7 slices (0% to 100%
in steps of 14.29%). There is no way to eat 96.37% of a 7-slice pizza.

These papers report "test scores" that cannot exist for the number of
test items. This is not a matter of methodology — it is arithmetic.

## Technical Note

The GCD-based impossibility proof works as follows:
- A metric value v% expressed as v/10000 requires k/n = v/10000
- This means 10000×k = v×n, so n must be divisible by 10000/gcd(v,10000)
- If the minimum required n exceeds the dataset size, the value is impossible

Lean's `omega` tactic verifies these arithmetic constraints exhaustively.

## Citation

If you use these proofs in your own work, please cite this repository.

## License

MIT License. These proofs are public domain mathematical facts.