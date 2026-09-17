---
title: Benchmarks
description: "Shazam's cost splits in two: indexing — folding an asset's whole history into a rolling fingerprint at every bar — and matching — computing one current fingerprint and finding…"
---

# Benchmarks

::: tip Looking for the indicator library's numbers?
This page is about Wickra Shazam. Wickra's own indicator benchmarks — the
comparison against TA-Lib, talipp, pandas-ta and the other Rust TA crates —
live at [wickra.org](https://wickra.org/benchmarks).
:::

Shazam's cost splits in two: **indexing** — folding an asset's whole history into
a rolling fingerprint at every bar — and **matching** — computing one current
fingerprint and finding its `k` nearest historical neighbours under the chosen
metric. The benchmarks here measure that **core index + match work**, so
throughput scales predictably with the history length, the fingerprint dimension
and the metric.

## What is measured

The `shazam-bench` crate (criterion) covers:

- **index** — building the `FingerprintIndex` over a history of N bars.
- **match** — one `match_current` over an index of N fingerprints for a fixed `k`.
- **Fingerprint dimension** — specs of a few different dimensions `N`.
- **Execution path** — the default rayon `parallel` feature vs
  `--no-default-features` (the sequential / WASM path), which must produce a
  byte-identical report.

## Methodology

Run against fixed, in-process synthetic histories so the numbers are reproducible
and contain no I/O variance:

```bash
cargo bench -p shazam-bench
```

## Results

Median criterion estimates on a single developer machine, default `parallel`
(rayon) path. Treat them as orders of magnitude, not guarantees — they vary with
CPU and toolchain. The nightly `bench.yml` workflow reruns the suite on a clean
runner for tracking over time.

### Index — building the `FingerprintIndex`

| History (bars) | Features | Median |
|---------------:|---------:|-------:|
| 1,000 | 5 | 869 µs |
| 1,000 | 20 | 4.36 ms |
| 10,000 | 5 | 9.42 ms |
| 10,000 | 20 | 49.4 ms |
| 100,000 | 5 | 158 ms |
| 100,000 | 20 | 723 ms |

Indexing scales roughly linearly in both the history length and the feature
count — a 100k-bar, 5-feature history folds in ~160 ms.

### Match — one `match_current` for `k` neighbours

| Index (fingerprints) | Metric | Median |
|---------------------:|--------|-------:|
| 10,000 | cosine | 557 µs |
| 10,000 | euclid | 564 µs |
| 10,000 | dtw | 3.50 ms |
| 100,000 | cosine | 6.49 ms |
| 100,000 | euclid | 6.92 ms |
| 100,000 | dtw | 43.8 ms |

`cosine` and `euclid` cost about the same; `dtw` is ~6× heavier because it aligns
per-bar sub-sequences rather than comparing flat vectors. Matching a query against
a 100k-fingerprint index is single-digit milliseconds for the flat metrics.

## Caveats

These figures bound shazam's own index + match overhead only. End-to-end time in
a real run also depends on loading the history from disk or a live feed, which
these in-process benchmarks do not capture.

The numbers above are the ones in the repository's [`BENCHMARKS.md`](https://github.com/wickra-lib/wickra-shazam/blob/main/BENCHMARKS.md), measured with the commands it names.
