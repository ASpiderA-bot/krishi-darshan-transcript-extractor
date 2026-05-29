# 🌾 Krishi Darshan — YouTube Transcript Extractor

Automated pipeline to extract, transcribe, and structure transcripts from the official **Doordarshan National Krishi Darshan** YouTube playlist for agricultural knowledge database enhancement.

Built as part of **annam.ai · IIT Ropar Vinternship 2026** under the Vicharanashala Lab for Educational Design (VLED).
[📄 View Full Report](https://docs.google.com/viewer?url=https://raw.githubusercontent.com/ASpiderA-bot/krishi-darshan-transcript-extractor/main/Krishi_Darshan_Project_Report.pdf)

---

## 📋 Project Overview

Krishi Darshan is India's longest-running agricultural TV programme, broadcast by Doordarshan since 1967. This pipeline extracts spoken content from 74 episodes and converts it into structured, searchable text data for downstream AI/NLP applications in agricultural intelligence.

| Detail | Value |
|---|---|
| Source | [Doordarshan National YouTube Playlist](https://www.youtube.com/playlist?list=PLUiMfS6qzIMxC3SpknSSQ85dhfyPEz-rb) |
| Total Episodes | 74 |
| Language | Hindi (hi) |
| Transcription Model | openai/whisper-medium (HuggingFace) |
| Avg Words per Episode | ~3,700 |
| Platform | Kaggle (T4 GPU) |

---

## 🗂️ Repository Structure

```
krishi-darshan-transcript-extractor/
│
├── krishi_darshan_extractor.ipynb     ← Main Kaggle notebook
├── krishi_darshan_transcripts.csv     ← Sample output (7 episodes)
├── requirements.txt                   ← Python dependencies
├── .gitignore
└── README.md
```

---

## ⚙️ Pipeline

```
YouTube Playlist (74 videos)
        ↓
  yt-dlp (audio download, one at a time)
        ↓
  openai/whisper-medium (HuggingFace)
        ↓
  Structured CSV output
        ↓
  (Optional) LLM spelling normalisation pass
```

**Checkpoint system**: Progress is saved after every video. If the session crashes or wifi drops, re-running resumes from the last completed video automatically.

---

## 📊 Output CSV Schema

| Column | Description |
|---|---|
| `episode_id` | YouTube video ID |
| `title` | Episode title |
| `upload_date` | YYYYMMDD format |
| `duration_seconds` | Video length |
| `video_url` | Full YouTube URL |
| `language_detected` | ISO language code (hi) |
| `transcript` | Full Whisper transcript |
| `word_count` | Number of words |
| `status` | success / failed |

---

## 🚀 Setup & Usage

### On Kaggle (recommended)

1. Add your HuggingFace token as a Kaggle Secret named `HF_TOKEN`
2. Upload `krishi_darshan_extractor.ipynb` to a new Kaggle notebook
3. Enable GPU (T4 x1) in notebook settings
4. Run all cells in order

### Install dependencies
```bash
pip install yt-dlp transformers torch torchaudio pandas -q
apt-get install -y ffmpeg -q
```

### Resume after session break
At the end of each session, download both:
- `krishi_darshan_transcripts.csv`
- `checkpoint.txt`

Upload both as a Kaggle Dataset and restore at the start of the next session.

---

## 📁 Sample Data

`krishi_darshan_transcripts.csv` contains 7 completed episode transcripts covering:

- Integrated Farming System (एकीकृत कृषि प्रणाली)
- Agricultural development in Haryana
- Village and rural India prosperity
- Chickpea crop (चना फसल)
- Zaed / summer crops
- Micro-irrigation for increased production
- Wheat sowing techniques (गेहूं की बुवाई)

---

## 🔧 Known Issues

- YouTube auto-captions not available on Doordarshan content — Whisper transcription required
- Hindi phonetic transcription produces spelling inconsistencies (e.g. कृषि → क्रिशी/किर्षी) — addressable with an LLM post-processing pass
- Kaggle sessions expire after 12 hours — checkpoint system handles multi-session runs
- Lab VM H200 GPUs had PyTorch/CUDA version mismatch (torch 2.12+cu130 vs driver CUDA 12.7)

---

## 🙏 Acknowledgements

- **Dr. Sudarshan Iyengar** — VLED Lab, IIT Ropar
- **annam.ai** — Agricultural AI initiative, IIT Ropar
- **Doordarshan National** — Source content
- **OpenAI Whisper** via HuggingFace Transformers

---

*Arnab Acharya · B.Tech CSE, IEM Kolkata · IIT Ropar Summer Internship 2026*
