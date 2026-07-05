# Node.js

Native napi-rs bindings over the Rust core. Construct a `Shazam` from a JSON spec,
then drive it with `command(json) -> json` — `index` first, then `match`.

```bash
npm install wickra-shazam
```

```javascript
import { Shazam } from 'wickra-shazam'

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
for (const m of report.matches) console.log(m.ts, m.similarity)
```

## More

- [npm](https://www.npmjs.com/package/wickra-shazam)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/node)
