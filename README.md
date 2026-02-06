# Improved Shellsort Gap Sequence via Evolutionary Optimization

[![Paper](https://img.shields.io/badge/Paper-PDF-red)](arxiv_submission/main.pdf)
[![Language](https://img.shields.io/badge/Language-C-blue)]()
[![License](https://img.shields.io/badge/License-MIT-green)]()

An empirically optimized gap sequence for Shellsort that achieves **0.52% fewer comparisons** than the widely-used Ciura sequence—the first published improvement in over 25 years.

## The Discovery

The evolved gap sequence:
```
[1, 4, 10, 23, 57, 132, 301, 701, 1577, 3524, 7705, 17961, 40056, 94681, 199137, 460316, 1035711, 3236462]
```

### Performance vs Ciura (1M-8M elements)

| Array Size | Improvement | p-value |
|------------|-------------|---------|
| 1,000,000  | +0.37%      | < 0.001 |
| 2,000,000  | +0.40%      | < 0.001 |
| 4,000,000  | +0.48%      | < 0.001 |
| 8,000,000  | +0.57%      | < 0.001 |

The improvement **scales with array size**, suggesting the sequence is better tuned for large-scale sorting.

## Why This Matters

Shellsort is one of the fundamental comparison-based sorting algorithms, taught in every CS curriculum. The Ciura sequence (2001) has been the gold standard for gap sequences for over two decades. This research demonstrates that evolutionary optimization can discover improvements where exhaustive search found none.

## Reproducibility

Full verification materials are included:

- **Pre-generated permutations** with cryptographic checksums
- **Exact random seeds** for all evolutionary runs
- **System configuration** details (AMD Ryzen 9 9950X3D, GCC 15.2, Linux 6.18)
- **Statistical analysis** with paired t-tests on identical permutations

### Quick Start

```bash
# Build the benchmark suite
cd src
gcc -O3 -march=native -fopenmp -o bench bench.c shellsort.c
gcc -O3 -march=native -o validate validate.c shellsort.c

# Validate results
./validate

# Run benchmarks
./bench
```

## Paper

The full paper is available in `arxiv_submission/main.pdf`. It includes:

- Complete methodology description
- Detailed statistical analysis
- Comparison against all known baselines (Ciura, Tokuda, Sedgewick, Lee-2021, Skean-2023)
- Convergence analysis across 4 independent evolutionary runs

## Project Structure

```
├── arxiv_submission/     # Complete paper package for arxiv
│   ├── main.tex         # LaTeX source
│   ├── main.pdf         # Compiled paper
│   ├── code/            # Minimal verification code
│   └── data/            # Benchmark outputs and checksums
├── src/                  # Source code
├── results/              # Full benchmark results
└── paper_results/        # Results used in paper
```

## Citation

If you use this gap sequence in your work:

```bibtex
@misc{banner2026shellsort,
  author = {Banner, Bryan},
  title = {An Improved Gap Sequence for Shellsort via Evolutionary Optimization},
  year = {2026},
  note = {Preprint}
}
```

## License

MIT License - See LICENSE file for details.

---

*This research was conducted in January 2026 using evolutionary optimization across 4 parallel runs with diverse random seeds, targeting simultaneous optimization across multiple array sizes.*
