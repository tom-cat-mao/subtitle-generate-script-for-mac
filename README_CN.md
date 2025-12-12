# Mac 自动字幕生成脚本

这是一个专为 Apple Silicon 芯片优化的超快自动化字幕生成工具 [1]。

该工具利用 **FFmpeg** 进行音频提取，并使用 **Lightning-Whisper-MLX** 进行高性能语音识别。通过利用 Apple MLX 框架和 4-bit 量化技术，它在 macOS 设备上实现了业界领先的推理速度，同时保持了高准确率 [1]。

## 🚀 功能特性

*   **Apple Silicon 优化**：基于 Apple MLX 框架构建，充分利用 M 系列芯片的 GPU [1]。
*   **多格式支持**：支持生成 **SRT**、**ASS** (Advanced Substation Alpha)、**VTT** (WebVTT) 和 **TXT** 文件 [1]。
*   **智能量化**：支持 **4-bit** 和 **8-bit** 量化，在极少损失精度的情况下实现更快的推理速度和更低的内存占用 [1]。
*   **自动化工作流**：自动从视频文件（MP4, MKV, MOV 等）提取音频，处理并生成字幕，最后清理临时文件 [1]。
*   **批处理**：通过可配置的 batch size 实现高吞吐量解码 [1]。

## 🛠 前置要求

### 硬件
*   **设备**：搭载 Apple Silicon 芯片的 Mac [1]。
*   **内存 (RAM)**：最低 8GB（使用 `large` 模型建议 16GB 以上）[1]。

### 软件
1.  **FFmpeg**：用于提取音频 [1]。
    ```bash
    brew install ffmpeg
    ```
2.  **Python 3.10+**（建议使用 Conda 环境）[1]。

## 📥 安装指南

### 1. 克隆本仓库

```bash
git clone https://github.com/tom-cat-mao/subtitle-generate-script-for-mac.git sgs
cd sgs
```
[1]

### 2. 安装依赖
你需要安装一个经过修补的 `lightning-whisper-mlx` 库 [1]。

```bash
git clone https://github.com/tom-cat-mao/lightning-whisper-mlx-quantization-fix.git lwm
cd lwm
pip install -e .
```
[1]

## 📖 使用方法

基本语法如下：

```bash
python auto_subtitle.py [视频路径] [选项]
```
[1]

### 示例

**1. 推荐选项**
使用 Medium 模型和 4-bit 量化处理动漫或电影 [1]。
```bash
python auto_subtitle.py "video.mkv" --model medium --quant 4bit
```

**2. 生成 ASS 字幕（适用于本地播放器）**
生成带有样式头的 `.ass` 文件，适用于 IINA 或 VLC 等播放器 [1]。
```bash
python auto_subtitle.py "movie.mp4" --format ass --quant 4bit
```

**3. 生成所有格式**
同时创建 SRT, ASS, VTT 和 TXT 文件 [1]。
```bash
python auto_subtitle.py "video.mov" --format all
```

**4. 高精度模式（Large 模型）**
使用最大的模型以获得最高准确率（需要更多内存）[1]。
```bash
python auto_subtitle.py "lecture.mp4" --model large-v3 --quant 4bit --batch_size 6
```

## ⚙️ 参数与配置

| 参数 | 类型 | 默认值 | 描述 |
| :--- | :--- | :--- | :--- |
| `video_path` | **必填** | - | 输入视频或音频文件的路径 [1]。 |
| `--model` | 可选 | `medium` | 使用的 Whisper 模型大小。<br>选项：`tiny`, `small`, `distil-small.en`, `base`, `medium`, `distil-medium.en`, `large`, `large-v2`, `distil-large-v2`, `large-v3`, `distil-large-v3` [1]。 |
| `--quant` | 可选 | `None` | **量化模式**。<br>• `4bit`：最快，低内存占用（推荐）。<br>• `8bit`：平衡模式。<br>• `None`：全精度 (FP16/FP32) [1]。 |
| `--format` | 可选 | `srt` | 输出格式。<br>选项：`srt`, `ass`, `vtt`, `txt`, `all`。<br>多种格式可用逗号分隔（例如：`srt,ass`）[1]。 |
| `--batch_size` | 可选 | `12` | 推理时的批处理大小。如果遇到内存错误，请减小此值 [1]。 |
| `--keep_audio` | 标志 | `False` | 如果设置，从视频中提取的临时 `.wav` 文件在处理后将**不会**被删除 [1]。 |


## 🔧 故障排除

### 1. 内存不足 (Killed: 9)
**解决方案：**
1. 使用 `--quant 4bit` [1]。
2. 减小批处理大小：`--batch_size 4` [1]。


## 📝 许可证

本项目开源。欢迎修改和分发 [1]。

## 🤝 致谢

*   **Lightning-Whisper-MLX** 由 [Mustafa Aljadery](https://github.com/mustafaaljadery) 开发，提供了出色的 MLX 实现 [1]。
*   **OpenAI** 提供的原始 Whisper 模型 [1]。
*   **Apple** 提供的 MLX 框架 [1]。
