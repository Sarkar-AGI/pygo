# Quickstart

## Go-first usage

PyGo's public application model is Go-first. A typical application creates tensors through the Go API and invokes the native runtime through the bridge.

```go
package main

import (
    "fmt"
    "github.com/Sarkar-Labs/pygo/go/tensor"
)

func main() {
    x := tensor.Zeros([]int64{2, 3})
    fmt.Println(x)
}
```

The exact package APIs should be checked against the package documentation for the release being used.

## Python ecosystem

Python is available for autograd, models and higher-level ecosystem components. In the intended architecture, Python runs inside the Go process when embedded runtime support is enabled.

## Native runtime

The C++ runtime is built by CMake and is the authoritative execution layer. Go and Python should not independently allocate competing tensor representations when a shared native tensor is available.
