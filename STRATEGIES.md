# Strategies for 1%+ Improvement Over Ciura

## The Challenge

Ciura's sequence has been empirically optimized and represents ~20 years of incremental improvement. Getting 0.25% took evolutionary search. Getting 1%+ requires fundamentally different approaches.

## Why 1% is Hard

1. **Smooth fitness landscape**: Small gap changes → small fitness changes
2. **Ciura is a local optimum**: Random mutations mostly make things worse
3. **Constraints limit exploration**: Must start with 1, strictly increasing
4. **Diminishing returns**: Each 0.1% improvement is harder than the last

## Strategies to Try

### 1. Size-Specific Sequences (Most Promising)

**Insight**: A single sequence can't be optimal for all sizes. Train separate sequences for different N ranges.

```
Small (N < 5K):     Optimize for N=1000, 2000, 3000
Medium (5K-100K):   Optimize for N=10000, 50000
Large (100K-1M):    Optimize for N=100000, 500000
Very Large (1M+):   Optimize for N=1000000, 2000000
```

**Implementation**: Add `--size-range` flag to focus evolution on specific sizes.

**Expected gain**: 0.5-1% per size range (but lose generalization)

### 2. Hybrid Sequences

**Insight**: Use different gap ratios in different regions.

```
Gaps 1-100:     Ratio ~2.2 (finer granularity)
Gaps 100-10K:   Ratio ~2.25 (Ciura-like)
Gaps 10K+:      Ratio ~2.3 (coarser is OK)
```

**Implementation**: Add multi-phase ratio sequences with optimized breakpoints.

### 3. Co-Evolution with Problem Structure

**Insight**: Gap sequences interact with permutation structure.

Try evolving on:
- Partially sorted inputs (10%, 50%, 90% sorted)
- Nearly-sorted with random swaps
- Specific patterns (reversed, organ-pipe, etc.)

Different sequences may be optimal for different input distributions.

### 4. Larger Search Space

**Current limitation**: We're searching near Ciura.

**Expand to**:
- Sequences with MORE gaps (20-25 instead of 16-17)
- Sequences with FEWER gaps (12-14)
- Non-integer-ratio sequences
- Prime-based sequences
- Sequences derived from mathematical constants

### 5. Simulated Annealing / Basin Hopping

**Problem**: Evolution gets stuck in local optima.

**Solution**:
- High-temperature phases that accept worse solutions
- Periodic random restarts from best-ever
- Basin hopping: local optimize → random jump → repeat

### 6. Mathematical Optimization

Instead of evolution, try:
- Gradient descent on continuous relaxation of gaps
- Bayesian optimization with Gaussian processes
- CMA-ES (Covariance Matrix Adaptation)

### 7. Theoretical Analysis

Study WHY certain gaps work:
- Number-theoretic properties (coprimality, divisibility)
- Relationship to h-sequences in mathematics
- Connection to insertion sort behavior

## Quick Wins to Try First

1. **Focus on large N only** (N=1M, 2M)
   - More room for improvement
   - Our evolved sequence already shows +0.25% there
   - Run: `--sizes 1000000,2000000`

2. **More gaps in mid-range**
   - Current: ~16 gaps
   - Try: 20+ gaps with mutations that favor insertion

3. **Longer evolution**
   - 200-500 generations
   - Larger population (100-150)
   - Lower mutation rate (0.15-0.2) for fine-tuning

4. **Different random seeds**
   - Run 10 parallel searches with different seeds
   - Take best result

## Realistic Expectations

| Strategy | Expected Improvement | Effort |
|----------|---------------------|--------|
| Size-specific sequences | 0.5-1.0% per range | Medium |
| Longer evolution | 0.1-0.3% | Low |
| Multi-seed parallel | 0.1-0.2% | Low |
| Hybrid ratios | 0.2-0.5% | Medium |
| New sequence families | Unknown (0-2%?) | High |
| Mathematical optimization | 0.3-0.8% | High |

## Command to Monitor

```bash
# Start evolution
./build/evolve_live --perms results/perms --out results/raw \
  --generations 200 --pop 80 --mutation 0.25 \
  --sizes 1000000,2000000 --threads 16 &

# Monitor live
watch -n2 cat results/status.txt
```

## The Nuclear Option

If 1% is truly required, consider:
- **Adaptive gap sequences**: Different gaps based on current pass statistics
- **Machine learning**: Train a model to predict optimal gaps for given N
- **Abandon Shellsort**: For 1% fewer comparisons, consider other algorithms

The truth is that Ciura's sequence is remarkably good. Getting 1% better may require abandoning the constraint of a fixed pre-computed gap sequence.
