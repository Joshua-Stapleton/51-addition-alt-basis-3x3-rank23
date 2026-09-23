# A 51-Addition Alternative-Basis Kernel for Rank-23 3x3 Matrix Multiplication

This repository provides a machine-checkable certificate for a rank-23 bilinear algorithm that multiplies two 3x3 matrices over the reals using
51 kernel additions and 5 additional additions for the change of basis, for a total of 56 additions.

The certificate is a single JSON file, `3x3x3_m23_cr51_alt_basis_certificate.json`, which contains:

* `original_matrices` — the matrices `U`, `V`, `W` of the source factorization in the original basis;
* `changed_matrices` — the matrices `Uc`, `Vc`, `Wc` of the factorization in the new basis;
* `basis` — the change-of-basis matrices `P`, `Q`, `R` relating the two, i.e. `U = Uc @ P`, `V = Vc @ Q`, and `W = R @ Wc`,
* `slp` — the straight-line programs for the three stages of the algorithm.

The certificate is self-contained: no network access, no external data files, and no dependencies beyond Python and NumPy are required to verify it.

## Verification

Run these commands from the repository root:

```bash
pip install -r requirements.txt
python3 verify_certificate.py
```

### What the verification script does

`verify_certificate.py` loads the certificate and performs three independent checks.

1. **Basis consistency**. It verifies that the stored change-of-basis matrices satisfy `U = Uc @ P`, `V = Vc @ Q`, and `W = R @ Wc`. This
    confirms that the original and changed factorizations describe the same bilinear algorithm.
2. **Brent equations**. It reconstructs the matrix-multiplication tensor and checks that it equals `einsum("ri,rj,kr->ijk", U, V, W)`. This
    confirms that the factorization really computes 3x3 matrix multiplication and is not merely a formal identity.
3. **Randomized evaluation**. It draws 1000 random pairs of 3x3 matrices a, b and checks that both the full scheme in the new basis and the
    explicit 51-addition kernel `multiply3x3_cr51` reproduce `c = a @ b` to within floating-point tolerance.

If all checks pass, the script prints `All tests passed`.

The addition counts quoted in the paper are reflected directly in the source of `multiply3x3_cr51`, where the u, v, and z stages are
annotated as requiring `13`, `12`, and `26` additions respectively.


## Citing

If you use this certificate or the accompanying algorithm in your work, please cite the paper:

```bibtex
@article{stapleton2026a,
    title={A 51-Addition Alternative-Basis Kernel for Rank-23 3x3 Matrix Multiplication},
    author={Stapleton, Joshua and Perminov, Andrew},
    journal={arXiv preprint arXiv:TODO},
    url={https://arxiv.org/abs/TODO},
    year={2026}
}
```
