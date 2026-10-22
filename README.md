# Awesome-Programming-Language-Runtimes

## Top Programming Language Runtimes Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on JIT Compilation, GIL Removal & Native Code Generation*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Programming Language Runtimes**. These are the execution engines that interpret, compile, and optimize code — from CPython's bytecode interpreter to PyPy's tracing JIT, GraalPy's polyglot VM, and Codon's ahead-of-time compiler. The runtime you choose shapes startup time, peak throughput, memory footprint, and whether your existing ecosystem of C extensions keeps working.

**Examples** include CPython, PyPy, GraalPy, Nuitka, Codon, Pyodide, PyScript, Node.js, Bun, and Deno (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, alternative execution models, and transparent performance characteristics — ideal for developers squeezing performance out of dynamic languages, researchers benchmarking runtime trade-offs, and teams deciding whether to migrate away from the reference implementation.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Python Cloud (Google Cloud Run / AWS Lambda)** — Managed compute environments running CPython with configurable versions. Usage-based pricing per request and compute time. The default deployment target for Python web applications and serverless functions.

- **JuliaHub** — Commercial platform for Julia with managed compute, package registry, and enterprise support. Hosted Jupyter environments for the Julia runtime.

- **RStudio Workbench (Posit)** — Enterprise R runtime environment with centralized server deployment, load balancing, high availability, SSO/Kerberos, and auditing/monitoring not available in the open-source edition.

## Open-Source GitHub Projects

- **[CPython](https://github.com/python/cpython)**  
  **The reference Python runtime** — PSF license, **65,000+ GitHub stars** . **The implementation everyone means when they say "Python"** — bytecode compilation followed by interpretation, with the most complete ecosystem of C extensions . **The GIL removal project** produces a build where threads run on separate cores while sharing memory, backed by the Python Software Foundation with a goal of making it default in a future version [citation:2]. **The experimental JIT** in 3.13 and 3.14 delivers **20-30% gains on CPU-bound tasks** — modest compared to PyPy, but the foundation for future optimization [citation:2]. **Faster CPython** delivered roughly **25% improvement in 3.11**, tapering to **3-5% in subsequent versions** [citation:9]. **The safest choice for compatibility** — every package targets it first, and every tutorial assumes it.

- **[PyPy](https://github.com/pypy/pypy)**  
  **The mature alternative Python runtime with a tracing JIT** — MIT licensed . **"PyPy adopts a series of optimizations to allow generally faster execution of Python code compared to the official implementation"** [citation:9]. **On math-heavy single-threaded benchmarks, PyPy runs in around 0.2 seconds where Python 3.14 takes about 9 seconds** — roughly 45x [citation:10]. **PyPy v7.3.20** supports **Python 2.7 and 3.11**, including stdlib for CPython 3.11.13 [citation:3]. **In controlled benchmarking, PyPy, Numba, and Codon showed the smallest average energy consumption** with a large effect size, while CPython and Pyston-lite had the highest [citation:1]. **Trade-offs**: **PyPy exhibits the highest average memory usage** among tested runtimes [citation:1], and **short-lived processes or C-extension-heavy code show little benefit** because JIT warmup dominates [citation:9]. **Best for long-running, pure-Python compute workloads** where the JIT has time to pay off.

- **[GraalPy](https://github.com/oracle/graalpython)**  
  **Oracle's Python 3.12-compliant runtime on GraalVM** — available as a Native Standalone distribution for Oracle GraalVM and GraalVM Community Edition [citation:12]. **Polyglot execution** — runs Python on the same VM as JavaScript, Ruby, R, and other GraalVM languages. **JVM Standalone artifacts are no longer supported** since 25.0.3; only Native Standalone remains [citation:12]. **Native extensions run by default as native binaries with full system access**, requiring `allowNativeAccess` to be set explicitly for sandboxed contexts [citation:4]. **Best for polyglot applications** needing one runtime for multiple languages, or teams already invested in the GraalVM ecosystem.

- **[Nuitka](https://github.com/Nuitka/Nuitka)**  
  **The Python-to-C compiler** — Apache 2.0 licensed . **Compiles Python code to C, then to native machine code** — not a JIT, but ahead-of-time compilation that produces standalone executables. **In controlled benchmarking, Nuitka used the least average memory** among tested runtimes and showed a small positive effect size on memory usage [citation:1]. **Trade-offs**: **Nuitka has the highest LLC cache miss rate at 58.73%** — consistently above 50% in every benchmark — and in some cases compilation increased energy consumption rather than reducing it [citation:1]. **Nuitka Release 2.8** includes anti-bloat improvements for astropy, IPython, keras, transformers, and torch, plus a fix rejecting the broken CPython 3.13.4 Windows release [citation:5]. **Best for distributing standalone executables** and reducing memory footprint, not for raw speed.

- **[Codon](https://github.com/exaloop/codon)**  
  **The high-performance Python compiler with built-in NumPy support** — compiles to native machine code via LLVM with **10-100x typical speedups over vanilla Python on a single thread** [citation:6]. **In independent benchmarking, Codon 0.19 achieved 26x on N-body and 142x on spectral-norm** versus CPython 3.14 [citation:14]. **Codon showed the smallest average energy consumption** in controlled testing, with a large effect size [citation:1]. **Critical caveat**: **Codon is not a drop-in replacement for CPython** — it implements its own Python, `import json` doesn't work, `sys.version_info` doesn't exist, and standard library coverage is incomplete [citation:14]. **You're not speeding up Python — you're rewriting in a Python-flavored language** that can't import your existing code [citation:14]. **Best for performance-critical kernels** where you can rewrite the hot path in Codon's subset and call it via JIT decorator or Python extension backend.

- **[Pyodide](https://github.com/pyodide/pyodide)**  
  **Python compiled to WebAssembly for browser execution** — MPL-2.0 licensed . **The engine under everything** — PyScript, JupyterLite, and most browser Python tools run on Pyodide [citation:15]. **Full CPython runtime in the browser** with NumPy, Pandas, and scientific packages. **Startup time under 3 seconds** is acceptable for WebAssembly-based Python; **under 5 seconds tolerable for full environments** [citation:7]. **Numerical computing (NumPy) performs at 80-100% of native speed** in the browser [citation:7]. **Best for data science in the browser** where Pyodide's full package ecosystem justifies the WASM payload.

- **[PyScript](https://github.com/pyscript/pyscript)**  
  **The browser Python framework built on Pyodide** — Apache 2.0 licensed . **Entering maintenance mode** — API frozen, only Pyodide updates continue at a slower cadence [citation:15]. **"PyScript didn't stall because the tech failed. It's rock-solid. It stalled because not enough people were writing about it"** [citation:15]. **Keep using it if you already do**; no new features. **Best for existing PyScript deployments** — evaluate Pyodide directly or smaller wrappers for new projects.

- **[Node.js](https://github.com/nodejs/node)**  
  **The dominant JavaScript runtime** — MIT licensed . **In WebSocket server benchmarking, Node.js 26 with uWebSockets.js handled 49,000 round trips per second** — faster than Deno's 40,000 and Bun's 37,000 — with a 99th percentile of 24.8µs [citation:8]. **The most mature ecosystem** for server-side JavaScript. **Best for production JavaScript services** where ecosystem breadth and stability matter.

- **[Bun](https://github.com/oven-sh/bun)**  
  **The fast all-in-one JavaScript runtime** — MIT licensed . **Built-in server, bundler, test runner, and package manager** in one binary. **In WebSocket benchmarking, Bun 1.4.2 handled 37,000 round trips per second** — slower than Node.js with uWebSockets.js in this test, with a 99th percentile of 46.4µs [citation:8]. **Anthropic acquired Bun** in 2025. **Best for development speed and all-in-one tooling**, not necessarily peak production throughput.

- **[Deno](https://github.com/denoland/deno)**  
  **The secure JavaScript/TypeScript runtime** — MIT licensed . **In WebSocket benchmarking, Deno 2.9.7 handled 40,000 round trips per second** — between Node.js and Bun — with a 99th percentile of 32.0µs [citation:8]. **Cloudflare acquired Deno** in 2025. **Best for TypeScript-first projects** wanting built-in security and modern tooling.

### Additional Strong Open-Source Options

- **Pyston-lite** — Python JIT optimized for short-lived processes. **In controlled benchmarking, Pyston-lite had the highest average energy consumption** alongside CPython, suggesting compilation overhead dominates for short workloads [citation:1].
- **Numba** — JIT compiler for numerical Python. **Showed the smallest average energy consumption** alongside PyPy and Codon in controlled testing [citation:1].
- **Brython / Skulpt** — JavaScript-transpiled Python for browsers. **Near-instant load, no WASM**, but no NumPy/Pandas support [citation:15].
- **Transcrypt** — Pre-compiles Python to JavaScript. **Fastest execution among browser Python options**, but requires a build step [citation:15].
- **JupyterLite** — Jupyter in the browser powered by Pyodide.

**Frameworks for building custom systems**: Combine **CPython** as the compatibility baseline and default deployment target, **PyPy** for long-running pure-Python compute where JIT warmup pays off, **Codon** for rewriting performance-critical kernels that can live in a restricted subset, **Nuitka** for distributing standalone executables with minimal memory, **Pyodide** for browser-side data science, and **Node.js** for production JavaScript services with the most mature ecosystem — to build a runtime strategy tailored to your workload profile, compatibility requirements, and deployment constraints.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Programming language runtimes differ significantly in execution model, compatibility, and performance profile. **PyPy's JIT delivers dramatic speedups on long-running math-heavy workloads** — roughly 45x on some benchmarks versus CPython [citation:10] — but shows little benefit for short-lived processes or C-extension-heavy code [citation:9]. **Codon is not a drop-in CPython replacement** — it cannot import arbitrary Python modules and implements its own restricted subset [citation:14]. **Nuitka can increase energy consumption** in some cases despite reducing memory usage [citation:1]. **CPython's experimental JIT delivers 20-30% gains**, not the 10-100x of specialized compilers [citation:2]. **Browser Python (Pyodide) startup takes seconds**, not milliseconds — acceptable for data science, unacceptable for latency-sensitive interactions [citation:7]. **PyScript is in maintenance mode** — evaluate before adopting [citation:15]. **WebSocket benchmarks are single-core round-trip tests** — production throughput depends on your specific traffic patterns and server configuration [citation:8].
- Self-hosted open-source solutions require proper benchmarking against your specific workloads, dependency compatibility verification, and awareness that alternative runtimes may lag behind CPython's supported Python version.

---

**Made for developers, performance engineers, and teams choosing execution runtimes for production workloads.**
Let's make programming language runtimes more open, performant, and workload-appropriate.
# Awesome-Programming-Language-Runtimes

# Top Programming Language Runtimes Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on JIT Compilation, Adaptive Specialization & Native Code Generation*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Programming Language Runtimes**. These are the execution engines that interpret, compile, and optimize code — from CPython's adaptive specializing interpreter to PyPy's tracing JIT, GraalPy's polyglot VM, and Codon's ahead-of-time compiler. The runtime you choose shapes startup time, peak throughput, memory footprint, and whether your existing ecosystem of C extensions keeps working.

**Examples** include CPython, PyPy, GraalPy, Nuitka, Codon, Pyodide, PyScript, Node.js, Bun, and Deno (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, alternative execution models, and transparent performance characteristics — ideal for developers squeezing performance out of dynamic languages, researchers benchmarking runtime trade-offs, and teams deciding whether to migrate away from the reference implementation.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Python Cloud (Google Cloud Run / AWS Lambda)** — Managed compute environments running CPython with configurable versions. Usage-based pricing per request and compute time. The default deployment target for Python web applications and serverless functions.

- **JuliaHub** — Commercial platform for Julia with managed compute, package registry, and enterprise support. Hosted Jupyter environments for the Julia runtime.

- **RStudio Workbench (Posit)** — Enterprise R runtime environment with centralized server deployment, load balancing, high availability, SSO/Kerberos, and auditing/monitoring not available in the open-source edition.

## Open-Source GitHub Projects

- **[CPython](https://github.com/python/cpython)**  
  **The reference Python runtime** — PSF license, **65,000+ GitHub stars**. **The implementation everyone means when they say "Python"** — bytecode compilation followed by interpretation, with the most complete ecosystem of C extensions. **Python 3.11 introduced the adaptive specializing interpreter (PEP 659)**, making the runtime between **10% and 60% faster than Python 3.10** depending on the application, achieved portably by letting the interpreter adapt to the program being run and streamlining key data structures [citation:1]. **The experimental JIT (PEP 744)** in 3.13 translates hot Tier 1 bytecode into a Tier 2 intermediate representation (micro-ops) and then to machine code via copy-and-patch, with no runtime dependencies but a build-time LLVM dependency [citation:15]. **The JIT is disabled by default** and not recommended for production while experimental; Windows and macOS 3.14 builds include it but require `PYTHON_JIT=1` to enable [citation:4]. **Faster CPython** delivered roughly **25% improvement in 3.11**, tapering to smaller gains in subsequent versions. **The safest choice for compatibility** — every package targets it first, and every tutorial assumes it.

- **[PyPy](https://github.com/pypy/pypy)**  
  **The mature alternative Python runtime with a tracing JIT** — MIT licensed. **"PyPy adopts a series of optimizations to allow generally faster execution of Python code compared to the official implementation"**. **On math-heavy single-threaded benchmarks, PyPy runs dramatically faster than CPython** — roughly 45x in some comparisons. **PyPy v7.3.20** supports **Python 2.7 and 3.11**, including stdlib for CPython 3.11.13. **In controlled benchmarking, PyPy, Numba, and Codon showed the smallest average energy consumption** with a large effect size, while CPython and Pyston-lite had the highest. **Trade-offs**: **PyPy exhibits the highest average memory usage** among tested runtimes, and **short-lived processes or C-extension-heavy code show little benefit** because JIT warmup dominates. **Best for long-running, pure-Python compute workloads** where the JIT has time to pay off.

- **[GraalPy](https://github.com/oracle/graalpython)**  
  **Oracle's Python 3.12-compliant runtime on GraalVM** — available as a Native Standalone distribution for Oracle GraalVM and GraalVM Community Edition. **Polyglot execution** — runs Python on the same VM as JavaScript, Ruby, R, and other GraalVM languages. **JVM Standalone artifacts are no longer supported** since 25.0.3; only Native Standalone remains. **Native extensions run by default as native binaries with full system access**, requiring `allowNativeAccess` to be set explicitly for sandboxed contexts. **Best for polyglot applications** needing one runtime for multiple languages, or teams already invested in the GraalVM ecosystem.

- **[Nuitka](https://github.com/Nuitka/Nuitka)**  
  **The Python-to-C compiler** — Apache 2.0 licensed. **Compiles Python code to C, then to native machine code** — not a JIT, but ahead-of-time compilation that produces standalone executables. **In controlled benchmarking, Nuitka used the least average memory** among tested runtimes and showed a small positive effect size on memory usage. **Trade-offs**: **Nuitka has the highest LLC cache miss rate at 58.73%** — consistently above 50% in every benchmark — and in some cases compilation increased energy consumption rather than reducing it. **Nuitka Release 2.8** includes anti-bloat improvements for astropy, IPython, keras, transformers, and torch. **Best for distributing standalone executables** and reducing memory footprint, not for raw speed.

- **[Codon](https://github.com/exaloop/codon)**  
  **The high-performance Python compiler with built-in NumPy support** — compiles to native machine code via LLVM with **10-100x typical speedups over vanilla Python on a single thread**. **In independent benchmarking, Codon 0.19 achieved 26x on N-body and 142x on spectral-norm** versus CPython 3.14. **Codon showed the smallest average energy consumption** in controlled testing, with a large effect size. **Critical caveat**: **Codon is not a drop-in replacement for CPython** — it implements its own Python, `import json` doesn't work, `sys.version_info` doesn't exist, and standard library coverage is incomplete. **You're not speeding up Python — you're rewriting in a Python-flavored language** that can't import your existing code. **Best for performance-critical kernels** where you can rewrite the hot path in Codon's subset and call it via JIT decorator or Python extension backend.

- **[Pyodide](https://github.com/pyodide/pyodide)**  
  **Python compiled to WebAssembly for browser execution** — MPL-2.0 licensed. **The engine under everything** — PyScript, JupyterLite, and most browser Python tools run on Pyodide. **Full CPython runtime in the browser** with NumPy, Pandas, and scientific packages. **Startup time under 3 seconds** is acceptable for WebAssembly-based Python; **under 5 seconds tolerable for full environments**. **Numerical computing (NumPy) performs at 80-100% of native speed** in the browser. **Best for data science in the browser** where Pyodide's full package ecosystem justifies the WASM payload.

- **[PyScript](https://github.com/pyscript/pyscript)**  
  **The browser Python framework built on Pyodide** — Apache 2.0 licensed. **Entering maintenance mode** — API frozen, only Pyodide updates continue at a slower cadence. **"PyScript didn't stall because the tech failed. It's rock-solid. It stalled because not enough people were writing about it"**. **Keep using it if you already do**; no new features. **Best for existing PyScript deployments** — evaluate Pyodide directly or smaller wrappers for new projects.

- **[Node.js](https://github.com/nodejs/node)**  
  **The dominant JavaScript runtime** — MIT licensed. **In WebSocket server benchmarking, Node.js 26 with uWebSockets.js handled 49,000 round trips per second** — faster than Deno's 40,000 and Bun's 37,000 — with a 99th percentile of 24.8µs. **The most mature ecosystem** for server-side JavaScript. **Best for production JavaScript services** where ecosystem breadth and stability matter.

- **[Bun](https://github.com/oven-sh/bun)**  
  **The fast all-in-one JavaScript runtime** — MIT licensed. **Built-in server, bundler, test runner, and package manager** in one binary. **In WebSocket benchmarking, Bun 1.4.2 handled 37,000 round trips per second** — slower than Node.js with uWebSockets.js in this test, with a 99th percentile of 46.4µs. **Anthropic acquired Bun** in 2025. **Best for development speed and all-in-one tooling**, not necessarily peak production throughput.

- **[Deno](https://github.com/denoland/deno)**  
  **The secure JavaScript/TypeScript runtime** — MIT licensed. **In WebSocket benchmarking, Deno 2.9.7 handled 40,000 round trips per second** — between Node.js and Bun — with a 99th percentile of 32.0µs. **Cloudflare acquired Deno** in 2025. **Best for TypeScript-first projects** wanting built-in security and modern tooling.

### Additional Strong Open-Source Options

- **Pyston-lite** — Python JIT optimized for short-lived processes. **In controlled benchmarking, Pyston-lite had the highest average energy consumption** alongside CPython, suggesting compilation overhead dominates for short workloads.
- **Numba** — JIT compiler for numerical Python. **Showed the smallest average energy consumption** alongside PyPy and Codon in controlled testing.
- **Brython / Skulpt** — JavaScript-transpiled Python for browsers. **Near-instant load, no WASM**, but no NumPy/Pandas support.
- **Transcrypt** — Pre-compiles Python to JavaScript. **Fastest execution among browser Python options**, but requires a build step.
- **JupyterLite** — Jupyter in the browser powered by Pyodide.

**Frameworks for building custom systems**: Combine **CPython** as the compatibility baseline and default deployment target, **PyPy** for long-running pure-Python compute where JIT warmup pays off, **Codon** for rewriting performance-critical kernels that can live in a restricted subset, **Nuitka** for distributing standalone executables with minimal memory, **Pyodide** for browser-side data science, and **Node.js** for production JavaScript services with the most mature ecosystem — to build a runtime strategy tailored to your workload profile, compatibility requirements, and deployment constraints.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Programming language runtimes differ significantly in execution model, compatibility, and performance profile. **PyPy's JIT delivers dramatic speedups on long-running math-heavy workloads** — roughly 45x on some benchmarks versus CPython — but shows little benefit for short-lived processes or C-extension-heavy code. **Codon is not a drop-in CPython replacement** — it cannot import arbitrary Python modules and implements its own restricted subset. **Nuitka can increase energy consumption** in some cases despite reducing memory usage. **CPython's experimental JIT (PEP 744) is disabled by default and not recommended for production** while it remains experimental; Windows and macOS 3.14 builds require `PYTHON_JIT=1` to enable it [citation:4]. **Python 3.11's adaptive specializing interpreter delivered 10-60% speedups** portably by letting the interpreter adapt to the running program [citation:1]. **Browser Python (Pyodide) startup takes seconds**, not milliseconds — acceptable for data science, unacceptable for latency-sensitive interactions. **PyScript is in maintenance mode** — evaluate before adopting. **WebSocket benchmarks are single-core round-trip tests** — production throughput depends on your specific traffic patterns and server configuration.
- Self-hosted open-source solutions require proper benchmarking against your specific workloads, dependency compatibility verification, and awareness that alternative runtimes may lag behind CPython's supported Python version.

---

**Made for developers, performance engineers, and teams choosing execution runtimes for production workloads.**
Let's make programming language runtimes more open, performant, and workload-appropriate.
