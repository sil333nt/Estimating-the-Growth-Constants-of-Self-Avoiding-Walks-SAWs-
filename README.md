# Estimating the Connective Constant of the Square Lattice

## Overview

This project investigates the **connective constant of the square lattice** by estimating the exponential growth rate of self-avoiding walks (SAWs) using various deterministic and Monte Carlo methods.

A self-avoiding walk is a path on a lattice that never revisits a previously visited site. The number of self-avoiding walks of length \(L\), denoted by \(C_L\), grows approximately exponentially:

\[
C_L \approx \mu^L
\]

where \(\mu\) is the connective constant. In simple terms, \mu is the average number of directions a SAW of any length can take.

The objective of this project is to explore different computational approaches to estimating \(\mu\), examine their accuracy and computational limitations, and investigate how the estimates behave as walk length increases.

## Methods

The project explores five approaches to estimating the number of self-avoiding walks and their growth constant.

### 1. Exact Enumeration (Recursive Backtracking)

Implemented a recursive backtracking algorithm to exhaustively enumerate all possible self-avoiding walks on a 2D square lattice.

- Explored all four possible movement directions at each step.
- Maintained a set of visited coordinates to prevent self-intersections.
- Computed exact SAW counts for small walk lengths.
- Used exact counts as ground truth for validating Monte Carlo estimates.

### 2. Basic Monte Carlo Simulation

Generated random walks of a fixed length and checked whether they were self-avoiding.

- Simulated a large number of random walks.
- Calculated the proportion of walks that satisfied the self-avoidance condition.
- Investigated how the probability of obtaining a valid SAW decreases as walk length increases.

This approach illustrates the inefficiency of rejection sampling for longer walks.

### 3. Rosenbluth Method (Importance Sampling)

Implemented the Rosenbluth method to estimate the number of SAWs through sequential importance sampling.

- Constructed walks by randomly selecting among available non-intersecting directions.
- Assigned each walk a Rosenbluth weight based on the number of available directions at each step.
- Assigned zero weight to walks that became trapped before reaching the target length.
- Estimated \(C_L\) using the average importance weight.

Compared estimated SAW counts against exact values for smaller walk lengths and examined the resulting estimates of \(\mu\).

### 4. Concatenation Method

Implemented a method to estimate the number of longer SAWs by concatenating independently generated shorter SAWs.

- Generated pairs of SAWs of equal length.
- Translated and joined the second walk to the endpoint of the first.
- Checked whether the resulting concatenated walk remained self-avoiding.
- Estimated the probability of successful concatenation and used it to estimate the number of SAWs of twice the original length.

Repeated the process recursively to estimate \(C_L\) for increasingly large walk lengths.

### 5. Population Growth Method

Developed a recursive population-based approach to estimate the growth constant over longer walk lengths.

- Initialized a population of valid SAWs.
- Estimated the average number of valid extensions per walk at each iteration.
- Extended the walks and used resampling to maintain a fixed population of valid SAWs.
- Applied the pivot algorithm to duplicated walks to improve population diversity and reduce correlations.
- Tracked growth constant estimates across successive walk lengths.

Examined convergence using running averages and window-smoothed estimates.

## Results and Observations

The different methods demonstrate the trade-offs between computational efficiency, estimation accuracy, and scalability.

| Method | Main observation |
|---|---|
| Exact Enumeration | Provides exact counts but becomes computationally infeasible as walk length increases. |
| Basic Monte Carlo | Becomes inefficient for longer walks as the probability of generating a valid SAW decreases. |
| Rosenbluth Method | Produces estimates close to exact counts for smaller lengths, but variance increases at larger lengths. |
| Concatenation Method | Allows estimation of much longer walks, but successful concatenations become increasingly rare and estimation errors accumulate recursively. |
| Population Growth | Provides a way to track growth constant estimates over longer walks, with estimates becoming more stable as walk length increases. |

### Selected Numerical Results

**Rosenbluth Method**

| Walk length \(L\) | Actual \(C_L\) | Estimated \(C_L\) |
|---:|---:|---:|
| 5 | 284 | 284 |
| 10 | 44,100 | 44,086 |
| 15 | 6,416,596 | 6,417,844 |

**Concatenation Method**

| Walk length \(L\) | Estimated \(C_L\) |
|---:|---:|
| 20 | \(8.98 \times 10^8\) |
| 40 | \(3.18 \times 10^{17}\) |
| 80 | \(3.13 \times 10^{34}\) |
| 160 | \(2.33 \times 10^{68}\) |
| 320 | \(1.06 \times 10^{136}\) |
| 640 | \(1.74 \times 10^{271}\) |

**Population Growth Method**

The population growth method tracked estimates of the connective constant from walk length \(L=15\) to \(L=715\).

The running-average estimates gradually became more stable as walk length increased, while window-smoothed estimates exhibited more noticeable fluctuations.

## Key Takeaways

- **Sampling strategy matters:** Direct rejection sampling becomes increasingly inefficient as the probability of generating a valid SAW decreases.
- **Importance sampling improves efficiency:** The Rosenbluth method avoids rejecting every non-self-avoiding random walk, although its estimates can suffer from high variance.
- **Longer walks introduce new challenges:** The concatenation method extends estimation to much larger walk lengths but suffers from decreasing acceptance probabilities and accumulated estimation errors.
- **Population-based approaches offer an alternative:** Resampling and the pivot algorithm help maintain a population of valid walks while tracking the growth constant over increasing lengths.
- **There is a trade-off between accuracy and scalability:** No single approach is equally effective across all walk lengths.

## References

Madras, N. and Sokal, A. D. (1988). *The pivot algorithm: A highly efficient Monte Carlo method for the self-avoiding walk*. Journal of Statistical Physics, 50(1), 109–186.
