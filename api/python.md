# Python

Native PyO3 bindings over the Rust core. Construct a `Shazam` from a JSON spec, then
drive it with `command(json) -> json` — `index` first, then `match`.

```bash
pip install wickra-shazam
```

```python
import json
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
    print(m["ts"], m["similarity"])
```

## More

- [PyPI](https://pypi.org/project/wickra-shazam/)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/python)
