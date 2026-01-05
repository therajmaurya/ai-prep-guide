---
title: "Audio & Speech Processing"
category: specialisation
levels: ["senior", "principal"]
skills: [asr, tts, dsp, transformers, diffusers]
questions:
  "mid": ["Why do we convert audio to Spectrograms before feeding it to a CNN?"]
  "senior": ["Explain the architecture of Whisper."]
  "principal": ["How would you build a real-time voice changer with <20ms latency?"]
---

# Audio & Speech Processing

## 💡 Core Ideas

Audio is sequential like text, but continuous like images. It sits at the intersection of Signal Processing and Deep Learning.

*   **DSP (Digital Signal Processing) Foundations**:
    *   **Sampling Rate**: Nyquist Theorem (sample at > 2x max freq).
    *   **Spectrograms**: FFT (Fast Fourier Transform). STFT (Short-Time FFT) converts 1D audio to 2D image (Time vs Freq).
    *   **Mel Scale**: Perceptual scale (humans hear log-scale frequencies).
*   **ASR (Automatic Speech Recognition)**:
    *   **CTC (Connectionist Temporal Classification)**: Solves the alignment problem (audio length != text length).
    *   **Wav2Vec 2.0 / HuBERT**: Self-supervised pre-training on raw audio.
    *   **Whisper**: Weakly supervised training on 680k hours of internet audio using an Encoder-Decoder Transformer.
*   **TTS (Text-to-Speech)**:
    *   **Spectrogram Generation**: Tacotron 2.
    *   **Vocoder**: Converts spectrogram back to waveform (WaveNet, HiFi-GAN).
    *   **End-to-End**: VITS (Variational Inference with adversarial learning) - single stage.
*   **Mel-frequency cepstral coefficients (MFCCs)**.

## 📚 Resources

*   [**HuggingFace Audio Course**](https://huggingface.co/learn/audio-course/chapter1/introduction) - Modern and practical.
*   [**Deep Learning for Audio Support**](https://github.com/asteroid-team/torch-audiomentations) - Augmentation libraries.
*   [**Speech Technology by CMU**](http://www.speech.cs.cmu.edu/) - Academic resources.
*   [**Speech and Language Processing (Jurafsky & Martin)**](https://web.stanford.edu/~jurafsky/slp3/)


## 🛠️ Practice

### Project 1: Keyword Spotting on Microcontrollers
Train a tiny CNN (Depthwise Separable) to detect "Yes"/"No" on the Speech Commands dataset.
*   **Goal**: Quantize it to int8 and measure inference time on a CPU.

### Project 2: Voice Cloning
Fine-tune a TTS model (like Coqui TTS or VITS) on 10 minutes of your own voice.
*   **Goal**: Generate a podcast intro in your own voice.

### Project 3: Fine-tune Whisper for a specific language.

## 🗣️ Interview Questions

### Mid-Level
*   **Q**: What is a Mel-Spectrogram?
*   **A**: It's a Spectrogram where the frequency axis is converted to the Mel scale, which mimics human ear perception (more resolution at low frequencies, less at high). It is the standard input feature for 99% of Audio models.

### Senior-Level
*   **Q**: Discuss the architecture of a modern end-to-end ASR system.
*   **A**: TBD
*   **Q**: Why is CTC (Connectionist Temporal Classification) necessary for ASR?
*   **A**: Input audio frames (e.g., 1000 frames) never match output text characters (e.g., 50 chars). CTC allows the network to predict a character or a "blank" token $\epsilon$ at every step, and then collapses repeats (`H-H-E-L-L--L-O` -> `HELLO`), solving the alignment problem without explicit timing labels.

### Principal-Level
*   **Q**: What is the Nyquist-Shannon sampling theorem?
*   **A**: It states that a signal must be sampled at a rate greater than twice its highest frequency component to avoid aliasing.
*   **Q**: What is the difference between STFT and Mel-Spectrogram?
*   **A**: STFT converts 1D audio to 2D image (Time vs Freq), while Mel-Spectrogram converts 1D audio to 2D image (Time vs Mel-Freq).
*   **Q**: What is the difference between MFCC and Mel-Spectrogram?
*   **A**: MFCC is a feature extraction method that converts Mel-Spectrogram to a 1D vector, while Mel-Spectrogram is a 2D image.
*   **Q**: Discuss the challenges of "Cocktail Party Problem" (Source Separation).
*   **A**: Separating multiple speakers overlapping in a single channel.
    *   **Permutation Invariant Training (PIT)**: The loss function must calculate the best match between predicted streams and ground truth streams (order doesn't matter).
    *   **Time-Domain vs Freq-Domain**: Modern models (Conv-TasNet) work directly on the waveform (Time Domain) to avoid Phase reconstruction issues associated with STFT.
