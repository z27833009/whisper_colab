# Whisper Colab - Audio/Video Transcription Tool

A simple, ready-to-use Google Colab notebook for transcribing audio and video files using OpenAI's Whisper model. No coding required!

## 🚀 Quick Start (3 Steps)

### Step 1: Open in Google Colab
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/yourusername/whisper_colab/blob/main/Whisper_Colab_Simple.ipynb)

### Step 2: Run All Cells
Click `Runtime` → `Run all` (or press `Ctrl+F9`)

### Step 3: Configure Your Settings
The notebook will prompt you for settings. Just fill in the forms!

---

## 📝 How to Configure Parameters

When you run the notebook, you'll see configuration cells with forms like this:

### 1️⃣ **Audio Directory Path**
```python
AUDIO_DIR = "/content/drive/MyDrive/audio_files"  # @param {type:"string"}
```
**How to change:**
- Click on the text field
- Replace with your Google Drive folder path
- Example: `/content/drive/MyDrive/my_recordings`
- Example: `/content/drive/MyDrive/meetings/2024`

### 2️⃣ **Output Directory Path**
```python
OUTPUT_DIR = "/content/drive/MyDrive/transcriptions"  # @param {type:"string"}
```
**How to change:**
- Click on the text field
- Enter where you want to save results
- Example: `/content/drive/MyDrive/transcripts`
- The folder will be created if it doesn't exist

### 3️⃣ **Model Selection**
```python
MODEL_SIZE = "medium"  # @param ["tiny", "base", "small", "medium", "large"]
```
**How to change:**
- Click the dropdown menu
- Select your preferred model:
  - `tiny` = Fastest, lower quality (good for testing)
  - `base` = Fast, decent quality
  - `small` = Balanced
  - `medium` = **Recommended** (best balance)
  - `large` = Highest quality (slow, needs GPU)

