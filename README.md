
<div align="center">

<h1>音频分离器 🎶</h1>
<br>

[![PyPI 版本](https://badge.fury.io/py/audio-separator.svg)](https://badge.fury.io/py/audio-separator)
[![Conda 版本](https://img.shields.io/conda/vn/conda-forge/audio-separator.svg)](https://anaconda.org/conda-forge/audio-separator)
[![Docker 下载](https://img.shields.io/docker/pulls/beveradb/audio-separator.svg)](https://hub.docker.com/r/beveradb/audio-separator/tags)
[![代码覆盖率](https://codecov.io/gh/karaokenerds/python-audio-separator/graph/badge.svg?token=N7YK4ET5JP)](https://codecov.io/gh/karaokenerds/python-audio-separator)

**中文简体** | [**English**](./README_EN.md) 

</div>

---


## 功能

- 将音频分离成多个音轨，例如伴奏和人声。
- 支持所有常见音频格式（WAV、MP3、FLAC、M4A 等）。
- 可以使用预训练模型（PTH 或 ONNX 格式）进行推理。
- 支持命令行接口，便于在脚本和批处理任务中使用。
- 提供 Python API，便于集成到其他项目中。

## 使用 GPU / CUDA 的具体安装步骤（通过 Pip）


cuda

- conda install pytorch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 pytorch-cuda=11.8 -c pytorch -c nvidia

或者

- `pip install torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cu118`

- `pip install --force-reinstall onnxruntime-gpu`
- `pip install "audio-separator[gpu]"`
- `apt-get update; apt-get install -y ffmpeg`

ONNX Runtime 仍需要 CUDA 11 库。
所以cuda安装11.8版本

## 使用 🚀

### 命令行接口（CLI）

你可以通过命令行使用音频分离器，例如：

```sh
audio-separator audio.wav --model_filename UVR-MDX-NET-Inst_HQ_3.onnx --output_dir ./output
```

此命令将下载指定的模型文件，处理 `audio.wav` 输入音频，并在当前目录中生成两个新文件，一个包含人声，一个包含伴奏。

**注意**：你不需要自己下载任何文件——音频分离器会自动为你下载！

要查看支持的模型列表，请运行 `audio-separator --list_models`

在列表输出中列出的任何文件都可以通过 `model_filename` 参数指定（例如 `--model_filename UVR_MDXNET_KARA_2.onnx`），并将在第一次使用时自动下载到 `--model_file_dir`（默认：`/tmp/audio-separator-models/`）文件夹中。

### 完整的命令行接口选项

```sh
usage: audio-separator [-h] [-v] [-d] [-e] [-l] [--log_level LOG_LEVEL] [-m MODEL_FILENAME] [--output_format OUTPUT_FORMAT] [--output_dir OUTPUT_DIR] [--model_file_dir MODEL_FILE_DIR] [--invert_spect]
                       [--normalization NORMALIZATION] [--single_stem SINGLE_STEM] [--sample_rate SAMPLE_RATE] [--mdx_segment_size MDX_SEGMENT_SIZE] [--mdx_overlap MDX_OVERLAP] [--mdx_batch_size MDX_BATCH_SIZE]
                       [--mdx_hop_length MDX_HOP_LENGTH] [--mdx_enable_denoise] [--vr_batch_size VR_BATCH_SIZE] [--vr_window_size VR_WINDOW_SIZE] [--vr_aggression VR_AGGRESSION] [--vr_enable_tta]
                       [--vr_high_end_process] [--vr_enable_post_process] [--vr_post_process_threshold VR_POST_PROCESS_THRESHOLD] [--demucs_segment_size DEMUCS_SEGMENT_SIZE] [--demucs_shifts DEMUCS_SHIFTS]
                       [--demucs_overlap DEMUCS_OVERLAP] [--demucs_segments_enabled DEMUCS_SEGMENTS_ENABLED] [--mdxc_segment_size MDXC_SEGMENT_SIZE] [--mdxc_override_model_segment_size]
                       [--mdxc_overlap MDXC_OVERLAP] [--mdxc_batch_size MDXC_BATCH_SIZE] [--mdxc_pitch_shift MDXC_PITCH_SHIFT]
                       [audio_file]

将音频文件分离成不同音轨。

位置参数：
  audio_file                                             要

分离的音频文件路径，支持任何常见格式。

选项：
  -h, --help                                             显示此帮助信息并退出。

信息和调试：
  -v, --version                                          显示程序版本号并退出。
  -d, --debug                                            启用调试日志，相当于 --log_level=debug。
  -e, --env_info                                         打印环境信息并退出。
  -l, --list_models                                      列出所有支持的模型并退出。
  --log_level LOG_LEVEL                                  日志级别，例如 info, debug, warning（默认：info）。

分离输入输出参数：
  -m MODEL_FILENAME, --model_filename MODEL_FILENAME     用于分离的模型文件名（默认：UVR-MDX-NET-Inst_HQ_3.onnx）。例如：-m 2_HP-UVR.pth
  --output_format OUTPUT_FORMAT                          分离文件的输出格式，支持任何常见格式（默认：FLAC）。例如：--output_format=MP3
  --output_dir OUTPUT_DIR                                写入输出文件的目录（默认：当前目录）。例如：--output_dir=/app/separated
  --model_file_dir MODEL_FILE_DIR                        模型文件目录（默认：/tmp/audio-separator-models/）。例如：--model_file_dir=/app/models

常见分离参数：
  --invert_spect                                         使用频谱图反转次要音轨（默认：False）。例如：--invert_spect
  --normalization NORMALIZATION                          设置输出文件的幅度归一化值（默认：0.9）。例如：--normalization=0.7
  --single_stem SINGLE_STEM                              仅输出单个音轨，例如伴奏、人声、鼓、贝斯、吉他、钢琴、其他。例如：--single_stem=Instrumental
  --sample_rate SAMPLE_RATE                              设置输出音频的采样率（默认：44100）。例如：--sample_rate=44100

MDX 架构参数：
  --mdx_segment_size MDX_SEGMENT_SIZE                    分段大小，越大占用资源越多，但效果可能更好（默认：256）。例如：--mdx_segment_size=256
  --mdx_overlap MDX_OVERLAP                              预测窗口的重叠量，0.001-0.999。越高效果越好但速度越慢（默认：0.25）。例如：--mdx_overlap=0.25
  --mdx_batch_size MDX_BATCH_SIZE                        批处理大小，越大占用更多内存但处理速度可能稍快（默认：1）。例如：--mdx_batch_size=4
  --mdx_hop_length MDX_HOP_LENGTH                        通常称为神经网络中的步长；仅在了解其作用的情况下更改（默认：1024）。例如：--mdx_hop_length=1024
  --mdx_enable_denoise                                   分离后启用降噪（默认：False）。例如：--mdx_enable_denoise

VR 架构参数：
  --vr_batch_size VR_BATCH_SIZE                          一次处理的“批次”数量。越高占用更多内存但处理速度稍快（默认：4）。例如：--vr_batch_size=16
  --vr_window_size VR_WINDOW_SIZE                        平衡质量和速度。1024 = 快但质量较低，320 = 慢但质量较高（默认：512）。例如：--vr_window_size=320
  --vr_aggression VR_AGGRESSION                          提取主要音轨的强度，-100 - 100。通常为 5（默认：5）。例如：--vr_aggression=2
  --vr_enable_tta                                        启用测试时间增强；速度慢但提高质量（默认：False）。例如：--vr_enable_tta
  --vr_high_end_process                                  镜像输出的缺失频率范围（默认：False）。例如：--vr_high_end_process
  --vr_enable_post_process                               识别人声输出中的剩余伪影；可能改善某些歌曲的分离效果（默认：False）。例如：--vr_enable_post_process
  --vr_post_process_threshold VR_POST_PROCESS_THRESHOLD  后处理功能的阈值：0.1-0.3（默认：0.2）。例如：--vr_post_process_threshold=0.1

Demucs 架构参数：
  --demucs_segment_size DEMUCS_SEGMENT_SIZE              音频分割的段大小，1-100。越高速度越慢但质量更好（默认：默认）。例如：--demucs_segment_size=256
  --demucs_shifts DEMUCS_SHIFTS                          带随机移位的预测次数，越高速度越慢但质量更好（默认：2）。例如：--demucs_shifts=4
  --demucs_overlap DEMUCS_OVERLAP                        预测窗口的重叠量，0.001-0.999。越高速度越慢但质量更好（默认：0.25）。例如：--demucs_overlap=0.25
  --demucs_segments_enabled DEMUCS_SEGMENTS_ENABLED      启用分段处理（默认：True）。例如：--demucs_segments_enabled=False

MDXC 架构参数：
  --mdxc_segment_size MDXC_SEGMENT_SIZE                  分段大小，越大占用资源越多但效果可能更好（默认：256）。例如：--mdxc_segment_size=256
  --mdxc_override_model_segment_size                     覆盖模型默认分段大小而不是使用模型默认值。例如：--mdxc_override_model_segment_size
  --mdxc_overlap MDXC_OVERLAP                            预测窗口的重叠量，2-50。越高效果越好但速度越慢（默认：8）。例如：--mdxc_overlap=8
  --mdxc_batch_size MDXC_BATCH_SIZE                      批处理大小，越大占用更多内存但处理速度可能稍快（默认：1）。例如：--mdxc_batch_size=4
  --mdxc_pitch_shift MDXC_PITCH_SHIFT                    处理时将音频音调调整一定数量的半音。可能改善低音/高音人声的输出（默认：0）。例如：--mdxc_pitch_shift=2
```

### 作为 Python 项目中的依赖

你可以在自己的 Python 项目中使用音频分离器。以下是用法示例：

```python
from audio_separator.separator import Separator

# 初始化 Separator 类（带有可选的配置属性，见下文）
separator = Separator()

# 加载机器学习模型（如果未指定，默认为 'model_mel_band_roformer_ep_3005_sdr_11.4360.ckpt'）
separator.load_model()

# 对特定音频文件进行分离而不重新加载模型
output_files = separator.separate('audio1.wav')

print(f"分离完成！输出文件：{' '.join(output_files)}")
```

#### 批量处理和使用多个模型进行处理

你可以处理多个文件而不重新加载模型，以节省时间和内存。

只需在选择或更改模型时加载模型即可。见下面示例：

```python
from audio_separator.separator import Separator

# 初始化 Separator 并带有其他配置属性，见下文
separator = Separator()

# 加载模型
separator.load_model(model_filename='UVR-MDX-NET-Inst_HQ_3.onnx')

# 分离多个音频文件而不重新加载模型
output_file_paths_1 = separator.separate('audio1.wav')
output_file_paths_2 = separator.separate('audio2.wav')
output_file_paths_3 = separator.separate('audio3.wav')

# 加载不同的模型
separator.load_model(model_filename='UVR_MDXNET_KARA_2.onnx')

# 使用新模型分离相同文件
output_file_paths_4 = separator.separate('audio1.wav')
output_file_paths_5 = separator.separate('audio2.wav')
output_file_paths_6 = separator.separate('audio3.wav')
```

## Separator 类的参数

- log_level：（可选）日志级别，例如 INFO、DEBUG、WARNING。默认：logging.INFO
- log_formatter：（可选）日志格式。默认：None，默认为 '%(asctime)s - %(levelname)s - %(module)s - %(message)s'
- model_file_dir：（可选）缓存模型文件的目录。默认：/tmp/audio-separator-models/
- output_dir：（可选）保存分离文件的目录。如果未指定，使用当前目录。
- output_format：（可选）输出文件的格式，任何常见格式（WAV、MP3、FLAC、M4A 等）。默认：WAV
- normalization_threshold：（可选）输出音频振幅的归一化值。默认：0.9
- output_single_stem：（可选）仅输出单个音轨，例如 'Instrumental' 和 'Vocals'。默认：None
- invert_using_spec：（可选）标志以使用频谱图进行反转。默认：False
- sample_rate：（可选）设置输出音频的采样率。默认：44100
- mdx_params：（可选）MDX 架构特定属性和默认值。默认：{"hop_length": 1024, "segment_size": 256, "overlap": 0.25, "batch_size": 1
