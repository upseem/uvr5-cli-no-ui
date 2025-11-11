简体中文（本页） | [English](./README_EN.md) | [模型列表](./models.md) | [RTX 4090 指南](./README_4090.md)

<div align="center">
 
# 🎶 音频分离器（Audio Separator） 🎶

[![PyPI version](https://badge.fury.io/py/audio-separator.svg)](https://badge.fury.io/py/audio-separator)
[![Conda Version](https://img.shields.io/conda/vn/conda-forge/audio-separator.svg)](https://anaconda.org/conda-forge/audio-separator)
[![Docker pulls](https://img.shields.io/docker/pulls/beveradb/audio-separator.svg)](https://hub.docker.com/r/beveradb/audio-separator/tags)
[![codecov](https://codecov.io/gh/karaokenerds/python-audio-separator/graph/badge.svg?token=N7YK4ET5JP)](https://codecov.io/gh/karaokenerds/python-audio-separator)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1gSlmSmna7f7fH6OjsiMEDLl-aJ9kGPkY?usp=sharing)
[![Open In Huggingface](https://huggingface.co/datasets/huggingface/badges/resolve/main/open-in-hf-spaces-sm.svg)](https://huggingface.co/spaces/theneos/audio-separator)

</div>

**摘要：** 这是一个易用的音频声部分离工具，既可以通过命令行使用，也可以作为 Python 依赖集成到你的项目中。它基于 UVR 社区（@Anjok07 & @aufr33）提供的优秀模型：MDX‑Net、VR 架构、Demucs 和 MDXC。

> 📚 **相关文档：** 查看 [完整模型列表](./models.md) 了解所有可用模型及其特点，或查看 [RTX 4090 专用配置指南](./README_4090.md) 获取 GPU 优化设置。

Audio Separator 是一个 Python 包，可将音频文件分离为多个声部（stems）。所用模型由 @Anjok07 为 [Ultimate Vocal Remover](https://github.com/Anjok07/ultimatevocalremovergui) 训练并提供。

最常见的用法是将音频分成两路：伴奏（Instrumental）与人声（Vocals），对制作卡拉 OK、混音等非常有用。除此之外，UVR 中的模型还能分离鼓、贝斯、钢琴、吉他等更多声部，并支持诸如去噪、去回声/混响等音频处理任务。

<details>
<summary align="center"><b>目录</b></summary>

- 🎶 音频分离器（本页）
  - 功能
  - 安装 🛠️
    - 🐳 Docker
    - 🎮 Nvidia GPU（CUDA）或 🧪 Google Colab
    -  Apple Silicon（CoreML 加速）
    - 🐢 仅 CPU（无硬件加速）
    - 🎥 FFmpeg 依赖
  - 使用方法 🚀
    - 命令行（CLI）
    - 列出与筛选可用模型
      - 按声部分类筛选
      - 限制结果数量
      - 输出 JSON
    - 完整命令行参数
    - 作为 Python 依赖使用
      - 批处理与多模型处理
      - 重命名输出声部
  - Separator 类参数
  - 远程 API 用法 🌐
  - 环境要求 📋
  - 本地开发
    - 先决条件
    - 克隆仓库
    - 创建与激活 Conda 环境
    - 安装依赖
    - 在本地运行 CLI
    - 退出虚拟环境
    - 构建发行包
  - 贡献 🤝
  - 许可 📄
  - 致谢 🙏
  - 联系方式 💌
  - 特别感谢所有贡献者
</details>

---

## 功能

- 将音频分离为多个声部（例如伴奏与人声）。
- 支持所有常见音频格式（WAV、MP3、FLAC、M4A 等）。
- 支持使用 PTH 或 ONNX 格式的预训练模型进行推理。
- 提供 CLI，便于脚本与批处理场景。
- 提供 Python API，便于在其他项目中集成。

## 安装 🛠️

### 🐳 Docker

如果可以使用 Docker，实际上无需在本机“安装”任何依赖——我们在 Docker Hub 提供了适用于 GPU（CUDA）与 CPU 推理的镜像，同时支持 `amd64` 与 `arm64` 平台：

建议把包含待分离音频的文件夹以卷的方式挂载，这样也可以作为输出目录。例如当前目录有 `input.wav`，可运行（更多示例见下方“用法”部分）：

```sh
docker run -it -v `pwd`:/workdir beveradb/audio-separator input.wav
```

若使用带 GPU 的机器，请使用 GPU 专用镜像并把 GPU 设备传入容器：

```sh
docker run -it --gpus all -v `pwd`:/workdir beveradb/audio-separator:gpu input.wav
```

若容器中未能检测到 GPU，请检查 Docker 运行时是否正确透传 GPU（可参考各类指南）。

### 🎮 Nvidia GPU（CUDA）或 🧪 Google Colab

支持的 CUDA 版本：11.8 与 12.2

> 💡 **RTX 4090 用户专用指南：** 如果你使用的是 RTX 4090（Ada 架构），请查看 [RTX 4090 专用配置指南](./README_4090.md) 获取针对 Python 3.12 + CUDA 12.x + 4090 环境的详细配置说明。

成功配置后，运行 `audio-separator --env_info` 应看到：
`ONNXruntime has CUDAExecutionProvider available, enabling acceleration`

Conda：
```sh
conda install pytorch=*=*cuda* onnxruntime=*=*cuda* audio-separator -c pytorch -c conda-forge
```

Pip：
```sh
pip install "audio-separator[gpu]"
```

Docker：
```sh
beveradb/audio-separator:gpu
```

###  Apple Silicon，macOS Sonoma+（M1 或更新机型，CoreML 加速）

成功配置后，运行 `audio-separator --env_info` 应看到：
`ONNXruntime has CoreMLExecutionProvider available, enabling acceleration`

Pip：
```sh
pip install "audio-separator[cpu]"
```

### 🐢 仅 CPU（无硬件加速）

Conda：
```sh
conda install audio-separator-c pytorch -c conda-forge
```

Pip：
```sh
pip install "audio-separator[cpu]"
```

Docker：
```sh
beveradb/audio-separator
```

### 🎥 FFmpeg 依赖

运行 `audio-separator --env_info` 可验证是否已配置 FFmpeg，日志应显示 `FFmpeg installed`。

若通过 conda 或 docker 安装，FFmpeg 通常已包含。否则需单独安装，例如：

🐧 Debian/Ubuntu：
```sh
apt-get update; apt-get install -y ffmpeg
```

 macOS：
```sh
brew update; brew install ffmpeg
```

## 通过 Pip 在 GPU / CUDA 环境下的额外注意

理论上只需用 `[gpu]` 附加项安装即可，但在某些环境中，让 PyTorch 与 ONNX Runtime 同时启用 CUDA 可能会遇到兼容问题。

可能需要手动重新安装，让 pip 为你的平台重新计算合适的版本，例如：

- `pip uninstall torch onnxruntime`
- `pip cache purge`
- `pip install --force-reinstall torch torchvision torchaudio`
- `pip install --force-reinstall onnxruntime-gpu`

推荐使用 PyTorch 官方“本地安装”向导选择适合你环境的安装命令：
<https://pytorch.org/get-started/locally/>

### 可能需要同时安装多个 CUDA 库版本

根据你的 CUDA 版本与运行环境，ONNX Runtime 可能需要不同版本的 CUDA 库。例如 Google Colab 现在默认使用 CUDA 12，但 ONNX Runtime 仍可能需要 CUDA 11 的库。

如果运行 `audio-separator` 出现 `Failed to load library` 或 `cannot open shared object file` 等错误，很可能是此问题。

你可以在保留 CUDA 12 的同时安装 CUDA 11 的库，例如：
```sh
apt update; apt install nvidia-cuda-toolkit
```

如果在 Colab 或其它环境看到如下提示：
```
[E:onnxruntime:Default, provider_bridge_ort.cc:1862 TryGetProviderInfo_CUDA] ... Failed to load library libonnxruntime_providers_cuda.so ... libcudnn_adv.so.9 ... No such file or directory

[W:onnxruntime:Default, onnxruntime_pybind_state.cc:993 CreateExecutionProviderInstance] Failed to create CUDAExecutionProvider. Require cuDNN 9.* and CUDA 12.*. ...
```
可尝试：
```sh
python -m pip install ort-nightly-gpu --index-url=https://aiinfra.pkgs.visualstudio.com/PublicPackages/_packaging/ort-cuda-12-nightly/pypi/simple/
```

> 备注：如果你知道更优雅的跨不同平台自动依赖方案，欢迎交流或提 PR！

## 用法 🚀

### 命令行（CLI）

示例：

```sh
audio-separator /path/to/your/input/audio.wav --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt
```

该命令会自动下载指定模型，处理输入音频 `audio.wav`，并在当前目录生成两份新文件：一份为人声（Vocals），另一份为伴奏（Instrumental）。

**注意：** 无需手动下载模型文件，首次使用会自动下载到 `--model_file_dir`（默认：`/tmp/audio-separator-models/`）。

列出支持的模型：

```sh
audio-separator --list_models
```

`--list_models` 的输出中列出的任意文件（含扩展名）都可直接通过 `--model_filename` 指定（例如 `--model_filename UVR_MDXNET_KARA_2.onnx`），程序会在首次使用时自动下载到模型目录。

### 列出与筛选可用模型

使用 `--list_models`（或 `-l`）查看所有可用模型：

```sh
audio-separator --list_models
```

输出包含如下列：
- 模型文件名（用于 `--model_filename`）
- 架构（MDX、MDXC、Demucs 等）
- 可分离的声部（以及可用时的 SDR 分数）
- 友好名称（描述性名称）

#### 按声部分类筛选

通过 `--list_filter` 按声部筛选并排序，例如查找可分离鼓声的模型：

```sh
audio-separator -l --list_filter=drums
```

示例输出：
```
-----------------------------------------------------------------------------------------------------------------------------------
Model Filename        Arch    Output Stems (SDR)                                            Friendly Name
-----------------------------------------------------------------------------------------------------------------------------------
htdemucs_ft.yaml      Demucs  vocals (10.8), drums (10.1), bass (11.9), other               Demucs v4: htdemucs_ft
hdemucs_mmi.yaml      Demucs  vocals (10.3), drums (9.7), bass (12.0), other                Demucs v4: hdemucs_mmi
htdemucs.yaml         Demucs  vocals (10.0), drums (9.4), bass (11.3), other                Demucs v4: htdemucs
htdemucs_6s.yaml      Demucs  vocals (9.7), drums (8.5), bass (10.0), guitar, piano, other  Demucs v4: htdemucs_6s
```

#### 限制结果数量

使用 `--list_limit` 限制显示的模型数量，例如仅查看人声分离效果前 5 的模型：

```sh
audio-separator -l --list_filter=vocals --list_limit=5
```

示例输出：
```
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Model Filename                             Arch  Output Stems (SDR)                   Friendly Name
--------------------------------------------------------------------------------------------------------------------------------------------------------------
model_bs_roformer_ep_317_sdr_12.9755.ckpt  MDXC  vocals* (12.9), instrumental (17.0)  Roformer Model: BS-Roformer-Viperx-1297
model_bs_roformer_ep_368_sdr_12.9628.ckpt  MDXC  vocals* (12.9), instrumental (17.0)  Roformer Model: BS-Roformer-Viperx-1296
vocals_mel_band_roformer.ckpt              MDXC  vocals* (12.6), other                Roformer Model: MelBand Roformer | Vocals by Kimberley Jensen
melband_roformer_big_beta4.ckpt            MDXC  vocals* (12.5), other                Roformer Model: MelBand Roformer Kim | Big Beta 4 FT by unwa
mel_band_roformer_kim_ft_unwa.ckpt         MDXC  vocals* (12.4), other                Roformer Model: MelBand Roformer Kim | FT by unwa
```

#### 输出 JSON

程序化使用时，可输出 JSON：

```sh
audio-separator -l --list_format=json
```

### 完整命令行参数

```sh
usage: audio-separator [-h] [-v] [-d] [-e] [-l] [--log_level LOG_LEVEL] [--list_filter LIST_FILTER] [--list_limit LIST_LIMIT] [--list_format {pretty,json}] [-m MODEL_FILENAME] [--output_format OUTPUT_FORMAT]
                       [--output_bitrate OUTPUT_BITRATE] [--output_dir OUTPUT_DIR] [--model_file_dir MODEL_FILE_DIR] [--download_model_only] [--invert_spect] [--normalization NORMALIZATION]
                       [--amplification AMPLIFICATION] [--single_stem SINGLE_STEM] [--sample_rate SAMPLE_RATE] [--use_soundfile] [--use_autocast] [--custom_output_names CUSTOM_OUTPUT_NAMES]
                       [--mdx_segment_size MDX_SEGMENT_SIZE] [--mdx_overlap MDX_OVERLAP] [--mdx_batch_size MDX_BATCH_SIZE] [--mdx_hop_length MDX_HOP_LENGTH] [--mdx_enable_denoise] [--vr_batch_size VR_BATCH_SIZE]
                       [--vr_window_size VR_WINDOW_SIZE] [--vr_aggression VR_AGGRESSION] [--vr_enable_tta] [--vr_high_end_process] [--vr_enable_post_process]
                       [--vr_post_process_threshold VR_POST_PROCESS_THRESHOLD] [--demucs_segment_size DEMUCS_SEGMENT_SIZE] [--demucs_shifts DEMUCS_SHIFTS] [--demucs_overlap DEMUCS_OVERLAP]
                       [--demucs_segments_enabled DEMUCS_SEGMENTS_ENABLED] [--mdxc_segment_size MDXC_SEGMENT_SIZE] [--mdxc_override_model_segment_size] [--mdxc_overlap MDXC_OVERLAP]
                       [--mdxc_batch_size MDXC_BATCH_SIZE] [--mdxc_pitch_shift MDXC_PITCH_SHIFT]
                       [audio_files ...]

Separate audio file into different stems.

positional arguments:
  audio_files                                            The audio file paths or directory to separate, in any common format.

options:
  -h, --help                                             show this help message and exit

Info and Debugging:
  -v, --version                                          Show the program's version number and exit.
  -d, --debug                                            Enable debug logging, equivalent to --log_level=debug.
  -e, --env_info                                         Print environment information and exit.
  -l, --list_models                                      List all supported models and exit. Use --list_filter to filter/sort the list and --list_limit to show only top N results.
  --log_level LOG_LEVEL                                  Log level, e.g. info, debug, warning (default: info).
  --list_filter LIST_FILTER                              Filter and sort the model list by 'name', 'filename', or any stem e.g. vocals, instrumental, drums
  --list_limit LIST_LIMIT                                Limit the number of models shown
  --list_format {pretty,json}                            Format for listing models: 'pretty' for formatted output, 'json' for raw JSON dump

Separation I/O Params:
  -m MODEL_FILENAME, --model_filename MODEL_FILENAME     Model to use for separation (default: model_bs_roformer_ep_317_sdr_12.9755.yaml). Example: -m 2_HP-UVR.pth
  --output_format OUTPUT_FORMAT                          Output format for separated files, any common format (default: FLAC). Example: --output_format=MP3
  --output_bitrate OUTPUT_BITRATE                        Output bitrate for separated files, any ffmpeg-compatible bitrate (default: None). Example: --output_bitrate=320k
  --output_dir OUTPUT_DIR                                Directory to write output files (default: <current dir>). Example: --output_dir=/app/separated
  --model_file_dir MODEL_FILE_DIR                        Model files directory (default: /tmp/audio-separator-models/). Example: --model_file_dir=/app/models
  --download_model_only                                  Download a single model file only, without performing separation.

Common Separation Parameters:
  --invert_spect                                         Invert secondary stem using spectrogram (default: False). Example: --invert_spect
  --normalization NORMALIZATION                          Max peak amplitude to normalize input and output audio to (default: 0.9). Example: --normalization=0.7
  --amplification AMPLIFICATION                          Min peak amplitude to amplify input and output audio to (default: 0.0). Example: --amplification=0.4
  --single_stem SINGLE_STEM                              Output only single stem, e.g. Instrumental, Vocals, Drums, Bass, Guitar, Piano, Other. Example: --single_stem=Instrumental
  --sample_rate SAMPLE_RATE                              Modify the sample rate of the output audio (default: 44100). Example: --sample_rate=44100
  --use_soundfile                                        Use soundfile to write audio output (default: False). Example: --use_soundfile
  --use_autocast                                         Use PyTorch autocast for faster inference (default: False). Do not use for CPU inference. Example: --use_autocast
  --custom_output_names CUSTOM_OUTPUT_NAMES              Custom names for all output files in JSON format (default: None). Example: --custom_output_names='{"Vocals": "vocals_output", "Drums": "drums_output"}'

MDX Architecture Parameters:
  --mdx_segment_size MDX_SEGMENT_SIZE                    Larger consumes more resources, but may give better results (default: 256). Example: --mdx_segment_size=256
  --mdx_overlap MDX_OVERLAP                              Amount of overlap between prediction windows, 0.001-0.999. Higher is better but slower (default: 0.25). Example: --mdx_overlap=0.25
  --mdx_batch_size MDX_BATCH_SIZE                        Larger consumes more RAM but may process slightly faster (default: 1). Example: --mdx_batch_size=4
  --mdx_hop_length MDX_HOP_LENGTH                        Usually called stride in neural networks, only change if you know what you're doing (default: 1024). Example: --mdx_hop_length=1024
  --mdx_enable_denoise                                   Enable denoising during separation (default: False). Example: --mdx_enable_denoise

VR Architecture Parameters:
  --vr_batch_size VR_BATCH_SIZE                          Number of batches to process at a time. Higher = more RAM, slightly faster processing (default: 1). Example: --vr_batch_size=16
  --vr_window_size VR_WINDOW_SIZE                        Balance quality and speed. 1024 = fast but lower, 320 = slower but better quality. (default: 512). Example: --vr_window_size=320
  --vr_aggression VR_AGGRESSION                          Intensity of primary stem extraction, -100 - 100. Typically, 5 for vocals & instrumentals (default: 5). Example: --vr_aggression=2
  --vr_enable_tta                                        Enable Test-Time-Augmentation; slow but improves quality (default: False). Example: --vr_enable_tta
  --vr_high_end_process                                  Mirror the missing frequency range of the output (default: False). Example: --vr_high_end_process
  --vr_enable_post_process                               Identify leftover artifacts within vocal output; may improve separation for some songs (default: False). Example: --vr_enable_post_process
  --vr_post_process_threshold VR_POST_PROCESS_THRESHOLD  Threshold for post_process feature: 0.1-0.3 (default: 0.2). Example: --vr_post_process_threshold=0.1

Demucs Architecture Parameters:
  --demucs_segment_size DEMUCS_SEGMENT_SIZE              Size of segments into which the audio is split, 1-100. Higher = slower but better quality (default: Default). Example: --demucs_segment_size=256
  --demucs_shifts DEMUCS_SHIFTS                          Number of predictions with random shifts, higher = slower but better quality (default: 2). Example: --demucs_shifts=4
  --demucs_overlap DEMUCS_OVERLAP                        Overlap between prediction windows, 0.001-0.999. Higher = slower but better quality (default: 0.25). Example: --demucs_overlap=0.25
  --demucs_segments_enabled DEMUCS_SEGMENTS_ENABLED      Enable segment-wise processing (default: True). Example: --demucs_segments_enabled=False

MDXC Architecture Parameters:
  --mdxc_segment_size MDXC_SEGMENT_SIZE                  Larger consumes more resources, but may give better results (default: 256). Example: --mdxc_segment_size=256
  --mdxc_override_model_segment_size                     Override model default segment size instead of using the model default value. Example: --mdxc_override_model_segment_size
  --mdxc_overlap MDXC_OVERLAP                            Amount of overlap between prediction windows, 2-50. Higher is better but slower (default: 8). Example: --mdxc_overlap=8
  --mdxc_batch_size MDXC_BATCH_SIZE                      Larger consumes more RAM but may process slightly faster (default: 1). Example: --mdxc_batch_size=4
  --mdxc_pitch_shift MDXC_PITCH_SHIFT                    Shift audio pitch by a number of semitones while processing. May improve output for deep/high vocals. (default: 0). Example: --mdxc_pitch_shift=2
```

#### 中文说明（译）

用途：
- 将音频文件分离为不同声部（stems）。

位置参数：
- `audio_files`：要分离的音频文件路径或目录，支持常见格式。

选项：
- `-h, --help`：显示帮助并退出。

信息与调试：
- `-v, --version`：显示程序版本并退出。
- `-d, --debug`：启用调试日志，相当于 `--log_level=debug`。
- `-e, --env_info`：打印环境信息并退出。
- `-l, --list_models`：列出所有支持的模型并退出；可搭配 `--list_filter` 过滤/排序，`--list_limit` 限制数量。
- `--log_level LOG_LEVEL`：日志级别，例如 info、debug、warning（默认：info）。
- `--list_filter LIST_FILTER`：按 name、filename 或任一声部（如 vocals、instrumental、drums）过滤/排序模型列表。
- `--list_limit LIST_LIMIT`：限制显示的模型数量。
- `--list_format {pretty,json}`：列表输出格式：`pretty` 表格形式；`json` 原始 JSON。

分离 I/O 参数：
- `-m, --model_filename MODEL_FILENAME`：用于分离的模型（默认：`model_bs_roformer_ep_317_sdr_12.9755.yaml`）。示例：`-m 2_HP-UVR.pth`。
- `--output_format OUTPUT_FORMAT`：输出文件格式，任意常见格式（默认：FLAC）。示例：`--output_format=MP3`。
- `--output_bitrate OUTPUT_BITRATE`：输出比特率，任意 ffmpeg 兼容值（默认：None）。示例：`--output_bitrate=320k`。
- `--output_dir OUTPUT_DIR`：输出目录（默认：当前目录）。示例：`--output_dir=/app/separated`。
- `--model_file_dir MODEL_FILE_DIR`：模型文件目录（默认：`/tmp/audio-separator-models/`）。示例：`--model_file_dir=/app/models`。
- `--download_model_only`：仅下载指定模型，不执行分离。

通用分离参数：
- `--invert_spect`：使用频谱反相生成次级声部（默认：False）。
- `--normalization NORMALIZATION`：输入输出音频最大峰值归一化（默认：0.9）。
- `--amplification AMPLIFICATION`：最小峰值增益阈值（默认：0.0）。
- `--single_stem SINGLE_STEM`：仅输出单一声部，如 Instrumental、Vocals、Drums、Bass、Guitar、Piano、Other。
- `--sample_rate SAMPLE_RATE`：设置输出采样率（默认：44100）。
- `--use_soundfile`：使用 soundfile 写出音频（默认：False）。
- `--use_autocast`：启用 PyTorch autocast 以加速（默认：False；CPU 推理请勿使用）。
- `--custom_output_names JSON`：为所有输出自定义文件名（JSON 格式）。示例：`'{"Vocals": "vocals_output", "Drums": "drums_output"}'`。

MDX 架构参数：
- `--mdx_segment_size`：分段大小，越大占用越多资源但可能更好（默认：256）。
- `--mdx_overlap`：预测窗口重叠，0.001–0.999；越高越慢但更好（默认：0.25）。
- `--mdx_batch_size`：批大小，越大占用更多内存但略快（默认：1）。
- `--mdx_hop_length`：步长（通常称 stride），非必要不建议修改（默认：1024）。
- `--mdx_enable_denoise`：启用去噪（默认：False）。

VR 架构参数：
- `--vr_batch_size`：批大小（默认：1）。
- `--vr_window_size`：质量/速度平衡。1024 更快但稍低，320 更慢但质量更好（默认：512）。
- `--vr_aggression`：主声部提取强度，-100 到 100（默认：5；人声/伴奏常用 5）。
- `--vr_enable_tta`：启用 TTA，较慢但质量更好（默认：False）。
- `--vr_high_end_process`：镜像输出缺失的高频范围（默认：False）。
- `--vr_enable_post_process`：识别并处理人声残留伪影（默认：False）。
- `--vr_post_process_threshold`：后处理阈值 0.1–0.3（默认：0.2）。

Demucs 架构参数：
- `--demucs_segment_size`：音频切分的片段大小，1–100；越高越慢但更好（默认：Default）。
- `--demucs_shifts`：带随机偏移的预测次数，越高越慢但更好（默认：2）。
- `--demucs_overlap`：预测窗口重叠，0.001–0.999；越高越慢但更好（默认：0.25）。
- `--demucs_segments_enabled`：启用分段式处理（默认：True）。

MDXC 架构参数：
- `--mdxc_segment_size`：分段大小（默认：256）。
- `--mdxc_override_model_segment_size`：覆盖模型默认的分段大小。
- `--mdxc_overlap`：预测窗口重叠，范围 2–50；越高越慢但更好（默认：8）。
- `--mdxc_batch_size`：批大小（默认：1）。
- `--mdxc_pitch_shift`：处理时按半音单位移调；对低/高音人声可能有助（默认：0）。

### 作为 Python 依赖在项目中使用

这是一个使用默认“两路（伴奏/人声）”模型的最小示例：

```python
from audio_separator.separator import Separator

# 初始化 Separator（可按需传入配置）
separator = Separator()

# 加载模型（不指定则使用默认：'model_mel_band_roformer_ep_3005_sdr_11.4360.ckpt'）
separator.load_model()

# 在不重复加载模型的情况下，直接对音频进行分离
output_files = separator.separate('audio1.wav')

print(f"分离完成！输出文件: {' '.join(output_files)}")
```

#### 批处理与多模型处理

你可以在不重复加载模型的情况下处理多个文件，从而节省时间与显存/内存。仅在更换模型时才需要调用 `load_model`：

```python
from audio_separator.separator import Separator

separator = Separator()

# 加载某个模型
separator.load_model(model_filename='model_bs_roformer_ep_317_sdr_12.9755.ckpt')

# 批量分离多个音频
output_files = separator.separate(['audio1.wav', 'audio2.wav', 'audio3.wav'])

# 切换另一个模型
separator.load_model(model_filename='UVR_MDXNET_KARA_2.onnx')

# 使用新模型再次分离同一批文件
output_files = separator.separate(['audio1.wav', 'audio2.wav', 'audio3.wav'])
```

你也可以传入一个包含音频文件的文件夹路径，而不是逐个列出：
```python
from audio_separator.separator import Separator

separator = Separator()
separator.load_model(model_filename='model_bs_roformer_ep_317_sdr_12.9755.ckpt')

# 分离文件夹中所有音频
output_files = separator.separate('path/to/audio_directory')
```

#### 重命名输出声部

可以通过指定输出名称来重命名分离出的文件，例如：
```python
output_names = {
    "Vocals": "vocals_output",
    "Instrumental": "instrumental_output",
}
output_files = separator.separate('audio1.wav', output_names)
```
此时输出文件名将为：`vocals_output.wav` 与 `instrumental_output.wav`。

也可以只重命名单个声部：

- 重命名人声（Vocals）
  ```python
  output_names = {
      "Vocals": "vocals_output",
  }
  output_files = separator.separate('audio1.wav', output_names)
  ```
  > 输出文件名将为：`vocals_output.wav` 与 `audio1_(Instrumental)_model_mel_band_roformer_ep_3005_sdr_11.wav`

- 重命名伴奏（Instrumental）
  ```python
  output_names = {
      "Instrumental": "instrumental_output",
  }
  output_files = separator.separate('audio1.wav', output_names)
  ```
  > 输出文件名将为：`audio1_(Vocals)_model_mel_band_roformer_ep_3005_sdr_11.wav` 与 `instrumental_output.wav`

- Demucs 模型的声部列表示例：
  - htdemucs_6s.yaml
    ```python
    output_names = {
        "Vocals": "vocals_output",
        "Drums": "drums_output",
        "Bass": "bass_output",
        "Other": "other_output",
        "Guitar": "guitar_output",
        "Piano": "piano_output",
    }
    ```
  - 其他 Demucs 模型
    ```python
    output_names = {
        "Vocals": "vocals_output",
        "Drums": "drums_output",
        "Bass": "bass_output",
        "Other": "other_output",
    }
    ```

## Separator 类参数

- **`log_level`：**（可选）日志级别，如 INFO、DEBUG、WARNING。默认：`logging.INFO`
- **`log_formatter`：**（可选）日志格式。默认：None（回退为 `'%(asctime)s - %(levelname)s - %(module)s - %(message)s'`）
- **`model_file_dir`：**（可选）模型缓存目录。默认：`/tmp/audio-separator-models/`
- **`output_dir`：**（可选）输出目录。不指定则使用当前目录。
- **`output_format`：**（可选）输出音频格式，任意常见格式（WAV、MP3、FLAC、M4A 等）。默认：`WAV`
- **`normalization_threshold`：**（可选）输出归一化峰值。默认：`0.9`
- **`amplification_threshold`：**（可选）最小峰值放大阈值。若音频峰值低于该值，会放大至该阈值。默认：`0.0`
- **`output_single_stem`：**（可选）仅输出单一声部（如 Instrumental 或 Vocals）。默认：`None`
- **`invert_using_spec`：**（可选）是否使用频谱反相。默认：`False`
- **`sample_rate`：**（可选）输出采样率。默认：`44100`
- **`use_soundfile`：**（可选）使用 soundfile 写出音频，可缓解长音频的 OOM 问题。
- **`use_autocast`：**（可选）启用 PyTorch autocast 加速（不适用于 CPU 推理）。默认：`False`
- **`mdx_params`：**（可选）MDX 架构参数与默认值。`{"hop_length": 1024, "segment_size": 256, "overlap": 0.25, "batch_size": 1, "enable_denoise": False}`
- **`vr_params`：**（可选）VR 架构参数与默认值。`{"batch_size": 1, "window_size": 512, "aggression": 5, "enable_tta": False, "enable_post_process": False, "post_process_threshold": 0.2, "high_end_process": False}`
- **`demucs_params`：**（可选）Demucs 架构参数与默认值。`{"segment_size": "Default", "shifts": 2, "overlap": 0.25, "segments_enabled": True}`
- **`mdxc_params`：**（可选）MDXC 架构参数与默认值。`{"segment_size": 256, "override_model_segment_size": False, "batch_size": 1, "overlap": 8, "pitch_shift": 0}`

## 远程 API 用法 🌐

项目包含远程 API 客户端，可连接到已部署的 Audio Separator 服务，从而在远程执行分离任务。该 API 采用异步作业 + 轮询的方式高效处理任务。

如何在 modal.com 上部署 Audio Separator 并用于远程处理，请参阅：[`audio_separator/remote/README.md`](audio_separator/remote/README.md)。

## 环境要求 📋

Python >= 3.10

依赖库：torch、onnx、onnxruntime、numpy、librosa、requests、six、tqdm、pydub

## 本地开发

本项目使用 Poetry 进行依赖管理与打包。建议步骤：

### 先决条件

- 确保已安装 Python 3.10 或更新版本。
- 建议安装 Conda（推荐 Miniforge）管理虚拟环境。

### 克隆仓库

```sh
git clone https://github.com/YOUR_USERNAME/audio-separator.git
cd audio-separator
```

若你是 fork 的仓库，请将 `YOUR_USERNAME` 替换为你的 GitHub 用户名；若具备权限，也可直接使用上游仓库地址。

### 创建与激活 Conda 环境

```sh
conda env create
conda activate audio-separator-dev
```

### 安装依赖

在虚拟环境中执行：

```sh
poetry install
```

根据运行后端安装额外依赖：
```sh
poetry install --extras "cpu"
```
或
```sh
poetry install --extras "gpu"
```
或
```sh
poetry install --extras "dml"
```

### 在本地运行命令行

```sh
audio-separator path/to/your/audio-file.wav
```

### 退出虚拟环境

```sh
conda deactivate
```

### 构建发行包

```sh
poetry build
```

打包产物将生成于 `dist` 目录（当前仅 @beveradb 有权限发布到 PyPI）。

## 贡献 🤝

非常欢迎贡献！请 fork 本仓库后提交 PR，我会尽快评审与合并。

- 本项目 100% 开源，任何人可自由使用与修改。
- 若维护工作量增大，我会寻求志愿者共同维护（不过可能性不大）。
- MDX‑Net 等模型的研发维护在 [UVR 项目](https://github.com/Anjok07/ultimatevocalremovergui) 中进行，本仓库旨在提供 CLI / Python 封装，便于以编程的方式运行这些模型。若你计划改进“模型本身”，请到 UVR 项目参与协作并获取指引。

## 许可 📄

本项目采用 MIT 许可（见 [LICENSE](LICENSE)）。

- 注意：如果你在其他项目中使用本项目默认模型或任何由 [UVR](https://github.com/Anjok07/ultimatevocalremovergui) 训练的模型，请遵循 MIT 许可条款并向 UVR 及其开发者致谢！

## 致谢 🙏

- [Anjok07](https://github.com/Anjok07) —— [Ultimate Vocal Remover GUI](https://github.com/Anjok07/ultimatevocalremovergui) 作者；本项目大量代码来自该项目，对本项目的优点功不可没！
- [DilanBoskan](https://github.com/DilanBoskan) —— 在项目初期的关键贡献。
- [Kuielab & Woosung Choi](https://github.com/kuielab) —— 原始 MDX‑Net 代码作者。
- [KimberleyJSN](https://github.com/KimberleyJensen) —— 提供了 MDX‑Net 与 Demucs 训练脚本实现方面的建议与帮助。
- [Hv](https://github.com/NaJeongMo/Colab-for-MDX_B) —— 协助在 MDX‑Net 中加入 chunks 处理。
- [zhzhongshi](https://github.com/zhzhongshi) —— 协助在本项目中加入对 MDXC 模型的支持。

## 联系方式 💌

如有问题或建议，请提交 issue 或直接联系 @beveradb（[Andrew Beveridge](mailto:andrew@beveridge.uk)）。

---
<div align="center">

<!-- sponsors --><!-- sponsors -->

## 感谢所有贡献者的努力

<a href="https://github.com/nomadkaraoke/python-audio-separator/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=nomadkaraoke/python-audio-separator" />
  </a>

</div>
