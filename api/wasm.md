# WASM

A `wasm-bindgen` build for the browser and other WebAssembly runtimes. The same
`Shazam` handle and JSON command protocol as the native bindings — the sequential
path that produces byte-identical output to the parallel build.

```bash
npm install wickra-shazam-wasm
```

```javascript
import init, { Shazam } from 'wickra-shazam-wasm'

await init()

const spec = JSON.stringify({
  features: [{ kind: 'price', field: 'close' }],
  window: 10,
  metric: 'euclid',
})

const shazam = new Shazam(spec)
shazam.command(JSON.stringify({ cmd: 'index', history }))
const report = JSON.parse(shazam.command(JSON.stringify({ cmd: 'match', current, k: 5 })))
for (const m of report.matches) console.log(m.ts, m.similarity)
```

## More

- [npm (wickra-shazam-wasm)](https://www.npmjs.com/package/wickra-shazam-wasm)
- [Source & bindings](https://github.com/wickra-lib/wickra-shazam/tree/main/bindings/wasm)
