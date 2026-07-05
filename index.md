---
layout: home
title: Wickra Shazam — match an asset's microstructure fingerprint against its history
titleTemplate: false

hero:
  name: "Wickra Shazam"
  text: "Match the moment against history."
  tagline: "Turn an asset's whole history into a rolling index of microstructure fingerprints, then match the current one against it — \"that's the May-2021 crash setup\". Pattern recognition over the full feature space, not price alone."
  image:
    src: /wickra-mark.svg
    alt: Wickra Shazam
  actions:
    - theme: brand
      text: View on GitHub
      link: https://github.com/wickra-lib/wickra-shazam
    - theme: alt
      text: Fingerprints & spec
      link: https://github.com/wickra-lib/wickra-shazam/tree/main/docs
    - theme: alt
      text: API
      link: /api/rust

features:
  - icon: 🧬
    title: The full feature space, not price alone
    details: "A fingerprint is a fixed-dimension vector over indicators, price and microstructure — order-book imbalance, funding, open interest, liquidations, footprint — so a match reflects the whole regime, not a single line."
  - icon: 📇
    title: Index the whole history
    details: "index(history, spec) folds a full history into a rolling index of fingerprints; match_current(index, current, k) returns the k most similar historical windows, each with a timestamp and a similarity score."
  - icon: 📐
    title: Three similarity metrics
    details: "Compare fingerprints under cosine, Euclidean or DTW similarity, with optional z-score or min-max normalization — chosen in the spec, not the code."
  - icon: 🏷️
    title: Named regimes
    details: "Attach a human label to a match — \"may_2021_crash\", \"pre_breakout\" — so recognition becomes a reproducible answer instead of a hunch."
  - icon: 🧩
    title: The fingerprint is data, not code
    details: A FingerprintSpec is an ordered feature list plus a window, a normalize mode and a metric. Because it is data, the exact same spec crosses the C ABI and WASM unchanged.
  - icon: 🌐
    title: 10 languages
    details: "The core is a JSON-over-C-ABI data API (Shazam::command) in Rust, Python, Node.js, WASM, C, C++, C#, Go, Java and R, plus a command-line reference consumer."
---

<script setup>
const installTabs = [
  { label: 'Python', lang: 'bash', code: 'pip install wickra-shazam' },
  { label: 'Node',   lang: 'bash', code: 'npm install wickra-shazam' },
  { label: 'Rust',   lang: 'bash', code: 'cargo add wickra-shazam' },
  { label: 'WASM',   lang: 'bash', code: 'npm install wickra-shazam-wasm' },
  { label: 'C',      lang: 'bash', code: '# prebuilt header + library from GitHub releases:\n# github.com/wickra-lib/wickra-shazam/releases' },
  { label: 'C#',     lang: 'bash', code: 'dotnet add package WickraShazam' },
  { label: 'Go',     lang: 'bash', code: 'go get github.com/wickra-lib/wickra-shazam-go' },
  { label: 'Java',   lang: 'xml',  code: '<!-- Maven Central -->\n<dependency>\n  <groupId>org.wickra</groupId>\n  <artifactId>wickra-shazam</artifactId>\n  <version>0.1.0</version>\n</dependency>' },
  { label: 'R',      lang: 'r',    code: 'install.packages("wickrashazam", repos = "https://wickra-lib.r-universe.dev")' },
]

const pyCode = `import json
from wickra_shazam import Shazam

spec = json.dumps({
    "features": [{"kind": "indicator", "name": "Rsi", "params": [14]},
                 {"kind": "price", "field": "close"}],
    "window": 10,
    "normalize": "z_score",
    "metric": "cosine",
})

shazam = Shazam(spec)
shazam.command(json.dumps({"cmd": "index", "history": history}))
report = json.loads(shazam.command(json.dumps({"cmd": "match", "current": current, "k": 5})))
for m in report["matches"]:
    print(m["ts"], m["similarity"])`

const nodeCode = `import { Shazam } from 'wickra-shazam'

const spec = JSON.stringify({
  features: [{ kind: 'indicator', name: 'Rsi', params: [14] },
             { kind: 'price', field: 'close' }],
  window: 10,
  normalize: 'z_score',
  metric: 'cosine',
})

const shazam = new Shazam(spec)
shazam.command(JSON.stringify({ cmd: 'index', history }))
const report = JSON.parse(shazam.command(JSON.stringify({ cmd: 'match', current, k: 5 })))
for (const m of report.matches) console.log(m.ts, m.similarity)`

const cliCode = `# Index a history and match the current state against it (deterministic, offline):
wickra-shazam --spec golden/specs/crash_setup.json \\
  --history golden/data/history/sym-01.csv --current golden/data/current/sym-01.csv

# Rank the k most similar historical setups as JSON:
wickra-shazam --spec golden/specs/price_euclid.json \\
  --history golden/data/history/sym-01.csv --k 5 --format json`

const snippetTabs = [
  { label: 'Python', lang: 'python',     code: pyCode },
  { label: 'Node',   lang: 'javascript', code: nodeCode },
  { label: 'CLI',    lang: 'bash',       code: cliCode },
]
</script>

## The fingerprint is JSON, not code

A `FingerprintSpec` names the ordered `features` to sample, the `window` length, an
optional `normalize` mode and the `metric` to compare under. Every fingerprint the
spec produces has the same fixed dimension `N`, which is what makes the index and its
matches deterministic.

```json
{
  "features": [
    { "kind": "indicator", "name": "Rsi", "params": [14] },
    { "kind": "price", "field": "close" },
    { "kind": "microstructure", "name": "Obv", "params": [] }
  ],
  "window": 10,
  "normalize": "z_score",
  "metric": "cosine"
}
```

Because the spec is data, the same fingerprint crosses the C ABI and WASM unchanged,
and the same match comes back byte-for-byte in every language.

## Install

The same matcher from every language — native Rust, Python, Node.js and WASM, plus a
C ABI for C, C++, C#, Go, Java and R.

<InstallTabs :tabs="installTabs" />

## Index once, match against the whole history

Build the rolling index from a history, then match the current window against it. The
`command` API returns the same bytes in every binding.

<InstallTabs :tabs="snippetTabs" />

## Built on the Wickra core

Wickra Shazam is part of the [Wickra](https://wickra.org) ecosystem. Its fingerprints
are drawn from the same typed feature space that
[`wickra-core`](https://github.com/wickra-lib/wickra) computes, so a match reasons over
exactly the numbers a backtest or a live chart would see.

> Wickra Shazam is a software library, not a trading system, and gives no financial
> advice — use at your own risk.
