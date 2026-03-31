# EEG Meditation Study — ERP & Power Spectral Analysis

Basic analysis of cortical responses to mindfulness interruption prompts using 
event-related potentials (ERP) and power spectral density (PSD) from a 
publicly available meditation EEG dataset.

---

## Project Overview
This project implements a full EEG preprocessing and analysis pipeline using 
MNE-Python, applied to resting-state meditation EEG data. The analysis 
characterises the brain's electrophysiological response to mindfulness 
self-assessment prompts across 4 participants, with a focus on components 
relevant to cognitive and attentional neuroscience.

---

## Dataset
- **Source:** OpenNeuro ds001787 — EEG Meditation Study
- **Reference:** Brandmeyer & Delorme (2018)
- **Participants:** 5 recorded, 4 included after quality control (sub-003 excluded due to excessive artifacts)
- **Sessions:** ses-01 used for all participants
- **EEG System:** BioSemi ActiveTwo, 64 channels, 256 Hz sampling rate
- **Paradigm:** Participants meditated continuously and were interrupted every 
  ~2 minutes to rate their level of mind-wandering vs concentration

## Data Access
Data is publicly available on OpenNeuro. To reproduce this analysis:
1. Download sub-001 through sub-005 from https://openneuro.org/datasets/ds001787
2. Place files in `data/` following BIDS structure:
```
data/sub-001/ses-01/eeg/
data/sub-002/ses-01/eeg/
...
```

---

## Pipeline

### Preprocessing
- Channel type correction (EXG → EOG, GSR/Resp/Plet/Temp → misc)
- Channel renaming from BioSemi A1-B32 to standard 10-20 nomenclature
- Bandpass filtering: 0.1–40 Hz (FIR, Hamming window)
- Epoch extraction: -200ms to +800ms around stimulus onset (event code 128)
- Baseline correction: -200ms to 0ms pre-stimulus
- Artifact rejection: epochs exceeding 150µV peak-to-peak on any EEG channel excluded

### Artifact Rejection Summary
| Subject | Total Epochs | Clean Epochs | Retention |
|---------|-------------|--------------|-----------|
| sub-001 | 28 | 14 | 50% |
| sub-002 | 28 | 10 | 36% |
| sub-003 | 28 | 0  | 0% — excluded |
| sub-004 | 28 | 7  | 25% |
| sub-005 | 28 | 17 | 61% |

---

## Key Findings

### ERP Analysis
- A clear stimulus-evoked response was observed across all included participants
- Early sensory components (N100/P100) visible at ~100ms post-stimulus, 
  reflecting initial detection of the interruption prompt
- A frontal positivity consistent with **P300** emerged at ~300ms, reflecting 
  conscious cognitive evaluation of the mindfulness prompt
- All misclassifications in scalp distribution showed a classic 
  anterior-posterior gradient evolving from 100ms to 500ms post-stimulus
- Grand average computed across N=4 subjects (48 clean epochs total)

### Power Spectral Density
- All subjects showed the characteristic **1/f power spectrum** of biological 
  neural signals, confirming data integrity
- A consistent **Alpha peak (8-11 Hz)** was observed across all 4 subjects, 
  consistent with the relaxed, meditative state of the paradigm
- Inter-subject variability in overall power was observed, particularly in 
  Alpha and Beta bands, reflecting normal individual differences in EEG power
- No 50Hz line noise artifact detected, confirming effective bandpass filtering

### Power Spectral Density
- All subjects showed the characteristic **1/f power spectrum** of biological 
  neural signals, confirming data integrity
- A consistent **Alpha peak (8-11 Hz)** was observed across all 4 subjects, 
  consistent with the relaxed, meditative state of the paradigm
- Inter-subject variability in overall power was observed, particularly in 
  Alpha and Beta bands, reflecting normal individual differences in EEG power
- No 50Hz line noise artifact detected, confirming effective bandpass filtering

### PSD Topography
- **Delta (0.5-4 Hz)** power was strongest over frontal and occipital regions, 
  consistent with slow cortical activity during restful states
- **Theta (4-8 Hz)** showed a predominantly central-posterior distribution, 
  consistent with meditative and internally directed cognition
- **Alpha (8-13 Hz)** displayed a clear **posterior-dominant topography**, 
  with peak power over occipital and parietal regions — the canonical 
  Alpha distribution associated with relaxed wakefulness and meditation
- **Beta (13-30 Hz)** was broadly distributed but relatively low in power, 
  consistent with reduced active cognitive processing during meditation
- **Gamma (30-40 Hz)** showed focal occipital concentration with minimal 
  overall power, as expected in a restful paradigm

---

## Figures
| Figure | Description |
|--------|-------------|
| `figures/01_erp_butterfly_sub001.png` | ERP with stimulus onset markers (sub-001) |
| `figures/02_topomap_sub001.png` | Scalp distribution at key timepoints (sub-001) |
| `figures/03_grand_avg_erp.png` | Grand average ERP across 4 subjects |
| `figures/04_grand_avg_topomap.png` | Grand average scalp distribution |
| `figures/05_psd_per_subject.png` | Power spectral density per subject |
| `figures/06_psd_topomap_bands.png` | PSD topomaps across all frequency bands (grand average) |

---

## Notebooks
| Notebook | Description |
|----------|-------------|
| `meditation_analysis.ipynb` | Full pipeline: loading, preprocessing, ERP, PSD |

---

## Tools & Libraries
- Python 3.x
- MNE-Python 1.11.0
- NumPy, Pandas
- Matplotlib

---

## Background & Motivation
This project is part of a broader portfolio exploring EEG biomarkers relevant 
to mental health and cognitive wellbeing. Mindfulness and meditation paradigms 
offer a valuable window into the neural correlates of attentional regulation — 
a key mechanism in stress resilience and psychological wellbeing.

The P300 component identified here is of particular interest, as its amplitude 
and latency have been studied as potential biomarkers of attentional capacity 
and cognitive load — with implications for clinical assessment of anxiety, 
depression, and attentional disorders.

---

## Limitations & Next Steps
- Low trial count per subject (7-17 clean epochs) limits ERP SNR — 
  extending to all 24 subjects and both sessions would substantially 
  improve signal quality
- Independent Component Analysis (ICA) would improve artifact rejection 
  compared to simple amplitude thresholding
- Future analyses: ERSP (event-related spectral perturbation), 
  EEG connectivity, comparison of high vs low mind-wandering trials
