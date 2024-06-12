
<div align="center">

<h1>音频分离器 🎶</h1>
<br>

[![PyPI 版本](https://badge.fury.io/py/audio-separator.svg)](https://badge.fury.io/py/audio-separator)
[![Conda 版本](https://img.shields.io/conda/vn/conda-forge/audio-separator.svg)](https://anaconda.org/conda-forge/audio-separator)
[![Docker 下载](https://img.shields.io/docker/pulls/beveradb/audio-separator.svg)](https://hub.docker.com/r/beveradb/audio-separator/tags)
[![代码覆盖率](https://codecov.io/gh/karaokenerds/python-audio-separator/graph/badge.svg?token=N7YK4ET5JP)](https://codecov.io/gh/karaokenerds/python-audio-separator)

**English** | [**中文简体**](./README.md) 

</div>

---


Summary: Easy to use audio stem separation from the command line or as a dependency in your own Python project, using the amazing MDX-Net, VR Arch, Demucs and MDXC models available in UVR by @Anjok07 & @aufr33.

Audio Separator is a Python package that allows you to separate an audio file into various stems, using models trained by @Anjok07 for use with UVR (<https://github.com/Anjok07/ultimatevocalremovergui>).

The simplest (and probably most used) use case for this package is to separate an audio file into two stems, Instrumental and Vocals, which can be very useful for producing karaoke videos! However, the models available in UVR can separate audio into many more stems, such as Drums, Bass, Piano, and Guitar, and perform other audio processing tasks, such as denoising or removing echo/reverb.

## Features

- Separate audio into multiple stems, e.g. instrumental and vocals.
- Supports all common audio formats (WAV, MP3, FLAC, M4A, etc.)
- Ability to inference using a pre-trained model in PTH or ONNX format.
- CLI support for easy use in scripts and batch processing.
- Python API for integration into other projects.

## Installation 🛠️

### 🐳 Docker

If you're able to use docker, you don't actually need to _install_ anything - there are [images published on Docker Hub](https://hub.docker.com/r/beveradb/audio-separator/tags) for GPU (CUDA) and CPU inferencing, for both `amd64` and `arm64` platforms.

You probably want to volume-mount a folder containing whatever file you want to separate, which can then also be used as the output folder.

For instance, if your current directory has the file `input.wav`, you could execute `audio-separator` as shown below (see [usage](#usage-) section for more details):

```sh
docker run -it -v `pwd`:/workdir beveradb/audio-separator input.wav
```

If you're using a machine with a GPU, you'll want to use the GPU specific image and pass in the GPU device to the container, like this:

```sh
docker run -it --gpus all -v `pwd`:/workdir beveradb/audio-separator:gpu input.wav
```

If the GPU isn't being detected, make sure your docker runtime environment is passing through the GPU correctly - there are [various guides](https://www.celantur.com/blog/run-cuda-in-docker-on-linux/) online to help with that.

### 🎮 Nvidia GPU with CUDA or 🧪 Google Colab

**Supported CUDA Versions:** 11.8 and 12.2

💬 If successfully configured, you should see this log message when running `audio-separator --env_info`:
 `ONNXruntime has CUDAExecutionProvider available, enabling acceleration`

Conda: `conda install pytorch=*=*cuda* onnxruntime=*=*cuda* audio-separator -c pytorch -c conda-forge`

Pip: `pip install "audio-separator[gpu]"`

Docker: `beveradb/audio-separator:gpu`

###  Apple Silicon, macOS Sonoma+ with M1 or newer CPU (CoreML acceleration)

💬 If successfully configured, you should see this log message when running `audio-separator --env_info`:
 `ONNXruntime has CoreMLExecutionProvider available, enabling acceleration`

Pip: `pip install "audio-separator[cpu]"`

### 🐢 No hardware acceleration, CPU only

Conda: `conda install audio-separator-c pytorch -c conda-forge`

Pip: `pip install "audio-separator[cpu]"`

Docker: `beveradb/audio-separator`

### 🎥 FFmpeg dependency

💬 To test if `audio-separator` has been successfully configured to use FFmpeg, run `audio-separator --env_info`. The log will show `FFmpeg installed`.

If you installed `audio-separator` using `conda` or `docker`, FFmpeg should already be avaialble in your environment.

You may need to separately install FFmpeg. It should be easy to install on most platforms, e.g.:

🐧 Debian/Ubuntu: `apt-get update; apt-get install -y ffmpeg`

 macOS:`brew update; brew install ffmpeg`

## GPU / CUDA specific installation steps with Pip

In theory, all you should need to do to get `audio-separator` working with a GPU is install it with the `[gpu]` extra as above.

However, sometimes getting both PyTorch and ONNX Runtime working with CUDA support can be a bit tricky so it may not work that easily.

You may need to reinstall both packages directly, allowing pip to calculate the right versions for your platform, for example:

- `pip uninstall torch onnxruntime`
- `pip cache purge`
- `pip install --force-reinstall torch torchvision torchaudio`
- `pip install --force-reinstall onnxruntime-gpu`

I generally recommend installing the latest version of PyTorch for your environment using the command recommended by the wizard here:
<https://pytorch.org/get-started/locally/>

### Multiple CUDA library versions may be needed

Depending on your CUDA version and environment, you may need to install specific version(s) of CUDA libraries for ONNX Runtime to use your GPU.

🧪 Google Colab, for example, now uses CUDA 12 by default, but ONNX Runtime still needs CUDA 11 libraries to work.

If you see the error `Failed to load library` or `cannot open shared object file` when you run `audio-separator`, this is likely the issue.

You can install the CUDA 11 libraries _alongside_ CUDA 12 like so:
`apt update; apt install nvidia-cuda-toolkit`

> Note: if anyone knows how to make this cleaner so we can support both different platform-specific dependencies for hardware acceleration without a separate installation process for each, please let me know or raise a PR!

## Usage 🚀

### Command Line Interface (CLI)

You can use Audio Separator via the command line, for example:

```sh
audio-separator /path/to/your/input/audio.wav --model_filename UVR-MDX-NET-Inst_HQ_3.onnx
```

This command will download the specified model file, process the `audio.wav` input audio and generate two new files in the current directory, one containing vocals and one containing instrumental.

**Note:** You do not need to download any files yourself - audio-separator does that automatically for you!

To see a list of supported models, run `audio-separator --list_models`

Any file listed in the list models output can be specified (with file extension) with the model_filename parameter (e.g. `--model_filename UVR_MDXNET_KARA_2.onnx`) and it will be automatically downloaded to the `--model_file_dir` (default: `/tmp/audio-separator-models/`) folder on first usage.

### Full command-line interface options
### 命令行选项翻译与说明

`audio-separator` 是用于将音频文件分离成不同音轨的工具。以下是其命令行选项及其对应的中文翻译和说明：

#### 基本参数

- `audio_file`：要分离的音频文件路径，支持任何常见格式。

#### 信息和调试

- `-h, --help`：显示帮助信息并退出。
- `-v, --version`：显示程序的版本号并退出。
- `-d, --debug`：启用调试日志，相当于 `--log_level=debug`。
- `-e, --env_info`：打印环境信息并退出。
- `-l, --list_models`：列出所有支持的模型并退出。
- `--log_level LOG_LEVEL`：设置日志级别，例如 info, debug, warning（默认：info）。

#### 分离输入输出参数

- `-m MODEL_FILENAME, --model_filename MODEL_FILENAME`：用于分离的模型文件名（默认：UVR-MDX-NET-Inst_HQ_3.onnx）。
- `--output_format OUTPUT_FORMAT`：分离文件的输出格式，支持任何常见格式（默认：FLAC）。
- `--output_dir OUTPUT_DIR`：写入输出文件的目录（默认：当前目录）。
- `--model_file_dir MODEL_FILE_DIR`：模型文件目录（默认：/tmp/audio-separator-models/）。

#### 常见分离参数

- `--invert_spect`：使用频谱图反转次要音轨（默认：False）。
- `--normalization NORMALIZATION`：设置输出文件的幅度归一化值（默认：0.9）。
- `--single_stem SINGLE_STEM`：只输出单个音轨，如 Instrumental, Vocals, Drums, Bass, Guitar, Piano, Other。
- `--sample_rate SAMPLE_RATE`：设置输出音频的采样率（默认：44100）。

#### MDX 架构参数

- `--mdx_segment_size MDX_SEGMENT_SIZE`：分段大小，越大占用资源越多，但效果可能更好（默认：256）。
- `--mdx_overlap MDX_OVERLAP`：预测窗口的重叠量，0.001-0.999。越高效果越好，但速度越慢（默认：0.25）。
- `--mdx_batch_size MDX_BATCH_SIZE`：批处理大小，越大占用更多内存，但处理速度可能稍快（默认：1）。
- `--mdx_hop_length MDX_HOP_LENGTH`：通常称为神经网络中的步长；仅在了解其作用的情况下更改（默认：1024）。
- `--mdx_enable_denoise`：分离后启用降噪（默认：False）。

#### VR 架构参数

- `--vr_batch_size VR_BATCH_SIZE`：一次处理的“批次”数量。越高占用更多内存，但处理速度稍快（默认：4）。
- `--vr_window_size VR_WINDOW_SIZE`：平衡质量和速度。1024 = 快但质量较低，320 = 慢但质量较高（默认：512）。
- `--vr_aggression VR_AGGRESSION`：提取主要音轨的强度，-100 - 100。通常为 5（默认：5）。
- `--vr_enable_tta`：启用测试时间增强；速度慢但提高质量（默认：False）。
- `--vr_high_end_process`：镜像输出的缺失频率范围（默认：False）。
- `--vr_enable_post_process`：识别人声输出中的剩余伪影；可能改善某些歌曲的分离效果（默认：False）。
- `--vr_post_process_threshold VR_POST_PROCESS_THRESHOLD`：后处理功能的阈值：0.1-0.3（默认：0.2）。

#### Demucs 架构参数

- `--demucs_segment_size DEMUCS_SEGMENT_SIZE`：音频分割的段大小，1-100。越高速度越慢但质量更好（默认：默认）。
- `--demucs_shifts DEMUCS_SHIFTS`：带随机移位的预测次数，越高速度越慢但质量更好（默认：2）。
- `--demucs_overlap DEMUCS_OVERLAP`：预测窗口的重叠量，0.001-0.999。越高速度越慢但质量更好（默认：0.25）。
- `--demucs_segments_enabled DEMUCS_SEGMENTS_ENABLED`：启用分段处理（默认：True）。

#### MDXC 架构参数

- `--mdxc_segment_size MDXC_SEGMENT_SIZE`：分段大小，越大占用资源越多，但效果可能更好（默认：256）。
- `--mdxc_override_model_segment_size`：覆盖模型默认分段大小，而不是使用模型默认值。
- `--mdxc_overlap MDXC_OVERLAP`：预测窗口的重叠量，2-50。越高效果越好但速度越慢（默认：8）。
- `--mdxc_batch_size MDXC_BATCH_SIZE`：批处理大小，越大占用更多内存但处理速度可能稍快（默认：1）。
- `--mdxc_pitch_shift MDXC_PITCH_SHIFT`：处理时将音频音调调整一定数量的半音。可能改善低音/高音人声的输出（默认：0）。

以上是 `audio-separator` 命令行工具的主要参数及其说明。使用这些参数，可以更好地控制音频分离的过程和结果。

### As a Dependency in a Python Project

You can use Audio Separator in your own Python project. Here's how you can use it:

```python
from audio_separator.separator import Separator

# Initialize the Separator class (with optional configuration properties, below)
separator = Separator()

# Load a machine learning model (if unspecified, defaults to 'model_mel_band_roformer_ep_3005_sdr_11.4360.ckpt')
separator.load_model()

# Perform the separation on specific audio files without reloading the model
output_files = separator.separate('audio1.wav')

print(f"Separation complete! Output file(s): {' '.join(output_files)}")
```

#### Batch processing and processing with multiple models

You can process multiple files without reloading the model to save time and memory.

You only need to load a model when choosing or changing models. See example below:

```python
from audio_separator.separator import Separator

# Initialize the Separator with other configuration properties, below
separator = Separator()

# Load a model
separator.load_model(model_filename='UVR-MDX-NET-Inst_HQ_3.onnx')

# Separate multiple audio files without reloading the model
output_file_paths_1 = separator.separate('audio1.wav')
output_file_paths_2 = separator.separate('audio2.wav')
output_file_paths_3 = separator.separate('audio3.wav')

# Load a different model
separator.load_model(model_filename='UVR_MDXNET_KARA_2.onnx')

# Separate the same files with the new model
output_file_paths_4 = separator.separate('audio1.wav')
output_file_paths_5 = separator.separate('audio2.wav')
output_file_paths_6 = separator.separate('audio3.wav')
```

## Parameters for the Separator class

- log_level: (Optional) Logging level, e.g., INFO, DEBUG, WARNING. Default: logging.INFO
- log_formatter: (Optional) The log format. Default: None, which falls back to '%(asctime)s - %(levelname)s - %(module)s - %(message)s'
- model_file_dir: (Optional) Directory to cache model files in. Default: /tmp/audio-separator-models/
- output_dir: (Optional) Directory where the separated files will be saved. If not specified, uses the current directory.
- output_format: (Optional) Format to encode output files, any common format (WAV, MP3, FLAC, M4A, etc.). Default: WAV
- normalization_threshold: (Optional) The amount by which the amplitude of the output audio will be multiplied. Default: 0.9
- output_single_stem: (Optional) Output only a single stem, such as 'Instrumental' and 'Vocals'. Default: None
- invert_using_spec: (Optional) Flag to invert using spectrogram. Default: False
- sample_rate: (Optional) Set the sample rate of the output audio. Default: 44100
- mdx_params: (Optional) MDX Architecture Specific Attributes & Defaults. Default: {"hop_length": 1024, "segment_size": 256, "overlap": 0.25, "batch_size": 1}
- vr_params: (Optional) VR Architecture Specific Attributes & Defaults. Default: {"batch_size": 16, "window_size": 512, "aggression": 5, "enable_tta": False, "enable_post_process": False, "post_process_threshold": 0.2, "high_end_process": False}
- demucs_params: (Optional) VR Architecture Specific Attributes & Defaults. {"segment_size": "Default", "shifts": 2, "overlap": 0.25, "segments_enabled": True}

## Requirements 📋

Python >= 3.9

Libraries: torch, onnx, onnxruntime, numpy, librosa, requests, six, tqdm, pydub

## Developing Locally

This project uses Poetry for dependency management and packaging. Follow these steps to setup a local development environment:

### Prerequisites

- Make sure you have Python 3.9 or newer installed on your machine.
- Install Conda (I recommend Miniforge: <https://github.com/conda-forge/miniforge>) to manage your Python virtual environments

### Clone the Repository

Clone the repository to your local machine:

```sh
git clone https://github.com/YOUR_USERNAME/audio-separator.git
cd audio-separator
```

Replace YOUR_USERNAME with your GitHub username if you've forked the repository, or use the main repository URL if you have the permissions.

### Create and activate the Conda Environment

To create and activate the conda environment, use the following commands:

```sh
conda env create
conda activate audio-separator-dev
```

### Install Dependencies

Once you're inside the conda env, run the following command to install the project dependencies:

```sh
poetry install
```

### Running the Command-Line Interface Locally

You can run the CLI command directly within the virtual environment. For example:

```sh
audio-separator path/to/your/audio-file.wav
```

### Deactivate the Virtual Environment

Once you are done with your development work, you can exit the virtual environment by simply typing:

```sh
conda deactivate
```

### Building the Package

To build the package for distribution, use the following command:

```sh
poetry build
```

This will generate the distribution packages in the dist directory - but for now only @beveradb will be able to publish to PyPI.

## Contributing 🤝

Contributions are very much welcome! Please fork the repository and submit a pull request with your changes, and I'll try to review, merge and publish promptly!

- This project is 100% open-source and free for anyone to use and modify as they wish.
- If the maintenance workload for this repo somehow becomes too much for me I'll ask for volunteers to share maintainership of the repo, though I don't think that is very likely
- Development and support for the MDX-Net separation models is part of the main [UVR project](https://github.com/Anjok07/ultimatevocalremovergui), this repo is just a CLI/Python package wrapper to simplify running those models programmatically. So, if you want to try and improve the actual models, please get involved in the UVR project and look for guidance there!

## License 📄

This project is licensed under the MIT [License](LICENSE).

- **Please Note:** If you choose to integrate this project into some other project using the default model or any other model trained as part of the [UVR](https://github.com/Anjok07/ultimatevocalremovergui) project, please honor the MIT license by providing credit to UVR and its developers!

## Credits 🙏

- [Anjok07](https://github.com/Anjok07) - Author of [Ultimate Vocal Remover GUI](https://github.com/Anjok07/ultimatevocalremovergui), which almost all of the code in this repo was copied from! Definitely deserving of credit for anything good from this project. Thank you!
- [DilanBoskan](https://github.com/DilanBoskan) - Your contributions at the start of this project were essential to the success of UVR. Thank you!
- [Kuielab & Woosung Choi](https://github.com/kuielab) - Developed the original MDX-Net AI code.
- [KimberleyJSN](https://github.com/KimberleyJensen) - Advised and aided the implementation of the training scripts for MDX-Net and Demucs. Thank you!
- [Hv](https://github.com/NaJeongMo/Colab-for-MDX_B) - Helped implement chunks into the MDX-Net AI code. Thank you!
- [zhzhongshi](https://github.com/zhzhongshi) - Helped add support for the MDXC models in `audio-separator`. Thank you!

## Contact 💌

For questions or feedback, please raise an issue or reach out to @beveradb ([Andrew Beveridge](mailto:andrew@beveridge.uk)) directly.

## Sponsors

<!-- sponsors --><!-- sponsors -->
