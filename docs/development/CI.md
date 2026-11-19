# Continuous Integration

PyGo uses separate workflows for different hardware and language boundaries.

## CPU CI

`.github/workflows/ci.yml` checks:

1. native CMake configuration/build;
2. native CTest tests;
3. Python installation, compilation and tests;
4. Python wheel creation; and
5. Go tests after installing the native runtime.

## CUDA CI

`.github/workflows/cuda.yml` is intentionally restricted to a self-hosted NVIDIA runner. It validates CUDA translation units with `nvcc`, builds the CUDA runtime and runs native tests.

## Release CI

`.github/workflows/release.yml` runs the CPU release test suite for tags matching `v*`, builds the Python wheel, installs the native library and creates a source archive before publishing the GitHub release.
