# Installation Guide

**Practical Thermodynamic Modelling for Petrology** — GICA2026 workshop

This guide covers installing the three software stacks used in the workshop:
[Python environment: petropandas + pypsbuilder](#1-python-environment-petropandas--pypsbuilder)
(Modules 1–2), [THERMOCALC setup for pypsbuilder](#3-pypsbuilder--thermocalc)
(Module 2), and [MAGEMin / MAGEMinApp](#4-magemin--mageminapp) (Module 3).

> **Workshop day-of note:** per the workshop agenda, participants only need a
> laptop with an internet connection — all software below will be installed
> live during the session. This document is provided so you can prepare in
> advance if you prefer, or follow along at your own pace.

---

## 1. Python environment (petropandas + pypsbuilder)

Both `petropandas` (Module 1) and `pypsbuilder` (Module 2) install together
into **one shared project**, using [`uv`](https://docs.astral.sh/uv/).

**Requirements:** Python ≥ 3.12 (this workshop targets Python 3.14).

### Option A — from scratch

```bash
uv init --python 3.14 --bare workshop
cd workshop
uv add petropandas[lab] pypsbuilder[pyqt6]
```

### Option B — from downloaded workshop materials

Download the workshop repository as a zip and unzip it, or clone it. Open a
terminal in that folder, then:

```bash
uv sync
```

This reads the repository's own `pyproject.toml`/`uv.lock` and installs the
exact same pinned `petropandas`/`pypsbuilder` versions the workshop notebooks
were built and tested against — no separate `uv add` step needed.

### Running things

Either way, run everything through `uv run` so it uses this project's
environment, e.g.:

```bash
uv run python          # open the Python interpreter
uv run jupyter lab     # open the notebooks
uv run tcinit          # THERMOCALC setup, see section 3
uv run ptbuilder       # pypsbuilder's interactive GUI, see slides/02_pypsbuilder.tex
```

---

## 2. petropandas

Pandas-accessor library for EPMA mineral analyses and whole-rock geochemistry
— already installed as part of the shared environment above
(`petropandas[lab]`).

Source: <https://github.com/ondrolexa/petropandas>

---

## 3. pypsbuilder + THERMOCALC

PyQt front-end that drives THERMOCALC to construct and visualize P–T (and
T–X, P–X) pseudosections — already installed as part of the shared
environment above (`pypsbuilder[pyqt6]`).

### 3.1 Set up THERMOCALC

pypsbuilder is a front-end — it does not bundle THERMOCALC itself.

**Automated (recommended for the workshop):** run in workshop directory:

```bash
uv run tcinit
```

Enter `avgpelite` as project directory, `demo` as name of the project,
`4.9, 12.1, 595, 725` as p-T range and choose `metapelite set` as
THERMOCALC input set. This downloads latest THERMOCALC 3.51 for your platform
bundled with `metapelite` datasets/a-x files. Everything is written pre-tagged
and ready for `ptbuilder`/`txbuilder`/`pxbuilder`.

**Manual setup (fallback):** prepare a working directory containing:

- the THERMOCALC and `drawpd` executables
- a preferences file
- a thermodynamic dataset and an a-x file
- `calcmode 1` set, with no `ask` in scripts

Then insert the special tags pypsbuilder needs to manage starting guesses,
calculation, and bulk composition into the scriptfile:
`%{PSBGUESS-BEGIN/END}`, `%{PSBCALC-BEGIN/END}`, `%{PSBBULK-BEGIN/END}`
(THERMOCALC 3.5), or `%{PSBDOGMIN-...}`(THERMOCALC 3.4). Example scriptfiles
ship in `examples/avgpelite` (TC3.5) and `examples/avgpelite_34` (TC3.4) in
the pypsbuilder repository.

### 3.2 Upgrading

From within the project directory:

```bash
uv lock --upgrade-package pypsbuilder
uv sync
```

(swap `pypsbuilder` for `petropandas` to upgrade that package instead, or
drop `--upgrade-package <name>` to upgrade everything).

Source: <https://github.com/ondrolexa/pypsbuilder>

---

## 4. MAGEMin / MAGEMinApp

Parallel Gibbs-energy minimization engine for stable mineral assemblages.
This workshop uses **MAGEMinApp**, its browser-based GUI — no Julia
scripting required; MAGEMinApp installs the underlying `MAGEMin_C` engine
automatically as a dependency.

**Requirements:** a recent [Julia](https://julialang.org/downloads/) install.

### 4.1 Launch Julia with threads

```bash
julia -t auto      # use all available threads, or e.g. `julia -t 6`
```

### 4.2 Install and launch MAGEMinApp

In the Julia REPL:

```julia
using Pkg
Pkg.add("MAGEMinApp")

using MAGEMinApp
App()
```

Julia prints `[ Info: Listening on: 127.0.0.1:8050, thread id: 1` — open
that address in a web browser.

### 4.3 Updating

From the package manager (`]` prompt):

```julia
rm MAGEMinApp
rm MAGEMin_C
update
add MAGEMinApp
up MAGEMinApp
```

If an update fails, set the registry preference to `eager` before retrying,
or pin a version: `add MAGEMinApp@x.y.z`.

Sources:
<https://github.com/ComputationalThermodynamics/MAGEMin> ·
<https://github.com/ComputationalThermodynamics/MAGEMinApp.jl> ·
<https://computationalthermodynamics.github.io/MAGEMin_C.jl/dev/MAGEMinApp/installation>
