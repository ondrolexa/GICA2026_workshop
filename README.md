# Practical Thermodynamic Modelling for Petrology — GICA2026 Workshop

Materials for a 1-day, 3-module workshop given at GICA2026 by Ondrej Lexa
(Institute of Petrology and Structural Geology, Charles University, Prague).

**Core stack:** Python (`petropandas`, `pypsbuilder`), Julia (`MAGEMin`,
`MAGEMinApp`), Jupyter notebooks, THERMOCALC.

## Schedule

| Time | Session | Key topics & tools |
|---|---|---|
| 09:00–10:30 | Software installation & setup | Python env, `petropandas`, `pypsbuilder`, `MAGEMinApp` |
| 10:30–10:45 | Coffee break | |
| 10:45–12:30 | Module 1 — Data processing with `petropandas` | Data ingestion, petrological recalculations, plots |
| 12:30–13:30 | Lunch break | |
| 13:30–15:30 | Module 2 — P–T pseudosections via `pypsbuilder` | Automated THERMOCALC setup, `ptbuilder`, `psexplorer` visualization |
| 15:30–15:45 | Coffee break | |
| 15:45–16:30 | Module 3 — Fast minimization with MAGEMin | Multi-core Gibbs energy minimization, `MAGEMinApp` |
| 16:30–17:00 | Q&A & wrap-up | |

## Getting started

Requires Python ≥ 3.12 (this workshop targets 3.14) and
[`uv`](https://docs.astral.sh/uv/). From a clone or unzip of this
repository:

```bash
uv sync
uv run jupyter lab
```

This installs the exact `petropandas`/`pypsbuilder` versions the notebooks
and slides were built and tested against (pinned in `pyproject.toml` /
`uv.lock`). See [`install.md`](install.md) for full setup instructions,
including the automated THERMOCALC setup (`uv run tcinit`) needed for
Module 2, and the Julia/MAGEMinApp setup for Module 3.

## Repository layout

```
pyproject.toml / uv.lock   # shared uv project: petropandas[lab] + pypsbuilder[pyqt6]
install.md                 # full install guide (Python env, THERMOCALC, MAGEMinApp)
slides/                    # Beamer decks, one per session
  00_install.tex/.pdf         Setup + whole-day welcome/schedule
  01_petropandas.tex/.pdf     Module 1
  02_pypsbuilder.tex/.pdf     Module 2
  03_magemin.tex/.pdf         Module 3
notebooks/                 # hands-on companions, run in order
  01_intro_pandas.ipynb       plain-pandas primer
  02_petropandas.ipynb        petropandas accessors deep-dive
  03_psexplorer.ipynb         pypsbuilder.psexplorer deep-dive
data/avg_pelite/           # example dataset used by the notebooks' hands-on exercises
```

Run the three notebooks in order — Notebook 3's capstone P–T estimate reuses
results computed in Notebook 2.
