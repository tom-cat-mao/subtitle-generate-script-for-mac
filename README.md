# Subtitle-Generate-Script-for-Mac


# Auto Subtitle Generator for macOS (MLX)

An ultra-fast, automated subtitle generation tool optimized for Apple Silicon chips.

This tool leverages **FFmpeg** for audio extraction and **Lightning-Whisper-MLX** for high-performance speech recognition. By utilizing the Apple MLX framework and 4-bit quantization, it achieves state-of-the-art inference speeds on macOS devices while maintaining high accuracy.

## 🚀 Features

*   **Apple Silicon Optimized**: Built on top of Apple's MLX framework to fully utilize the NPU and GPU on M-series chips.
*   **Multi-Format Support**: Generates **SRT**, **ASS** (Advanced Substation Alpha), **VTT** (WebVTT), and **TXT** files.
*   **Smart Quantization**: Supports **4-bit** and **8-bit** quantization for faster inference and lower memory usage with minimal accuracy loss.
*   **Automated Workflow**: Automatically extracts audio from video files (MP4, MKV, MOV, etc.), processes it, generates subtitles, and cleans up temporary files.
*   **Batch Processing**: High-throughput decoding with configurable batch sizes.

## 🛠 Prerequisites

### Hardware
*   **Device**: Mac with Apple Silicon 
*   **RAM**: 8GB minimum (16GB+ recommended for `large` models).

### Software
1.  **FFmpeg**: Required for audio extraction.
    ```bash
    brew install ffmpeg
    ```
2.  **Python 3.10+** (Conda environment recommended).

## 📥 Installation

### 1. Clone this Repository

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

### 2. Install Dependencies
You need to install a patched version `lightning-whisper-mlx` library.

```bash
git clone https://github.com/tom-cat-mao/lightning-whisper-mlx-quantization-fix.git lwm
cd lwm
pip install -e .
```

## 📖 Usage

The basic syntax is:

```bash
python auto_subtitle.py [VIDEO_PATH] [OPTIONS]
```

### Examples

**1. Recommended Option**
Process an anime/movie with the Medium model and 4-bit quantization.
```bash
python auto_subtitle.py "video.mkv" --model medium --quant 4bit
```

**2. Generate ASS Subtitles (For Local Players)**
Generate `.ass` files with styled headers for players like IINA or VLC.
```bash
python auto_subtitle.py "movie.mp4" --format ass --quant 4bit
```

**3. Generate All Formats**
Create SRT, ASS, VTT, and TXT files simultaneously.
```bash
python auto_subtitle.py "video.mov" --format all
```

**4. High Precision (Large Model)**
Use the largest model for maximum accuracy (requires more RAM).
```bash
python auto_subtitle.py "lecture.mp4" --model large-v3 --quant 4bit --batch_size 6
```

## ⚙️ Arguments & Configuration

| Argument | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `video_path` | **Required** | - | Path to the input video or audio file. |
| `--model` | Optional | `medium` | The Whisper model size to use.<br>Options: `tiny`, `small`, `base`, `medium`, `large-v2`, `distil-large-v2`, `large-v3`, `distil-medium.en`. |
| `--quant` | Optional | `None` | **Quantization mode.**<br>• `4bit`: Fastest, low memory (Recommended).<br>• `8bit`: Balanced.<br>• `None`: Full precision (FP16/FP32). |
| `--format` | Optional | `srt` | Output format(s).<br>Options: `srt`, `ass`, `vtt`, `txt`, `all`.<br>Multiple formats can be comma-separated (e.g., `srt,ass`). |
| `--batch_size` | Optional | `12` | Batch size for inference. Reduce this if you encounter memory errors. |
| `--keep_audio` | Flag | `False` | If set, the temporary `.wav` file extracted from the video will **not** be deleted after processing. |

---

## 🔧 Troubleshooting

### 1. Out of Memory (Killed: 9)
**Solution:**
1. Use `--quant 4bit`.
2. Reduce the batch size: `--batch_size 4`.

---

## 📝 License

This project is open-source. Feel free to modify and distribute.

## 🤝 Acknowledgements

*   **Lightning-Whisper-MLX** by [Mustafa Aljadery](https://github.com/mustafaaljadery) for the incredible MLX implementation.
*   **OpenAI** for the original Whisper model.
*   **Apple** for the MLX framework.
