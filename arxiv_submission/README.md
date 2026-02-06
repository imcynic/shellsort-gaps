# Supplementary Materials

## An Improved Gap Sequence for Shellsort via Evolutionary Optimization

### Contents

```
arxiv_submission/
├── main.tex              # LaTeX source for the paper
├── main.pdf              # Compiled paper (generate with pdflatex)
├── README.md             # This file
├── code/
│   ├── shellsort.c       # Shellsort implementation with comparison counting
│   ├── shellsort.h       # Header file
│   ├── gaps_baselines.h  # All gap sequence definitions (including evolved)
│   ├── rng.h             # xoshiro256** random number generator
│   ├── permgen.c         # Permutation generator
│   ├── validate.c        # Validation tool
│   ├── full_bench.c      # Comprehensive benchmark with statistics
│   └── evolve_live.c     # Evolutionary search algorithm
└── data/
    └── checksums.txt     # MD5 checksums for permutation files
```

### Building

```bash
# Compile all tools
cd code
cc -O3 -march=native -fopenmp -std=c11 -o permgen permgen.c -lm
cc -O3 -march=native -fopenmp -std=c11 -o validate validate.c shellsort.c -lm
cc -O3 -march=native -fopenmp -std=c11 -o full_bench full_bench.c shellsort.c -lm
cc -O3 -march=native -fopenmp -std=c11 -o evolve_live evolve_live.c shellsort.c -lm
```

### Reproducing Results

#### Step 1: Generate Permutations

```bash
mkdir -p results/perms
./permgen --out results/perms --seed 0xC0FFEE1234 \
  --sizes 1000000,2000000,4000000,8000000 \
  --trials 100,50,25,10
```

Verify checksums match those in `data/checksums.txt`.

#### Step 2: Run Validation

```bash
./validate results/perms 16
```

Expected output:
```
Validating Evolved Sequence on All Sizes (Full Trials)
=======================================================

N            | Ciura            | Evolved          | Diff %
-------------|------------------|------------------|------------
1000000      |      31944358.18 |      31825783.55 |   +0.3712%
2000000      |      67831818.98 |      67563913.46 |   +0.3950%
4000000      |     143478037.36 |     142782469.08 |   +0.4848%
8000000      |     302706308.80 |     300974436.40 |   +0.5721%
-------------|------------------|------------------|------------
TOTAL        |     545960523.32 |     543146602.49 |   +0.5154%

*** Evolved sequence is 0.5154% BETTER on holdout sizes ***
```

#### Step 3: Run Full Statistical Benchmark

```bash
./full_bench results/perms 16
```

This outputs detailed statistics including standard deviations, confidence intervals, and paired t-test results.

### The Evolved Sequence

```c
static const uint64_t EVOLVED_GAPS[] = {
    1, 4, 10, 23, 57, 132, 301, 701,
    1577, 3524, 7705, 17961, 40056, 94681,
    199137, 460316, 1035711, 3236462
};
```

For arrays larger than 8M elements, extend with the 2.25x rule:
- 3236462 × 2.25 = 7282039
- 7282039 × 2.25 = 16384588
- ...

### Compiling the Paper

```bash
pdflatex main.tex
bibtex main      # if using bibtex
pdflatex main.tex
pdflatex main.tex
```

### License

This code is released into the public domain for research purposes.

### Contact

[Author contact information]
