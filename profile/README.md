<p align="center"><img src="https://raw.githubusercontent.com/go-net-health/brand/main/social/go-net-health.png" alt="go-net-health" width="640"></p>

<h1 align="center">go-net-health</h1>
<p align="center"><strong>An active network health-probe runner in pure Go — stateless TCP/HTTP checks, a rolling healthy/unhealthy verdict, no cgo.</strong></p>

<p align="center">
  🌐 <a href="https://go-net-health.github.io">Website</a> ·
  📚 <a href="https://go-net-health.github.io/docs/">Documentation</a>
</p>

<p align="center">
  <a href="https://go-net-health.github.io/docs/"><img alt="Docs" src="https://img.shields.io/badge/docs-mkdocs--material-0D9488?style=flat-square"></a>
  <a href="https://github.com/go-net-health/health/blob/main/LICENSE"><img alt="License: BSD-3-Clause" src="https://img.shields.io/badge/license-BSD--3--Clause-blue?style=flat-square"></a>
  <img alt="Go 1.26.4+" src="https://img.shields.io/badge/go-1.26.4%2B-00ADD8?style=flat-square&logo=go&logoColor=white">
  <img alt="Coverage 100%" src="https://img.shields.io/badge/coverage-100%25-1a7f37?style=flat-square">
</p>

---

go-net-health is an **active health-probe runner in pure Go (no cgo)**: stateless
network checks — HTTP or TCP — pointed at a single target address and folded into
a rolling `healthy` / `unhealthy` / `unknown` verdict.

A **`Probe`** runs one attempt of a configured check and reports pass/fail plus
latency. A **`Runner`** drives a `Probe` on a fixed period, honours an optional
initial delay, and derives the rolling `Status` by stacking consecutive
passes/failures against success/failure thresholds — a Kubernetes-style
liveness/readiness probe as a small, embeddable library.

It has **no opinion** on what to do with a verdict: `Healthy` vs `Unhealthy` is
just a fact your own state machine (a supervisor, a load-balancer, a respawn
loop) acts on.

> **It probes and reports; the caller decides.**

## Repositories

| Repo | What it is |
|------|------------|
| [**health**](https://github.com/go-net-health/health) | the probe runner: `Probe` (HTTP/TCP), `Runner`, `Status`, `Result`, `IsTimeout` |
| [**docs**](https://github.com/go-net-health/docs) | MkDocs Material documentation, versioned with [mike], served at [/docs/](https://go-net-health.github.io/docs/) |
| [**go-net-health.github.io**](https://github.com/go-net-health/go-net-health.github.io) | the Hugo landing page |
| [**brand**](https://github.com/go-net-health/brand) | logos and brand assets |

## Principles

- **Pure Go, zero cgo.** Depends only on the standard library (`net`,
  `net/http`, `context`, `time`); cross-compiles and embeds anywhere as a static
  binary.
- **Stateless probes, rolling verdict.** Each attempt is a fact; the `Runner`
  stacks them into `Healthy` / `Unhealthy` per explicit thresholds.
- **It probes; you decide.** No respawn, no routing, no side effects beyond the
  one network call — the verdict is yours to act on.
- **100% test coverage** is the target, enforced as a CI gate.

## Status

**Probe runner complete.** `TypeHTTP` (configurable path/method/timeout,
any-2xx-or-explicit status set, no redirect following) and `TypeTCP` (connect
check) probes, a periodic `Runner` with initial delay + success/failure
thresholds + a buffered `Result` stream, and `IsTimeout` to separate
"unreachable" from "refused". `TypeExec` is reserved. 100% coverage, `gofmt` +
`go vet` clean, CI green across the six 64-bit Go targets (amd64, arm64, riscv64,
loong64, ppc64le, s390x).

BSD-3-Clause.

[mike]: https://github.com/jimporter/mike
