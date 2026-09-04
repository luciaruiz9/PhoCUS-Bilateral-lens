# PhoCUS Bilateral Acoustic Lens

k-Wave simulation pipeline for a bilateral holographic acoustic lens for the PhoCUS
ring-aperture transducer (MSc project, Imperial College London). Covers skull-derived
acoustic property mapping, virtual-source phase extraction, phase-to-thickness lens
design, and forward-propagation lens-verification, for both free-field and
transcranial (through-skull) conditions.

## Repository contents

```
notebooks/
  phocus_lens_simulation.ipynb        Main simulation pipeline (Methods Ch. 2):
                                       skull acquisition, hippocampal target
                                       localisation, virtual-source phase extraction,
                                       lens design, CAD/STL export, and
                                       forward-propagation lens-verification, for
                                       both free-field and transcranial media.
  phocus_transducer_validation.ipynb  Transducer/skull model validation against
                                       Murphy et al. (2022) (Methods Sec. 2.5,
                                       Results Sec. 3.1).

data/
  CT/       Skull CT volume (NRRD header only in this repo -- see "Getting the CT
            data" below for the raw payload).
  Targets/  Bilateral hippocampal target coordinates (fus_planner output CSVs).
```

Running `phocus_lens_simulation.ipynb` writes simulation outputs (`.h5`, figures,
STL) to the folder set by its `OUTPUT_ROOT` variable near the top of the notebook
-- update that path for your own setup before running. Those outputs are not part
of this repo.

## Getting the CT data

The `.raw` payload for `data/CT/DMBA_N20_230328-4-1_CT-cropped_M4D.nhdr` is not
included in this repository (249 MB, over GitHub's 100 MB per-file limit). Download
the Duke Mouse Brain Atlas dataset from:

> Methodfuel Inc. Duke CIVM ImageSpace.
> https://civmimagespace.civm.duhs.duke.edu/login.php/client/NA==, 2026. Duke.edu.
> Accessed: Aug. 17, 2026.

Download the `DMBA_N20_230328-4-1_CT-cropped_M4D` dataset's `.raw` file and place it
alongside the `.nhdr` header already in this repo, at:

```
data/CT/DMBA_N20_230328-4-1_CT-cropped_M4D.raw
```

## Requirements

Both notebooks use `k-wave-python`, `pynrrd`, `h5py`, `numpy`, `matplotlib`,
`plotly`, `pandas`, `scipy`, and `numpy-stl`. `phocus_lens_simulation.ipynb` runs
four full 3-D `kspaceFirstOrder3DC` simulations per execution and is
compute-intensive -- it was developed for Imperial College's HPC cluster.
