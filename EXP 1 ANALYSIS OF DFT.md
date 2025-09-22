# EXP 1 :  ANALYSIS OF DFT WITH AUDIO SIGNAL

# AIM: 

  To analyze audio signal by removing unwanted frequency. 

# APPARATUS REQUIRED: 
   
   PC installed with SCILAB/Python. 

# PROGRAM: 

// analyze audio signal
# ==============================
# AUDIO DFT ANALYSIS IN COLAB
# ==============================

# Step 1: Install required packages
```
!pip install -q librosa soundfile
```
# Step 2: Upload audio file
```
from google.colab import files
uploaded = files.upload()   # choose your .wav / .mp3 / .flac file
filename = next(iter(uploaded.keys()))
print("Uploaded:", filename)
```

# Step 3: Load audio
```
import librosa, librosa.display
import numpy as np
import soundfile as sf

y, sr = librosa.load(filename, sr=None, mono=True)  # keep original sample rate
duration = len(y) / sr
print(f"Sample rate = {sr} Hz, duration = {duration:.2f} s, samples = {len(y)}")
```
# Step 4: Play audio
```
from IPython.display import Audio, display
display(Audio(y, rate=sr))
```
# Step 5: Full FFT (DFT) analysis
```
import matplotlib.pyplot as plt

n_fft = 2**14   # choose large power of 2 for smoother spectrum
Y = np.fft.rfft(y, n=n_fft)
freqs = np.fft.rfftfreq(n_fft, 1/sr)
magnitude = np.abs(Y)

plt.figure(figsize=(12,4))
plt.plot(freqs, magnitude)
plt.xlim(0, sr/2)
plt.xlabel("Frequency (Hz)")
plt.ylabel("Magnitude")
plt.title("FFT Magnitude Spectrum (linear scale)")
plt.grid(True)
plt.show()

plt.figure(figsize=(12,4))
plt.semilogy(freqs, magnitude+1e-12)
plt.xlim(0, sr/2)
plt.xlabel("Frequency (Hz)")
plt.ylabel("Magnitude (log scale)")
plt.title("FFT Magnitude Spectrum (log scale)")
plt.grid(True)
plt.show()
```
# Step 6: Top 10 dominant frequencies
```
N = 10
idx = np.argsort(magnitude)[-N:][::-1]
print("\nTop 10 Dominant Frequencies:")
for i, k in enumerate(idx):
    print(f"{i+1:2d}. {freqs[k]:8.2f} Hz  (Magnitude = {magnitude[k]:.2e})")
```
# Step 7: Spectrogram (STFT)
```
n_fft = 2048
hop_length = n_fft // 4
D = librosa.stft(y, n_fft=n_fft, hop_length=hop_length, window='hann')
S_db = librosa.amplitude_to_db(np.abs(D), ref=np.max)

plt.figure(figsize=(12,5))
librosa.display.specshow(S_db, sr=sr, hop_length=hop_length,
                         x_axis='time', y_axis='hz')
plt.colorbar(format="%+2.0f dB")
plt.title("Spectrogram (dB)")
plt.ylim(0, sr/2)
plt.show()
```
## AUDIO USED :
[1758536138260962108gkklt4ugi-voicemaker.in-speech.mp3](https://github.com/user-attachments/files/22471256/1758536138260962108gkklt4ugi-voicemaker.in-speech.mp3)

# OUTPUT: 
Top 10 Dominant Frequencies:

1. 2100.59 Hz (Magnitude = 1.91e+00)
2. 2097.66 Hz (Magnitude = 1.90e+00)
3. 2103.52 Hz (Magnitude = 1.90e+00)
4. 2106.45 Hz (Magnitude = 1.90e+00)
5. 2109.38 Hz (Magnitude = 1.90e+00)
6. 2094.73 Hz (Magnitude = 1.90e+00)
7. 2112.30 Hz (Magnitude = 1.90e+00)
8. 2091.80 Hz (Magnitude = 1.89e+00)
9. 2088.87 Hz (Magnitude = 1.89e+00)
10.2115.23 Hz (Magnitude = 1.89e+00)

<img width="1237" height="928" alt="Screenshot 2025-09-22 211430" src="https://github.com/user-attachments/assets/2def860c-3bbc-4309-b4a4-63dd9416d026" />
<img width="1167" height="775" alt="Screenshot 2025-09-22 211505" src="https://github.com/user-attachments/assets/e38504a8-4a0d-45b3-81c0-c7c5cab017bf" />

# RESULTS
BY USING AUDIO SIGNAL THE DFT IS ANALYSISED USING PYTHON.


