# entroptics-frb

An untuned rank-selection and reconstruction procedure, applied to the public CHIME/FRB Catalog 1
waterfalls and compared against CHIME's own *fitburst* forward model. No constant is fitted to the
data, no burst template enters, and nothing is set per event.

This repository holds the paper and everything needed to regenerate every number in it. The
procedure is the [`entroptics`](https://github.com/Agience/entroptics) library's public interface,
installed from PyPI; nothing here reimplements it.

| file | what it is |
|---|---|
| [`PAPER.md`](PAPER.md) | the paper |
| `reproduce.py` | regenerates every table from the waterfalls |
| `verify.py` | checks every number in the paper against the tables |
| `figures/frb_panel.py` | Figure 1 and the per-burst reads (`frb_panel.png`, `frb_panel.csv`, `frb_display.csv`) |
| `figures/frb_spotcheck.py` | the 12-waterfall random draw (`frb_spotcheck.csv`) |
| `tables/events.csv` | per-burst reads for the four events (Table 1) |
| `tables/spotcheck.csv` | the random draw (§3.2) |
| `tables/agreement.csv` | every correlation, including both reference points (Tables 2 and 3) |

## Install

Python 3.10 or later.

```
git clone https://github.com/Agience/entroptics-frb
cd entroptics-frb
pip install -r requirements.txt
```

This installs `entroptics` 0.2.10 or later from PyPI, with numpy, h5py and matplotlib.

## The data

The waterfalls are a separate public download and are not in this repository:

> CHIME/FRB Catalog 1 waterfalls
> CHIME/FRB Collaboration 2021, ApJS 257, 59 (arXiv:2106.04352)
> Public release: CANFAR CISTI.CANFAR/21.0007, https://www.canfar.net/

The release is browsable at CANFAR under that identifier and can be pulled with the CADC client
tools (`pip install vos`, then `vcp`) or over HTTP from the archive's file listing. Download the
per-event waterfall tree.

No script here downloads it. The release is far larger than a reproduction script should pull
unasked, and the scripts write committed tables, so a partial download would overwrite a good table
with an empty one while still exiting zero. The scripts refuse, and say what is missing.

Name the tree in any one of three ways, most specific first:

```
# 1. an explicit argument, per run
python reproduce.py /path/to/chime-frb-catalog1

# 2. the environment variable, per run
FRB_WATERFALLS=/path/to/chime-frb-catalog1 python reproduce.py

# 3. per machine: copy the example and edit it (git-ignored)
cp research.local.env.example research.local.env
```

The root is the directory that contains the per-event directories, so that
`<root>/*/*_waterfall.h5` matches. The four events of Figure 1 are `FRB20190425A`, `FRB20190106B`,
`FRB20190227A` and `FRB20190323B`; check the printed table has four rows.

## Reproduce

```
python reproduce.py      # writes tables/ and the figure outputs
python verify.py         # checks the paper's prose against the tables
```

Every seed is fixed and the BLAS pool is pinned, so the tables are byte-reproducible across runs.
`verify.py` needs no data and runs against the committed tables.

## Relation to the instrument paper

The `entroptics` repository's [`research/PAPER.md`](https://github.com/Agience/entroptics/blob/main/research/PAPER.md) is the full reference for the construction: the
definitions, the proofs, the provenance of every constant, and the synthetic validation with a
planted ground truth. This paper carries a self-contained account of the read path in §2 and cites
the instrument paper for everything else.

## Licence

Apache-2.0 — see [`LICENSE`](LICENSE), [`NOTICE`](NOTICE) and [`PATENTS.md`](PATENTS.md). The
`entroptics` read path is patent pending; its Apache-2.0 patent grant covers its use here.
Patent inquiries: connect@ikailo.com. Everything else: connect@agience.ai.

## Citing

See [`CITATION.cff`](CITATION.cff). Please also cite the CHIME/FRB Catalog 1 release for the data.
