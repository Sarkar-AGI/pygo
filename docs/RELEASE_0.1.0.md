# PyGo 0.1.0 Release Notes

## Overview

PyGo 0.1.0 establishes the Go-first architecture for a native deep-learning framework:

```text
Go API → CGo / C ABI → embedded CPython → pybind11 → C++ → CPU/AVX-512/CUDA
```

## Included

- Go-facing framework packages
- Python tensor/autograd/model ecosystem
- Native C++ runtime
- CUDA kernel sources and CUDA build workflow
- CGo and C ABI bridge layers
- pybind11 binding layer
- Documentation for architecture, installation and development
- GitHub Actions workflows for CPU CI, Go/Python integration, CUDA validation and tagged releases

## Validation policy

A source file being present does not by itself establish runtime support. The CI workflows therefore distinguish:

- CPU build/test validation on GitHub-hosted runners;
- Python package/test validation;
- Go tests after native-library installation;
- CUDA validation on explicitly configured NVIDIA self-hosted runners.

## Known release boundaries

- CUDA validation requires NVIDIA hardware and a compatible CUDA toolkit.
- AVX-512 behavior must be validated on an AVX-512 capable CPU before making hardware-specific performance claims.
- Embedded CPython/pybind11 requires the corresponding Python development and pybind11 build dependencies.
- TPU/XLA functionality is not claimed merely from API-level declarations.

## Version

`0.1.0`
