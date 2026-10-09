# IBL choice decoder

Decoding a mouse's upcoming choice from Neuropixels population activity in the International Brain Laboratory (IBL) brain-wide map, and mapping which brain regions carry that information.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1h_RXYJasDB8h5kdx5MWN383Tp_GdEPf9?usp=sharing)

## What it does

**Part A: GRU vs. logistic regression**
- Decodes left vs. right choice from the 500 ms of activity before first movement (10 ms bins, z-scored with training statistics only).
- Compares a logistic regression on the last 5 bins with a PyTorch GRU (hidden size 64, dropout 0.3).
- Evaluated with 5-fold stratified cross-validation and balanced accuracy, since always guessing "right" already scores 0.68 plain accuracy in the example session.
- Also shows when choice information appears over time and how the GRU's hidden-state trajectories for left and right choices separate (PCA).

**Part B: Which regions carry choice information?**
- Decodes choice from each region's neurons alone (logistic regression, 5-fold CV) across 82 insertions and 22 regions.
- Controls for neuron count by subsampling every region to 15 neurons (10 random draws per session).

## Results

- Across six random sessions, the GRU and logistic regression perform about the same (0.607 vs. 0.599 balanced accuracy). The spread across sessions is much larger than the gap between models.
- Decodability is highest in brainstem reticular nuclei, secondary motor cortex and superior colliculus, and near chance in hippocampus and early visual areas, including after matching neuron count.

## Limitations

- The decoding window is the last 100 ms before first movement, so high accuracy in brainstem and motor regions largely reflects movement preparation, not decision making.
- Sessions from the same mouse are not independent.
- Regions with only 3 to 4 sessions (e.g. GRN) are suggestive, not established.
- The GRU adds nothing over a linear decoder here, which is expected with a few hundred trials per session.

## Data and setup

Data: IBL brain-wide map, using the preprocessed PSTHs from the Neuromatch Academy tutorial (aligned to first movement, accessed through the ONE API). The first part of the notebook is the tutorial's setup and data-loading code. The analysis is in Part A and Part B.

Run it in Colab with the badge above. It installs `ONE-api` and `ibllib`, then uses PyTorch and scikit-learn.
