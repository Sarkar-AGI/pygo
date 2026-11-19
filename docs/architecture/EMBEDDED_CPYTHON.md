# Embedded CPython

PyGo's Python integration is designed around **embedded CPython**.

## Why embedding?

A sidecar design would require Go to communicate with a separate Python process. PyGo instead targets:

```text
Go process
├── Go runtime
├── PyGo native runtime
├── CPython interpreter
└── pybind11 module bindings
```

This makes Python an in-process ecosystem layer rather than a deployment dependency that must be separately scheduled beside every Go application instance.

## Lifecycle

The native layer must:

1. initialize the interpreter before Python APIs are used;
2. register statically linked native Python modules before interpreter startup when required;
3. serialize access according to the CPython runtime rules;
4. translate Python exceptions into stable native/Go errors;
5. release Python references correctly; and
6. finalize the interpreter only when no Python work remains.

Repeated initialization/finalization must be explicitly synchronized. A runtime must not report successful initialization after it has already entered an invalid finalized state.

## pybind11 boundary

The pybind11 layer exposes selected C++ functionality to Python. It should not become a second tensor implementation. Native tensor storage remains authoritative.

## Deployment

The desired production topology is:

```text
┌─────────────────────────────────────┐
│              Go process             │
│                                     │
│  Go API → C ABI → CPython →         │
│             pybind11 → C++ → CUDA   │
│                                     │
└─────────────────────────────────────┘
```

No Python network sidecar is required by this architecture.
