# 326 GB/s Token Processing Kernel (66% Efficiency)

**66% memory efficiency on RTX 2080 SUPER with real computation. Hardware-verified with Nsight Compute.**

## Results

| Metric | Value |
|--------|-------|
| GPU | NVIDIA GeForce RTX 2080 SUPER |
| Theoretical Peak | 496.1 GB/s |
| **Achieved** | **326 GB/s** |
| **Efficiency** | **66%** |
| Memory Throughput (Nsight) | 65.78% |
| Compute Throughput (Nsight) | 29.66% |

## Why This Matters

Most "high bandwidth" benchmarks copy memory and do nothing. This kernel performs **real computation per token**:

- Position-dependent hashing
- XOR accumulation
- **Integer modulo** (20-80 cycle latency)
- Float casting, scaling, FMA accumulation
- Tree reduction with synchronization
- 256 global atomics

Kernels with integer division typically achieve **25-45% efficiency**. This achieves **66%**.

## Verification

Results verified with Nsight Compute 2025.4.1.

![Nsight Compute Summary](nsight_summary.png)

Full profiling report with all graphs and analysis: **[nsight_details.pdf](nsight_details.pdf)**

See [`NSIGHT_RESULTS.md`](NSIGHT_RESULTS.md) for details.

## Technical Papers

The papers in this repository document the optimization journey:

1. **[Paper I](papers/iron_kernel_optimization_arxiv.pdf)** - Alignment fixes and hybrid parallelism (5 MB/s → 280 MB/s)
2. **[Paper II](papers/iron_kernel_ii_reduce_then_apply_arxiv.pdf)** - Reduce-then-apply algorithmic insight (280 MB/s → 326 GB/s)
3. **[Paper III](papers/iron_kernel_326gbs_arxiv.pdf)** - Hardware verification: 326 GB/s at 66% efficiency

## Configuration

- **Buffer**: 3 MB (768K tokens × 4 bytes)
- **Grid**: 256 blocks × 256 threads
- **Mean kernel time**: ~10.5 μs
- **CUDA**: 12.6+ / Driver 591.74+

---

Isaac Oravec | [GitHub](https://github.com/IsaacNudeton)

*Hardware-verified January 2026*
