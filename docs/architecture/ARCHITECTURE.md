# PyGo Architecture

## Design goal

PyGo is organized as a Go-first deep-learning framework with an in-process Python ecosystem and a native C++ compute engine.

```text
Application
    │
    ▼
Go API
    │
    ▼
CGo / stable C ABI
    │
    ▼
Embedded CPython
    │
    ▼
pybind11
    │
    ▼
C++ runtime
    │
    ├── CPU / AVX-512
    └── CUDA
```

## Process model

The intended deployment model is a **single application process**. CPython is embedded in that process rather than being launched as a separate Python service for each application instance.

This removes a mandatory network/RPC boundary between Go and Python and allows the native runtime to remain the common execution layer.

## Responsibilities

### Go

Go is the public application language. It owns the ergonomic top-level API used by Go applications and coordinates training, inference, serving and runtime operations.

### CGo and the C ABI

The C ABI is the boundary used to keep Go/native calls explicit and stable. It should expose opaque handles and lifecycle operations rather than leaking C++ implementation types into Go.

### Embedded CPython

CPython provides the in-process Python runtime. It is initialized and finalized by the native runtime according to an explicit lifecycle contract.

### Python

Python contains the higher-level ecosystem: autograd integration, model definitions/model zoo, callbacks, data utilities and Python-side orchestration.

### pybind11

pybind11 maps selected native objects and functions into the embedded Python interpreter. It is an implementation boundary, not a second process boundary.

### C++ / CUDA / AVX-512

C++ owns the native tensor/runtime layer. Hardware-specific kernels are selected by the native dispatch layer. CUDA is used for NVIDIA GPUs; CPU kernels provide the baseline execution path, with AVX-512 used where available and implemented.

## Tensor ownership

A tensor should be represented by a native storage object plus metadata such as shape, strides, dtype and device.

Language handles refer to that native object. Reference counting or an equivalent ownership mechanism must keep storage alive until the last valid handle is released.

For external buffers, the owner must provide a lifetime contract. A raw pointer without a lifetime mechanism is not a safe zero-copy API.
