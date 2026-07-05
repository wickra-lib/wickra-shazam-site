# Rust

The native crate. Build a rolling index over a history with `build_index`, then find
the most similar historical windows with `match_index`.

```bash
cargo add wickra-shazam
```

```rust
use shazam_core::{build_index, match_index, Candle, FingerprintSpec};

const SPEC: &str = r#"{
    "features": [{"kind": "indicator", "name": "Rsi", "params": [14]},
                 {"kind": "price", "field": "close"}],
    "window": 10,
    "normalize": "z_score",
    "metric": "cosine"
}"#;

let spec = FingerprintSpec::from_json(SPEC).expect("valid spec");
let index = build_index(&history, &spec).expect("index");
let report = match_index(&index, &current, 5).expect("match");

for m in &report.matches {
    println!("ts {} similarity {}", m.ts, m.similarity);
}
```

## More

- [crates.io/crates/wickra-shazam](https://crates.io/crates/wickra-shazam) · [docs.rs](https://docs.rs/wickra-shazam)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/rust)
- [Fingerprints & spec](https://github.com/wickra-lib/wickra-shazam/tree/main/docs)
