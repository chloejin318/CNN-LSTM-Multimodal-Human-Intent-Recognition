# CNN-LSTM-Multimodal-Human-Intent-Recognition
Predicting human cognitive intent in human-robot collaboration by fusing simulated body-language signals with neurophysiological priors; no synchronized sensors required.
### OVERVIEW
Robots operating alongside humans need to anticipate intent before an action completes — not just react to it. This repo implements a CNN-LSTM framework that classifies human cognitive/intent state from:
- 🎥 Vision modality — skeletal keypoints extracted via PoseNet from Blender-simulated HRC scenarios
- 🧬 Neurophysiology modality — EEG / EMG / ECG / GSR signals from public datasets, used as a population-level prior \
The two are fused with cross-attention and modeled over time with an LSTM.
<p align="center">
<img width="442" height="182" alt="image" src="https://github.com/user-attachments/assets/422b76b0-472b-4ee3-b4df-cb77cf913338" />
</p>
<p align="center">
Fig 1. Architecture Overview.
</p>

### Why This Works

1. Works on any slice of frames, not just full sequences.
Velocity and human-robot proximity are computed locally within a window, so you can feed in:
- a full recorded session,
- a short clip pulled from anywhere in it, or a live stream of incoming frames
  ...and still get a valid prediction. Good for both offline evaluation and streaming/online inference.
2. Fuses in data it never trained alongside.
The physiology branch is trained on public datasets never paired with this vision data. Cross-attention lets the model pull on physiological evidence selectively; e.g. up-weighting workload signals when the pose already shows hesitation — instead of blending modalities uniformly. This sidesteps the need for live wearable sensors in real HRC deployments.

