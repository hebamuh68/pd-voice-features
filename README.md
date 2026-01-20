# Voice Feature Extraction for Parkinson's Disease Detection

An explainable AI approach for early Parkinson's Disease (PD) detection through acoustic voice analysis.

<img width="2086" height="925" alt="shimmer_comparison_by_gender" src="https://github.com/user-attachments/assets/61a1b570-9b28-4061-810c-5da68e0f960f" />
<img width="2086" height="925" alt="pitch_comparison_by_gender" src="https://github.com/user-attachments/assets/6103c7ee-4275-43e6-8383-fc523624812c" />
<img width="2086" height="925" alt="jitter_comparison_by_gender" src="https://github.com/user-attachments/assets/29278932-49ea-4f85-9e0f-00d423e9cefa" />
<img width="2086" height="925" alt="hnr_comparison_by_gender" src="https://github.com/user-attachments/assets/d49de5fc-4cd0-4243-9e36-9bdb299a77d5" />



## Background

Parkinson's Disease affects the muscles that control speech. Up to **90% of people with PD develop voice changes**, often before other motor symptoms appear. This makes voice analysis a promising non-invasive biomarker for early detection.

## Features Extracted

| Feature | Description | Healthy vs PD |
|---------|-------------|---------------|
| **Pitch** | Fundamental frequency (Hz) - how high/low the voice sounds | Varies by gender |
| **Jitter** | Pitch instability - voice "wobble" between vocal cord vibrations | PD: Higher (>1%) |
| **Shimmer** | Amplitude instability - volume variations between cycles | PD: Higher (>3.8%) |
| **HNR** | Harmonics-to-Noise Ratio - voice clarity vs breathiness (dB) | PD: Lower (<20 dB) |

## Installation

```bash
# Clone the repository
git clone https://github.com/hebamuh68/pd-voice-features.git
cd pd-voice-features

# Create a virtual environment (recommended)
conda create -n pd_voice python=3.11
conda activate pd_voice

# Install dependencies
pip install -r requirements.txt
```

## Usage

### Quick Start

```python
import parselmouth
from parselmouth.praat import call
import numpy as np

# Load a voice recording
voice = parselmouth.Sound("path/to/audio.wav")

# Extract pitch
pitch = voice.to_pitch()
pitch_values = pitch.selected_array['frequency']
mean_pitch = np.mean(pitch_values[pitch_values > 0])

# Extract jitter
point_process = call(voice, "To PointProcess (periodic, cc)", 75, 500)
jitter = call(point_process, "Get jitter (local)", 0, 0, 0.0001, 0.02, 1.3)

# Extract shimmer
shimmer = call([voice, point_process], "Get shimmer (local)", 0, 0, 0.0001, 0.02, 1.3, 1.6)

# Extract HNR
harmonicity = voice.to_harmonicity()
hnr_values = harmonicity.values[harmonicity.values != -200]
hnr_mean = np.mean(hnr_values)

print(f"Pitch: {mean_pitch:.2f} Hz")
print(f"Jitter: {jitter:.5f}")
print(f"Shimmer: {shimmer:.5f}")
print(f"HNR: {hnr_mean:.2f} dB")
```

### Full Analysis

See [Voice_Feature_Extraction.ipynb](Voice_Feature_Extraction.ipynb) for the complete analysis pipeline with detailed explanations.

## Project Structure

```
pd-voice-features/
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
├── Voice_Feature_Extraction.ipynb     # Main analysis notebook
├── data/
│   ├── HC_AH/                         # Healthy Control recordings
│   └── PD_AH/                         # Parkinson's Disease recordings
├── results/
│   ├── HC_result.csv                  # Extracted features (HC)
│   └── PD_result.csv                  # Extracted features (PD)
└── docs/
    └── Parselmouth_Sound_Guide.pdf    # Reference documentation
```

## Requirements

- Python 3.11+
- parselmouth (Praat wrapper)
- pandas
- numpy

## References

- [Parselmouth Documentation](https://parselmouth.readthedocs.io/)
- [Praat: Doing Phonetics by Computer](https://www.fon.hum.uva.nl/praat/)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Citation

If you use this code in your research, please cite:

```bibtex
@software{pd_voice_analysis,
  author = {Heba Allah Hashim},
  title = {Voice Feature Extraction for Parkinson's Disease Detection},
  year = {2026},
  url = {https://github.com/hebamuh68/pd-voice-features}
}
```

## Acknowledgments

- Built using [Parselmouth](https://github.com/YannickJadoul/Parselmouth) by Yannick Jadoul
- Voice analysis methodology based on established acoustic biomarkers for PD
