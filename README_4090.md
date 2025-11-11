[中文](./README.md) | [English](./README_EN.md) | [模型列表](./models.md) | 本页面

<div align="center">

> 📚 **相关文档：** 查看 [完整模型列表](./models.md) 了解所有可用模型，或返回 [主文档](./README.md) 查看完整使用说明。

---

## 🧩 一、建议的 Conda 环境创建

```bash
conda create -n audio-separator python=3.12 -y
conda activate audio-separator
```

4090 属于 **Ada Lovelace 架构**，推荐使用 **CUDA 12.1 或 12.4** 的 PyTorch。

---

## ⚙️ 二、GPU 版本依赖安装

### ✅ 官方 PyTorch (CUDA 12.1) 安装

```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

---

### ✅ 安装 audio-separator + GPU 支持包

```bash
pip install "audio-separator[gpu]" onnxruntime-gpu==1.20.0
```

> 🔹 说明：
>
> * `onnxruntime-gpu==1.20.0` 支持 **Python 3.12** + **CUDA 12.1+**
> * `audio-separator[gpu]` 会自动拉取 **demucs / UVR 模型依赖**
> * 如果有 `torch` 版本冲突，请先装 torch，再装 audio-separator

---

## 🧱 三、额外工具依赖（建议装）

```bash
sudo apt install ffmpeg -y     # Linux
# or
brew install ffmpeg            # macOS
```

---

## 🧪 四、验证是否启用 GPU 加速

运行：

```bash
python -c "import torch;print(torch.cuda.is_available(), torch.cuda.get_device_name(0))"
```

应输出：

```
True NVIDIA GeForce RTX 4090
```

再测试 audio-separator：

```bash
audio-separator --env_info
```

如果显示：

```
ONNXruntime has CUDAExecutionProvider available
FFmpeg installed
```

✅ 表示 GPU 加速生效。

## 💡 小结

| 组件              | 版本            |
| --------------- | ------------- |
| Python          | 3.12 ✅        |
| CUDA            | 12.1 – 12.4 ✅ |
| GPU             | RTX 4090 ✅    |
| Torch           | 2.4+ ✅        |
| ONNXRuntime     | 1.20.0 ✅      |
| audio-separator | 最新 ✅          |

---

---

## ⚡ 五、完整使用示例

### 📋 步骤 1：查看和筛选可用模型

首先，查看所有可用的模型：

```bash
audio-separator --list_models
```

筛选出人声分离效果最好的前 5 个模型：

```bash
audio-separator --list_models --list_filter=vocals --list_limit=5
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

### 📥 步骤 2：下载模型（可选）

模型会在首次使用时自动下载，但你也可以提前下载：

```bash
# 下载单个模型（不执行分离）
audio-separator --download_model_only --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt

# 指定模型存储目录（默认：/tmp/audio-separator-models/）
audio-separator --download_model_only \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --model_file_dir ~/audio-separator-models
```

### 🎵 步骤 3：基础分离（人声 + 伴奏）

最简单的分离命令：

```bash
audio-separator song.mp3 --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt
```

这会在当前目录生成两个文件：
- `song_(Vocals)_model_bs_roformer_ep_317_sdr_12.9755.flac` - 人声
- `song_(Instrumental)_model_bs_roformer_ep_317_sdr_12.9755.flac` - 伴奏

### 🎯 步骤 4：高质量分离配置（推荐）

针对 RTX 4090 的高质量分离命令：

```bash
audio-separator song.mp3 \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --output_format flac \
  --output_dir ./output \
  --mdxc_batch_size 8 \
  --mdxc_segment_size 256 \
  --mdxc_overlap 8 \
  --use_autocast \
  --normalization 0.9
```

