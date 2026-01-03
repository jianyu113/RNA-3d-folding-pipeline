# RNA-3d-folding-pipeline

This repository demonstrates the inference and post-processing pipeline
I built for the Stanford RNA 3D Folding Kaggle Code Competition.

The project focuses on:
- orchestrating sequence-to-structure inference
- parsing structural biology formats (mmCIF / PDB)
- extracting residue-level C1' coordinates
- robust ensembling aligned with best-of-5 TM-score evaluation

## Scope
This repository does NOT include:
- pre-trained model weights
- Kaggle competition data
- code to reproduce leaderboard results

## Structure
- `pipeline/`: core inference and post-processing logic
- `examples/`: small illustrative inputs and structure files
- `notes/`: design rationale and engineering decisions

## Technologies
Python, pandas, NumPy, BioPython, structural biology tooling