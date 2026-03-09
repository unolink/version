# version

Build-time version information for Go binaries. Zero dependencies.

Inject version, commit, date, and build number via `ldflags` at compile time. Get clean, human-readable or semver-compatible output with no boilerplate.

## Install

```bash
go get github.com/unolink/version
```

## Usage

### Inject at build time

```bash
go build -ldflags "\
  -X github.com/unolink/version.Version=1.2.0 \
  -X github.com/unolink/version.Commit=$(git rev-parse HEAD) \
  -X github.com/unolink/version.Date=$(date -u +%Y-%m-%dT%H:%M:%SZ) \
  -X github.com/unolink/version.Build=42"
```

### Read in your code

```go
package main

import (
    "fmt"

    "github.com/unolink/version"
)

func main() {
    fmt.Println(version.Human())     // 1.2.0-a1b2c3d (#42, 2026-03-09T12:00:00Z)
    fmt.Println(version.Technical()) // 1.2.0+42.20260309T12:00:00Z.a1b2c3d
}
```

## API

| Function | Output format | Example |
|----------|--------------|---------|
| `Human()` | `{version}-{short_commit} (#{build}, {date})` | `1.2.0-a1b2c3d (#42, 2026-03-09)` |
| `Technical()` | `{version}+{build}.{date}.{short_commit}` | `1.2.0+42.20260309.a1b2c3d` |

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `Version` | `"dev"` | Semantic version |
| `Commit` | `"none"` | Full git commit hash (truncated to 7 chars in output) |
| `Date` | `"unknown"` | Build date (RFC3339 recommended) |
| `Build` | `"0"` | CI build number |

## Why this package?

Every Go binary needs version info. The `ldflags` pattern is standard but repetitive. This package gives you two well-formatted output functions and sensible defaults with zero dependencies and zero setup.

## License

[MIT](LICENSE)
