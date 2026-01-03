## What did I do in this project?
This project is based on a Kaggle RNA 3D folding competition, where the task is to predict RNA backbone structures from sequence.
I worked mainly on the inference and post-processing side. I built an end-to-end pipeline that takes RNA sequences, runs stochastic multi-sample inference using pre-trained models, parses structural outputs like mmCIF and PDB files, and generates submission-ready 3D coordinates.
My focus was on robustness, reproducibility, and aligning the pipeline with the TM-score-based evaluation used in the competition.

## Why does the competition ask for five predicted structures instead of one? How does that affect your approach?
Because RNA folding is not deterministic, a single sequence can adopt multiple plausible conformations.
The competition evaluates predictions using a best-of-5 TM-score, so generating diverse candidates increases the chance that at least one structure aligns well with the experimental reference.
In my pipeline, I treated structure prediction as a stochastic generation problem and used multiple samples and models to increase geometric diversity rather than averaging predictions.

## What does TM-score actually measure?
TM-score measures structural similarity after optimal rigid-body alignment, so it focuses on the overall geometric shape rather than absolute coordinates.
This is suitable for RNA structure prediction because we care about whether the predicted fold matches the experimental structure up to rotation and translation, not the exact coordinate system.
It’s also more tolerant to local errors, which makes it appropriate for evaluating global folding accuracy.

## What were the main technical challenges in this project?
One major challenge was handling structural data correctly.
The models output structures in formats like mmCIF or tensors, so parsing residue-level coordinates and ensuring correct indexing was critical.
Another challenge was robustness—some predictions could be invalid or degenerate, so I implemented fallback logic and careful post-processing to ensure the final submission was always valid and reproducible under strict competition constraints.

## How to impove this project?
I would focus on better candidate ranking rather than just generation—for example, training a lightweight scoring model or heuristic to select the most promising structure among the sampled candidates before submission.
This would better exploit the best-of-5 evaluation without increasing inference cost too much.

## How to extract the C1′ coordinates from the model output?
The model outputs full atomic structures in mmCIF format.
By parsing the atomic records, iterating over all atoms, and extracting the Cartesian coordinates of C1′ atoms, which uniquely represent each residue’s backbone position and match the competition’s evaluation protocol.

## Why use two models? Why not just use one stronger model??
Multi-sample inference within a single model mostly explores local variations, while multi-model inference explores different basins in the structural space. Since TM-score selects the best aligned candidate, expanding basin coverage is more effective than increasing samples from a single model.

## Why no coordinate averaging?
Averaging 3D coordinates can destroy geometric consistency and reduce TM-score.
Candidate replacement preserves valid backbone structures.