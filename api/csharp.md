# C#

Idiomatic .NET over the C ABI. Construct a `Shazam` from a JSON spec, then drive it
with `Command(json) -> json` — `index` first, then `match`.

```bash
dotnet add package WickraShazam
```

```csharp
using Wickra.Shazam;

var spec = """
{"features":[{"kind":"price","field":"close"}],
 "window":10,"metric":"euclid"}
""";

using var shazam = new Shazam(spec);
shazam.Command("""{"cmd":"index","history":[ ]}""");
var response = shazam.Command("""{"cmd":"match","current":[ ],"k":5}""");
Console.WriteLine(response);
```

Targets .NET 8.

## More

- [NuGet](https://www.nuget.org/packages/WickraShazam)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/csharp)
