# PhoCUS Bilateral Acoustic Lens

A ring-structured acoustic holographic lens for the PhoCUS transducer that splits its
single focused ultrasound beam into two spatially separated foci, one per
hippocampus, for bilateral transcranial focused ultrasound in freely moving mice,

without obstructing the transducer's central fibre-photometry cannula. Developed for
an MSc thesis at Imperial College London.

This repository holds the [k-Wave](http://www.k-wave.org/) simulation pipeline: skull
acquisition and acoustic-property modelling from CT, virtual-source phase extraction,
phase-to-thickness lens design, and forward-propagation verification of the resulting
bilateral field, for both free-field and transcranial conditions.

## Repository structure

```
notebooks/
├── phocus_lens_simulation.ipynb        Main simulation pipeline
└── phocus_transducer_validation.ipynb  Transducer/skull model validation

data/
├── CT/       Skull CT volume (NRRD header; raw payload downloaded separately, see below)
└── Targets/  Bilateral hippocampal target coordinates
```

### `phocus_lens_simulation.ipynb`

Runs the full bilateral-lens pipeline for both free-field and transcranial media in
one execution:

1. Skull acquisition, segmentation and acoustic-property assignment from CT
2. Hippocampal target localisation
3. Virtual-source time-reversal phase extraction
4. Phase-to-thickness lens design and CAD/STL export
5. Forward-propagation lens-verification simulation and focal-performance metrics

Each stage is its own notebook cell, cross-referenced to the corresponding section of
the thesis Methods chapter, and controlled by a handful of top-level
variables/environment variables (drive frequency, pressure, lens standoff,
phase-extraction method, ...) rather than hardcoded alternatives -- see the
"Configuration switches" table near the top of the notebook.

### `phocus_transducer_validation.ipynb`

Validates the k-Wave transducer/skull model against Murphy et al. (2022) by comparing
simulated free-field and transcranial focal metrics (focal depth, lateral/axial FWHM,
insertion loss) against their published measurements.

## Setup

```bash
pip install k-wave-python pynrrd h5py numpy matplotlib plotly pandas scipy numpy-stl
```

`phocus_lens_simulation.ipynb` runs four full 3-D `kspaceFirstOrder3DC` k-Wave
simulations per execution and is compute-intensive -- it was developed for execution
on Imperial College's HPC cluster.

## Getting the CT data

The `.raw` payload for `data/CT/DMBA_N20_230328-4-1_CT-cropped_M4D.nhdr` (249 MB) is
not included in this repository -- over GitHub's 100 MB per-file limit. Download the
Duke Mouse Brain Atlas dataset from:

> Methodfuel Inc. Duke CIVM ImageSpace.
> https://civmimagespace.civm.duhs.duke.edu/login.php/client/NA==, 2026. Duke.edu.
> Accessed: Aug. 17, 2026.

Download the `DMBA_N20_230328-4-1_CT-cropped_M4D` dataset's `.raw` file and place it
alongside the `.nhdr` header already in this repo:

```
data/CT/DMBA_N20_230328-4-1_CT-cropped_M4D.raw
```

## Output

Running `phocus_lens_simulation.ipynb` writes simulation outputs (`.h5` pressure
fields, figures, lens STL) to the folder set by its `OUTPUT_ROOT` variable near the
top of the notebook -- update that path for your own setup before running. Outputs
are not tracked in this repo.

## Reference

Murphy et al. (2022), *PNAS* -- source of the PhoCUS transducer geometry/drive
parameters and the free-field/transcranial focal-metric benchmark used in
`phocus_transducer_validation.ipynb`.
