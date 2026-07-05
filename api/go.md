# Go

A cgo wrapper over the C ABI. Construct a `Shazam` from a JSON spec, then drive it
with `Command(json) -> json` — `index` first, then `match`.

```bash
go get github.com/wickra-lib/wickra-shazam-go
```

```go
package main

import (
	"fmt"

	wickra "github.com/wickra-lib/wickra-shazam-go"
)

func main() {
	spec := `{"features":[{"kind":"price","field":"close"}],
	  "window":10,"metric":"euclid"}`

	shazam, err := wickra.New(spec)
	if err != nil {
		panic(err)
	}
	defer shazam.Close()

	if _, err := shazam.Command(`{"cmd":"index","history":[]}`); err != nil {
		panic(err)
	}
	report, err := shazam.Command(`{"cmd":"match","current":[],"k":5}`)
	if err != nil {
		panic(err)
	}
	fmt.Println("wickra-shazam", wickra.Version())
	fmt.Println(report)
}
```

## More

- [Go module (pkg.go.dev)](https://pkg.go.dev/github.com/wickra-lib/wickra-shazam-go)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/go)