### 4️⃣ **Language Setting**
```python
LANGUAGE = "auto"  # @param {type:"string"}
```
**How to change:**
- `"auto"` = Automatic detection (default)
- `"en"` = English
- `"zh"` = Chinese
- `"es"` = Spanish
- `"fr"` = French
- [See all language codes](https://github.com/openai/whisper/blob/main/whisper/tokenizer.py)

### 5️⃣ **Processing Mode**
```python
PROCESS_MODE = "latest"  # @param ["latest", "all", "specific"]
```
**How to change:**
- `"latest"` = Process newest file only
- `"all"` = Process all files in folder
- `"specific"` = Process a specific file (enter name below)

### 6️⃣ **Email Notifications (Optional)**
```python
ENABLE_EMAIL = False  # @param {type:"boolean"}
EMAIL_FROM = ""  # @param {type:"string"}
EMAIL_PASSWORD = ""  # @param {type:"string"}
EMAIL_TO = ""  # @param {type:"string"}
```
**How to enable:**
1. Check the `ENABLE_EMAIL` box
2. Enter your Gmail address in `EMAIL_FROM`
3. Enter Gmail app password in `EMAIL_PASSWORD` ([How to get app password](https://support.google.com/accounts/answer/185833))
4. Enter recipient email in `EMAIL_TO`

---

## 📁 Supported File Formats

- **Audio:** `.mp3`, `.wav`, `.m4a`
- **Video:** `.mp4`, `.avi`, `.mov`, `.mkv`

---

## 📂 Where to Put Your Files

1. Upload files to Google Drive first
2. Organize them in a folder
3. Use the folder path in configuration

**Example Google Drive structure:**
```
My Drive/
├── audio_files/           ← Use this path as AUDIO_DIR
│   ├── meeting1.mp4
│   ├── interview.m4a
│   └── podcast.mp3
└── transcriptions/        ← Use this path as OUTPUT_DIR
    └── (output will go here)
```

---

## 🎯 Common Use Cases

### Use Case 1: Transcribe Latest Meeting Recording
```python
AUDIO_DIR = "/content/drive/MyDrive/meetings"
OUTPUT_DIR = "/content/drive/MyDrive/meeting_transcripts"
MODEL_SIZE = "medium"
LANGUAGE = "auto"
PROCESS_MODE = "latest"
```

### Use Case 2: Batch Process All Podcasts
```python
AUDIO_DIR = "/content/drive/MyDrive/podcasts"
OUTPUT_DIR = "/content/drive/MyDrive/podcast_transcripts"
MODEL_SIZE = "small"  # Faster for many files
LANGUAGE = "en"
PROCESS_MODE = "all"
```

### Use Case 3: Transcribe Specific Video in Chinese
```python
AUDIO_DIR = "/content/drive/MyDrive/videos"
OUTPUT_DIR = "/content/drive/MyDrive/video_subtitles"
MODEL_SIZE = "large"  # Best quality
LANGUAGE = "zh"
PROCESS_MODE = "specific"
SPECIFIC_FILE = "presentation.mp4"
```

---

## 💡 Tips for Best Results

### Speed vs Quality
| If you want... | Use this model | Notes |
|----------------|----------------|-------|
| Quick test | `tiny` | 10 min audio → ~20 seconds |
| Fast processing | `base` or `small` | Good for many files |
| Best balance | `medium` | **Recommended** |
| Maximum accuracy | `large` | Enable GPU first! |

### Enable GPU for Faster Processing
1. Click `Runtime` → `Change runtime type`
2. Select `GPU` under Hardware accelerator
3. Click `Save`
4. 2-3x faster processing!

### File Size Recommendations
- **< 30 min:** Any model works well
- **30-60 min:** Use `small` or `medium`
- **> 60 min:** Split into parts or use `tiny`/`base`

---

## 📤 Output Files

For each audio/video file, you get 4 output files:

| File | Description | Use for |
|------|-------------|---------|
| `.txt` | Plain text | Reading, copying |
| `.srt` | Subtitles with timing | Video players |
| `.vtt` | Web subtitles | Web videos |
| `.json` | Detailed data | Developers |

**Example output:**
```
transcriptions/
└── meeting_2024/
    ├── meeting_2024.txt      # Full transcript text
    ├── meeting_2024.srt      # Subtitle file
    ├── meeting_2024.vtt      # Web subtitle file
    └── meeting_2024.json     # Detailed with timestamps
```

---

## ❓ Troubleshooting

### "File not found"
- Check your folder path starts with `/content/drive/MyDrive/`
- Make sure Google Drive is mounted (Step 1)
- Avoid special characters in folder names

### "Out of memory"
- Use a smaller model (`tiny` or `base`)
- Enable GPU (see tips above)
- Process files one at a time

### "Installation failed"
- Try `Runtime` → `Restart runtime`
- Then run all cells again

### Email not sending
- Use app-specific password, not regular password
- Check spam folder
- Make sure `ENABLE_EMAIL` is checked

---

## 🔧 Advanced Settings

### Custom Whisper Options
You can modify the transcription call in the notebook:
```python
result = model.transcribe(
    file_path,
    language=self.language,
    task="transcribe",  # or "translate" for translation
    temperature=0.0,    # Lower = more consistent
    verbose=False       # True to see progress
)
```

### Batch Processing Specific Files
To process only certain files, modify the find_files function:
```python
# Only process MP4 files
self.supported_formats = [".mp4"]

# Only files containing "meeting"
if "meeting" in filename:
    process_file(filename)
```

---

## 📊 Performance Estimates

| File Duration | Tiny | Base | Small | Medium | Large |
|---------------|------|------|-------|--------|-------|
| 10 min | 20s | 40s | 2min | 5min | 10min |
| 30 min | 1min | 2min | 6min | 15min | 30min |
| 60 min | 2min | 4min | 12min | 30min | 60min |

*Times with GPU enabled. CPU-only is 2-3x slower.*

---

## 🌍 Language Support

Whisper supports 100+ languages. Common codes:
- `en` - English
- `zh` - Chinese
- `es` - Spanish
- `hi` - Hindi
- `ar` - Arabic
- `fr` - French
- `de` - German
- `ja` - Japanese
- `ko` - Korean
- `ru` - Russian

Use `"auto"` to detect automatically!

---

## 📧 Contact & Support

- **Issues?** [Open an issue](https://github.com/yourusername/whisper_colab/issues)
- **Questions?** Check the notebook's built-in instructions
- **Contribute?** PRs welcome!

---

## ⭐ If this helped you, please star the repository!

---

## License

MIT - Free to use and modify!