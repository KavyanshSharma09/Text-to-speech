# Text-to-Speech (Kokoro TTS)

A systematic guide and Jupyter Notebook for generating high-quality Text-to-Speech (TTS) audio using **Kokoro-82M**.

---

## 📋 Systematic Notebook Breakdown

### 🔹 Cell 1: Environment Setup & Installation
Installs required packages and dependencies:
```bash
!pip install -q kokoro>=0.9.4 soundfile
!apt-get -qq -y install espeak-ng > /dev/null 2>&1
```
* **`kokoro`**: Python library for Kokoro TTS pipeline.
* **`soundfile`**: Handles writing generated raw audio arrays to `.wav` files.
* **`espeak-ng`**: Underlying backend dependency required for phonemization.

---

### 🔹 Cell 2: Import Libraries & Initialize Kokoro Pipeline
Loads required Python modules and instantiates the TTS pipeline:
```python
from kokoro import KPipeline
import soundfile as sf
import numpy as np
from IPython.display import display, Audio

pipeline = KPipeline(lang_code='a')
print("Kokoro Pipeline ready.")
```
* **`lang_code='a'`**: Configures pipeline for American English (`'b'` for British English).

---

### 🔹 Cell 3: Speech Generation & Audio Export
Executes inference on custom input text, concatenates audio segments, saves to disk, and displays an inline player:
```python
chosen_voice = 'af_bella'

text = """
Hello! Welcome to the Kokoro Text-to-Speech demonstration notebook. This systematic setup generates high quality audio.
"""

generator = pipeline(text, voice=chosen_voice, speed=1.0, split_pattern=r'\n+')
all_audio = []
for i, (gs, ps, audio) in enumerate(generator):
    all_audio.append(audio)
final_audio = np.concatenate(all_audio)
sf.write("final_output.wav", final_audio, 24000)
print("Success! Saved ONE single file: final_output.wav")
display(Audio(data=final_audio, rate=24000, autoplay=True))
```
* **`voice`**: Selects voice profile (e.g., `'af_bella'`).
* **`final_output.wav`**: Output audio file (24kHz sampling rate).

---

## 🚀 Getting Started
1. Open [`text_to_speech.ipynb`](text_to_speech.ipynb).
2. Run **Cell 1** $\rightarrow$ **Cell 2** $\rightarrow$ **Cell 3** sequentially.
3. Listen to the generated audio in the notebook player or download `final_output.wav`.