**参数说明：**
- `--model_filename`: 使用 SDR 12.9 的高质量模型
- `--output_format flac`: 无损格式，保证音质
- `--output_dir ./output`: 指定输出目录
- `--mdxc_batch_size 8`: 4090 的 24GB 显存可以支持批处理大小 8
- `--mdxc_segment_size 256`: 较大的分段大小，提高质量
- `--mdxc_overlap 8`: 适中的重叠，平衡质量和速度
- `--use_autocast`: 启用混合精度，加速推理
- `--normalization 0.9`: 归一化到 90% 峰值，避免削波

### 🎤 步骤 5：仅提取人声或仅提取伴奏

仅提取人声：

```bash
audio-separator song.mp3 \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --single_stem Vocals \
  --output_format flac \
  --output_dir ./output
```

仅提取伴奏：

```bash
audio-separator song.mp3 \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --single_stem Instrumental \
  --output_format flac \
  --output_dir ./output
```

### 🎸 步骤 6：分离更多声部（鼓、贝斯、吉他等）

使用 Demucs 模型可以分离更多声部：

```bash
# 查看支持多声部分离的模型
audio-separator --list_models --list_filter=drums --list_limit=3

# 使用 htdemucs_6s 模型分离 6 个声部
audio-separator song.mp3 \
  --model_filename htdemucs_6s.yaml \
  --output_format flac \
  --output_dir ./output \
  --demucs_segment_size 256 \
  --demucs_shifts 4 \
  --demucs_overlap 0.25 \
  --use_autocast
```

这会生成：
- `song_(Vocals).flac` - 人声
- `song_(Drums).flac` - 鼓
- `song_(Bass).flac` - 贝斯
- `song_(Guitar).flac` - 吉他
- `song_(Piano).flac` - 钢琴
- `song_(Other).flac` - 其他

### 📁 步骤 7：批量处理多个文件

处理整个文件夹：

```bash
audio-separator /path/to/audio/folder \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --output_format flac \
  --output_dir ./output \
  --mdxc_batch_size 8 \
  --use_autocast
```

### 🎨 步骤 8：自定义输出文件名

```bash
audio-separator song.mp3 \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --output_format flac \
  --custom_output_names '{"Vocals": "lead_vocals", "Instrumental": "backing_track"}' \
  --output_dir ./output
```

输出文件：
- `lead_vocals.flac` - 人声
- `backing_track.flac` - 伴奏

### ⚡ 步骤 9：极致质量模式（最慢但最好）

如果追求最高质量，可以使用以下参数：

```bash
audio-separator song.mp3 \
  --model_filename model_bs_roformer_ep_317_sdr_12.9755.ckpt \
  --output_format flac \
  --output_dir ./output \
  --mdxc_batch_size 4 \
  --mdxc_segment_size 512 \
  --mdxc_overlap 12 \
  --normalization 0.95 \
  --sample_rate 44100
```

**极致质量参数说明：**
- `--mdxc_segment_size 512`: 更大的分段，质量更高但更慢
- `--mdxc_overlap 12`: 更高的重叠，减少边界伪影
- `--mdxc_batch_size 4`: 降低批大小以容纳更大的分段
- `--normalization 0.95`: 更高的归一化阈值
- `--sample_rate 44100`: 保持原始采样率

### 📊 性能优化建议总结

| 设置                | 推荐值（4090）        | 说明                        |
| ----------------- | ---------------- | ------------------------- |
| 模型                | `model_bs_roformer_ep_317_sdr_12.9755.ckpt` | SDR 12.9，质量最佳           |
| 输出格式              | `--output_format flac` | 无损格式                      |
| MDXC 批处理大小         | `--mdxc_batch_size 8` | 24GB 显存可支持               |
| MDXC 分段大小          | `--mdxc_segment_size 256` | 平衡质量和速度（极致质量用 512）      |
| MDXC 重叠            | `--mdxc_overlap 8` | 平衡质量和速度（极致质量用 12）       |
| 混合精度加速            | `--use_autocast` | 4090 支持，可提升 30-50% 速度    |
| 归一化                | `--normalization 0.9` | 避免削波，保护音质                |

---



</div>
