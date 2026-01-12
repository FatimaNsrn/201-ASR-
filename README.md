# 🎙️ Persian Automatic Speech Recognition (ASR)  
### Comparative Study of Modern ASR Models  
**NLP Course Project**

This repository presents a **comparative study of Automatic Speech Recognition (ASR) models for Persian (Farsi)**.  
The project was completed as a **team assignment** for an NLP course and evaluates multiple deep learning and transformer-based ASR systems on a real Persian speech dataset.

---

## 🧠 Task Description

The goal of this project is:

> **To convert spoken Persian audio into written Persian text and compare different ASR modeling approaches.**

We evaluate each model using standard ASR metrics:

- **WER (Word Error Rate)**
- **CER (Character Error Rate)**  

Lower values indicate better transcription quality.

---

## 📂 Dataset

**Dataset:** `hezarai/common-voice-13-fa`  
**Source:** Mozilla Common Voice  

This dataset contains **crowd-sourced Persian speech recordings** with different speakers, accents, and recording conditions.

| Split | Size |
|------|------|
| Training | ~28,000 audio clips |
| Validation + Test | ~10,000 audio clips |
| Clip length | Mostly 2–5 seconds |

The dataset provides a **realistic Persian speech distribution** suitable for benchmarking ASR systems.

---

## 🧹 Preprocessing Pipeline

### Audio
- All audio resampled to **16 kHz**
- Depending on the model:
  - Converted to **Log Mel Spectrograms (80 mel bins)**  
  - Or used as **raw waveform input**

### Text Normalization (Persian-specific)
- Arabic → Persian normalization  
  - `ي → ی`
  - `ك → ک`
- Removed punctuation and non-Persian characters
- Standardized whitespace
- **Character-level tokenization**

These steps were crucial for reducing spelling ambiguity in Persian and improving CER.

---

## 🧪 Models Evaluated

### 1️⃣ CNN + BiLSTM + CTC
A classical ASR architecture using:
- CNN for acoustic features  
- BiLSTM for temporal modeling  
- CTC loss for alignment  

---

### 2️⃣ HuBERT (Self-Supervised)
Model: `jonatasgrosman/exp_w2v2t_fa_hubert_s801`  
Base: `facebook/hubert-large-ll60k`  

A large self-supervised model pre-trained on English and fine-tuned on Persian.

---

### 3️⃣ Wav2Vec2
Base: **Facebook Wav2Vec2 Large (XLSR-53)**  
- Pre-trained on **53 languages**  
- Fine-tuned on Persian  
- Character-level decoding  

---

### 4️⃣ Wav2Vec2 + KenLM
Wav2Vec2 combined with a **KenLM language model** during decoding to improve linguistic consistency.

---

### 5️⃣ NVIDIA FastConformer (RNNT)
Framework: **NVIDIA NeMo**  
Model: `stt_fa_fastconformer_hybrid_large`

- Trained on **2500+ hours of Persian speech**
- Uses **RNN-Transducer (RNNT)**
- Uses **SentencePiece sub-word tokenization**

---

### 6️⃣ Whisper (Transformer)
Framework: **Hezar Library**  
Model: `hezarai/whisper-small-fa`  
Base: **OpenAI Whisper Small (244M parameters)**  
Fine-tuned on Persian speech.

---

## 📊 Results

### 🔹 Performance Comparison

| Model | WER (%) | CER (%) |
|------|---------|---------|
| CNN + BiLSTM + CTC | 90.77 | 38.28 |
| HuBERT (Fine-tuned) | 55.20 | 16.16 |
| Wav2Vec2 | 27.49 | 14.52 |
| Wav2Vec2 + KenLM | 41.76 | 10.12 |
| NVIDIA FastConformer (RNNT) | 11.81 | 3.33 |
| **Whisper (Hezar)** | **6.88** | **1.65** |

---

## 🔍 Observations

- **CNN + BiLSTM + CTC** learned acoustics (low CER) but failed to form correct words (very high WER).
- **HuBERT** showed strong gains from self-supervised pretraining.
- **Wav2Vec2** predicted complete Persian words, not just sounds.
- **KenLM** improved character accuracy but sometimes hurt WER due to spacing issues.
- **FastConformer RNNT** performed extremely well due to large Persian-specific training.
- **Whisper (Hezar)** achieved the **best overall performance**.

---

## 🏁 Conclusion

Modern transformer-based and RNNT ASR models **dramatically outperform classical CTC-based architectures for Persian speech recognition**.  
Fine-tuning on Persian-specific datasets is critical for achieving low WER and CER.
