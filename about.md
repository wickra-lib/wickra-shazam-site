# About Wickra Shazam

Wickra Shazam matches an asset's **current** microstructure fingerprint against its
**entire history**. It turns a whole history into a rolling index of fixed-dimension
fingerprints — vectors built from the full Wickra feature space — then finds the past
setups that look most like right now, so *"that's the May-2021 crash setup"* becomes a
ranked, reproducible answer instead of a hunch.

## What makes it different

- **The full feature space, not price alone.** A fingerprint is a fixed-dimension
  vector over indicators, price and microstructure — order-book imbalance, funding,
  open interest, liquidations, footprint — so a match reflects the whole regime.
- **The fingerprint is data, not code.** A `FingerprintSpec` is an ordered feature
  list plus a `window`, a `normalize` mode and a `metric` (cosine, Euclidean or DTW).
  Because it is data, the exact same spec crosses the C ABI and WASM unchanged.
- **Deterministic core.** Indexing and matching have a fixed dimension `N` and are
  byte-identical across all ten bindings and between the parallel (rayon) and
  sequential (WASM) builds.
- **Three operations, one core.** `index(history, spec)` builds the rolling index,
  `match_current(index, current, k)` finds the `k` most similar historical
  fingerprints, and a label attaches a human name (`"may_2021_crash"`) to a match.

## Why it exists

Traders recognise regimes by eye — "this looks like the run-up before the last
flush". Wickra Shazam makes that recognition explicit and reproducible: it defines
the fingerprint surface once, in Rust, and exposes it as a JSON-over-C-ABI data API to
Rust, Python, Node.js, WASM and — over a C ABI — C, C++, C#, Go, Java and R, so the
same match is available in every language and byte-for-byte identical.

## Open source

Released under the **MIT OR Apache-2.0** license — permissive, OSI-approved, free for
any use including commercial. Source, issues and releases on
[GitHub](https://github.com/wickra-lib/wickra-shazam).

## Disclaimer

Wickra Shazam is a software library, **not** a trading system, and is provided
**as-is with no warranty**. It matches fingerprints and names regimes; it does not
give financial advice. Use it at your own risk.
