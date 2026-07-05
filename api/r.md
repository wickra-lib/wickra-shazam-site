# R

A `.Call` binding over the C ABI. Construct a shazam from a JSON spec, then drive it
with `wkshzm_command(shazam, json) -> json` — `index` first, then `match`.

```r
install.packages("wickrashazam", repos = "https://wickra-lib.r-universe.dev")
```

```r
library(wickrashazam)

spec <- '{"features":[{"kind":"price","field":"close"}],
  "window":10,"metric":"euclid"}'

shazam <- wkshzm_new(spec)
wkshzm_command(shazam, '{"cmd":"index","history":[]}')
response <- wkshzm_command(shazam, '{"cmd":"match","current":[],"k":5}')
cat("wickra-shazam", wkshzm_version(), "\n")
cat(response, "\n")
```

## More

- [r-universe](https://wickra-lib.r-universe.dev)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/r)
