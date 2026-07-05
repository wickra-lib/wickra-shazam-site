# Java

An FFM (Panama) wrapper over the C ABI. Construct a `Shazam` from a JSON spec, then
drive it with `command(json) -> json` — `index` first, then `match`.

```xml
<!-- Maven Central -->
<dependency>
  <groupId>org.wickra</groupId>
  <artifactId>wickra-shazam</artifactId>
  <version>0.1.0</version>
</dependency>
```

```java
import org.wickra.shazam.Shazam;

public final class Match {
    public static void main(String[] args) {
        String spec = """
            {"features":[{"kind":"price","field":"close"}],
             "window":10,"metric":"euclid"}""";

        try (Shazam shazam = new Shazam(spec)) {
            shazam.command("{\"cmd\":\"index\",\"history\":[]}");
            String response = shazam.command("{\"cmd\":\"match\",\"current\":[],\"k\":5}");
            System.out.println("wickra-shazam " + Shazam.version());
            System.out.println(response);
        }
    }
}
```

The binding uses the Java Foreign Function & Memory API, so it needs JDK 22+ and
`--enable-native-access`.

## More

- [Maven Central](https://central.sonatype.com/artifact/org.wickra/wickra-shazam)
- [Source & examples](https://github.com/wickra-lib/wickra-shazam/tree/main/examples/java)
