# Audio Separator 模型列表与说明

**相关文档：** [中文 README](./README.md) | [English README](./README_EN.md) | [RTX 4090 专用指南](./README_4090.md)

本文档详细列出了 audio-separator 支持的所有模型，按架构分类，并说明每个系列和型号的特点。

---

## 📋 目录

- [VR 架构模型](#vr-架构模型)
- [MDX 架构模型](#mdx-架构模型)
- [Demucs 架构模型](#demucs-架构模型)
- [MDXC 架构模型](#mdxc-架构模型)

---

## 🎯 VR 架构模型

**架构特点：** VR (Vocal Remover) 架构是较早的模型架构，主要用于人声/伴奏分离。模型文件格式为 `.pth`，处理速度较快，但质量相对较新架构略低。

### VR Arch Single Model v5 系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| [`1_HP-UVR.pth`](#1_hp-uvrpth) | 伴奏* | 7.9 | **13.7** | HP (High Performance) 系列，专注高质量伴奏提取 |
| [`2_HP-UVR.pth`](#2_hp-uvrpth) | 伴奏* | 8.2 | **13.5** | HP 系列变体，伴奏质量略低但人声保留更好 |
| [`3_HP-Vocal-UVR.pth`](#3_hp-vocal-uvrpth) | 人声* | **8.2** | 14.0 | HP Vocal 系列，专注人声提取，伴奏质量高 |
| [`4_HP-Vocal-UVR.pth`](#4_hp-vocal-uvrpth) | 人声* | **8.3** | 13.6 | HP Vocal 系列改进版，人声质量最佳 |
| [`5_HP-Karaoke-UVR.pth`](#5_hp-karaoke-uvrpth) | 伴奏* | 5.2 | **12.2** | 卡拉 OK 专用，强去除人声，适合制作伴奏 |
| [`6_HP-Karaoke-UVR.pth`](#6_hp-karaoke-uvrpth) | 伴奏* | 4.6 | **13.0** | 卡拉 OK 改进版，伴奏质量更高 |
| [`7_HP2-UVR.pth`](#7_hp2-uvrpth) | 伴奏* | 8.3 | **13.5** | HP2 系列，HP 的升级版本 |
| [`8_HP2-UVR.pth`](#8_hp2-uvrpth) | 伴奏* | 8.2 | **13.5** | HP2 系列变体 |
| [`9_HP2-UVR.pth`](#9_hp2-uvrpth) | 伴奏* | 8.0 | **13.7** | HP2 系列最佳伴奏质量 |
| [`10_SP-UVR-2B-32000-1.pth`](#10_sp-uvr-2b-32000-1pth) | 伴奏* | 7.5 | **13.3** | SP (Standard Performance) 系列，2B 参数，32kHz |
| [`11_SP-UVR-2B-32000-2.pth`](#11_sp-uvr-2b-32000-2pth) | 伴奏* | 7.3 | **13.8** | SP 系列，伴奏质量最佳 |
| [`12_SP-UVR-3B-44100.pth`](#12_sp-uvr-3b-44100pth) | 伴奏* | 7.5 | **13.1** | SP 系列，3B 参数，44.1kHz |
| [`13_SP-UVR-4B-44100-1.pth`](#13_sp-uvr-4b-44100-1pth) | 伴奏* | 7.8 | **13.3** | SP 系列，4B 参数 |
| [`14_SP-UVR-4B-44100-2.pth`](#14_sp-uvr-4b-44100-2pth) | 伴奏* | 8.0 | **13.5** | SP 系列，4B 参数改进版 |
| [`15_SP-UVR-MID-44100-1.pth`](#15_sp-uvr-mid-44100-1pth) | 伴奏* | 7.5 | **13.1** | SP MID 系列，中等参数配置 |
| [`16_SP-UVR-MID-44100-2.pth`](#16_sp-uvr-mid-44100-2pth) | 伴奏* | 7.4 | **13.3** | SP MID 系列改进版 |
| [`17_HP-Wind_Inst-UVR.pth`](#17_hp-wind_inst-uvrpth) | 无木管* | - | - | 特殊用途：分离木管乐器 |

**系列说明：**
- **HP (High Performance) 系列**：高性能模型，适合追求高质量分离的用户
- **HP2 系列**：HP 的升级版本，在保持高质量的同时优化了性能
- **SP (Standard Performance) 系列**：标准性能模型，平衡质量和速度
- **Karaoke 系列**：专门为制作卡拉 OK 伴奏设计，强去除人声

### VR 音频处理系列

| 模型文件名 | 主要输出 | 特点说明 |
|-----------|---------|---------|
| `UVR-De-Echo-Aggressive.pth` | 无回声* | 激进模式去回声，适合严重回声的音频 |
| `UVR-De-Echo-Normal.pth` | 无回声* | 标准模式去回声，平衡效果和音质 |
| `UVR-DeEcho-DeReverb.pth` | 无混响* | 同时去除回声和混响 |
| `UVR-DeNoise-Lite.pth` | 噪声* | 轻量级去噪，保留更多细节 |
| `UVR-DeNoise.pth` | 噪声* | 标准去噪，平衡效果和音质 |
| `UVR-De-Reverb-aufr33-jarredou.pth` | 干声* | 去除混响，保留干声信号 |

### VR Arch Single Model v4 系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `MGM_HIGHEND_v4.pth` | 伴奏* | 6.9 | **12.3** | MGM 系列，专注高频处理 |
| `MGM_LOWEND_A_v4.pth` | 伴奏* | 7.0 | **13.0** | MGM 系列，专注低频处理 A 版 |
| `MGM_LOWEND_B_v4.pth` | 伴奏* | 7.5 | **13.1** | MGM 系列，专注低频处理 B 版 |
| `MGM_MAIN_v4.pth` | 伴奏* | 6.2 | **12.4** | MGM 主模型，平衡处理 |

---

## 🎵 MDX 架构模型

**架构特点：** MDX-Net 是较新的架构，使用 ONNX 格式（`.onnx`），质量比 VR 架构更高，SDR 分数通常在 9-15 之间。支持更精细的声部分离。

### MDX-Net Inst HQ 系列（高质量伴奏）

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `UVR-MDX-NET-Inst_HQ_1.onnx` | 伴奏* | 8.8 | **15.4** | HQ 系列，高质量伴奏提取 |
| `UVR-MDX-NET-Inst_HQ_2.onnx` | 伴奏* | 8.8 | **15.3** | HQ 系列变体 |
| `UVR-MDX-NET-Inst_HQ_3.onnx` | 伴奏* | 8.8 | **15.4** | HQ 系列变体 |
| `UVR-MDX-NET-Inst_HQ_4.onnx` | 伴奏* | 8.8 | **15.5** | HQ 系列最佳伴奏质量 ⭐ |
| `UVR-MDX-NET-Inst_HQ_5.onnx` | 伴奏* | 8.7 | **15.3** | HQ 系列变体 |

**推荐：** `UVR-MDX-NET-Inst_HQ_4.onnx` - 伴奏质量最高（SDR 15.5）

### MDX-Net Main 系列（人声/伴奏平衡）

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `UVR_MDXNET_Main.onnx` | 人声* | **10.2** | 15.4 | Main 系列，人声质量优秀 |
| `UVR-MDX-NET-Inst_Main.onnx` | 伴奏* | 8.5 | **15.1** | Main 系列，专注伴奏 |
| `UVR-MDX-NET-Voc_FT.onnx` | 人声* | **10.2** | 15.4 | Fine-Tuned 版本，人声质量优秀 |

### MDX-Net 基础系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `UVR_MDXNET_1_9703.onnx` | 人声* | **9.6** | 15.0 | 基础系列 1 |
| `UVR_MDXNET_2_9682.onnx` | 人声* | **9.4** | 15.0 | 基础系列 2 |
| `UVR_MDXNET_3_9662.onnx` | 人声* | **9.7** | 15.0 | 基础系列 3，人声质量最佳 |
| `UVR-MDX-NET-Inst_1.onnx` | 伴奏* | 9.2 | **15.2** | Inst 系列 1 |
| `UVR-MDX-NET-Inst_2.onnx` | 伴奏* | 9.2 | **15.3** | Inst 系列 2 |
| `UVR-MDX-NET-Inst_3.onnx` | 伴奏* | 9.2 | **15.3** | Inst 系列 3 |

### MDX-Net Karaoke 系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `UVR_MDXNET_KARA.onnx` | 人声* | **5.6** | 14.1 | 卡拉 OK 专用，强去除人声 |
| `UVR_MDXNET_KARA_2.onnx` | 伴奏* | 5.4 | **14.8** | 卡拉 OK 改进版，伴奏质量更高 ⭐ |

### MDX-Net 特殊用途系列

| 模型文件名 | 主要输出 | 特点说明 |
|-----------|---------|---------|
| `Reverb_HQ_By_FoxJoy.onnx` | 混响* | 高质量混响分离 |
| `UVR-MDX-NET_Crowd_HQ_1.onnx` | 无人群* | 去除人群噪音 |
| `UVR_MDXNET_9482.onnx` | 人声* | 特殊训练版本 |

### MDX-Net Kim 系列（Kimberley Jensen 训练）

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `Kim_Vocal_1.onnx` | 人声* | **10.1** | 15.5 | Kim 系列，人声质量优秀 |
| `Kim_Vocal_2.onnx` | 人声* | **10.2** | 15.4 | Kim 系列改进版 ⭐ |
| `Kim_Inst.onnx` | 伴奏* | 9.1 | **15.5** | Kim 系列，伴奏质量最高 ⭐ |

### MDX-Net Kuielab 系列（多声部分离）

| 模型文件名 | 主要输出 | SDR | 特点说明 |
|-----------|---------|-----|---------|
| `kuielab_a_vocals.onnx` | 人声* | 9.6 | Kuielab A 系列，人声分离 |
| `kuielab_a_other.onnx` | 其他* | - | Kuielab A 系列，其他声部 |
| `kuielab_a_bass.onnx` | 贝斯* | **10.4** | Kuielab A 系列，贝斯分离 ⭐ |
| `kuielab_a_drums.onnx` | 鼓* | 7.0 | Kuielab A 系列，鼓分离 |
| `kuielab_b_vocals.onnx` | 人声* | 9.0 | Kuielab B 系列，人声分离 |
| `kuielab_b_other.onnx` | 其他* | - | Kuielab B 系列，其他声部 |
| `kuielab_b_bass.onnx` | 贝斯* | 9.9 | Kuielab B 系列，贝斯分离 |
| `kuielab_b_drums.onnx` | 鼓* | 7.1 | Kuielab B 系列，鼓分离 |

**系列说明：** Kuielab 系列支持多声部分离，可以分别提取人声、贝斯、鼓和其他声部。

### MDX-Net VIP 系列（高级模型）

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `UVR-MDX-NET_Main_340.onnx` | 人声* | **10.2** | 15.4 | VIP 系列，Main 340 |
| `UVR-MDX-NET_Main_390.onnx` | 人声* | **4.5** | 8.8 | VIP 系列，Main 390（特殊用途） |
| `UVR-MDX-NET_Main_406.onnx` | 人声* | **10.4** | 15.3 | VIP 系列，Main 406，人声质量最佳 ⭐ |
| `UVR-MDX-NET_Main_427.onnx` | 人声* | **10.2** | 15.5 | VIP 系列，Main 427，伴奏质量最高 ⭐ |
| `UVR-MDX-NET_Main_438.onnx` | 人声* | **10.1** | 15.3 | VIP 系列，Main 438 |
| `UVR-MDX-NET_Inst_82_beta.onnx` | 伴奏* | 8.2 | **14.3** | VIP 系列，Inst Beta 82 |
| `UVR-MDX-NET_Inst_90_beta.onnx` | 伴奏* | 8.1 | **13.9** | VIP 系列，Inst Beta 90 |
| `UVR-MDX-NET_Inst_187_beta.onnx` | 伴奏* | 8.5 | **14.3** | VIP 系列，Inst Beta 187 |
| `UVR-MDX-NET-Inst_full_292.onnx` | 伴奏* | 8.5 | **15.1** | VIP 系列，Inst Full 292 ⭐ |

---

## 🥁 Demucs 架构模型

**架构特点：** Demucs v4 是 Meta（Facebook）开发的模型，支持多声部分离（人声、鼓、贝斯、其他等），模型文件格式为 `.yaml`。质量优秀，特别适合需要分离多个声部的场景。

| 模型文件名 | 人声 SDR | 鼓 SDR | 贝斯 SDR | 其他声部 | 特点说明 |
|-----------|---------|--------|---------|---------|---------|
| `htdemucs_ft.yaml` | **10.8** | 10.0 | **12.0** | ✅ | Fine-Tuned 版本，贝斯质量最佳 ⭐ |
| `htdemucs.yaml` | 9.9 | 9.4 | 11.6 | ✅ | 标准版本，平衡各声部质量 |
| `hdemucs_mmi.yaml` | 10.2 | 9.6 | **12.2** | ✅ | MMI 版本，贝斯质量最高 ⭐ |
| `htdemucs_6s.yaml` | 9.6 | 8.5 | 10.1 | ✅ + 吉他 + 钢琴 | 6 声部版本，额外支持吉他和钢琴分离 ⭐ |

**系列说明：**
- **htdemucs**：Hybrid Transformer Demucs，使用 Transformer 架构
- **hdemucs**：Hybrid Demucs，混合架构
- **6s**：6 stems，支持分离 6 个声部（人声、鼓、贝斯、吉他、钢琴、其他）

**推荐使用场景：**
- 需要分离多个声部：使用 `htdemucs_6s.yaml`
- 追求贝斯质量：使用 `hdemucs_mmi.yaml`
- 平衡各声部质量：使用 `htdemucs_ft.yaml`

---

## 🚀 MDXC 架构模型

**架构特点：** MDXC（MDX23C）是最新的架构，使用 `.ckpt` 格式，质量最高。包含 Roformer 系列模型，SDR 分数通常在 10-17 之间，是目前质量最好的模型架构。

### MDX23C 基础系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `MDX23C-8KFFT-InstVoc_HQ.ckpt` | 人声 | **10.6** | **15.8** | MDX23C HQ 版本，平衡质量 ⭐ |
| `MDX23C_D1581.ckpt` | 人声 | **10.0** | 15.5 | MDX23C D1581 版本 |
| `MDX23C-8KFFT-InstVoc_HQ_2.ckpt` | 人声 | **10.5** | **15.9** | MDX23C HQ 2，伴奏质量最高 ⭐ |

### BS-Roformer 系列（最佳人声质量）

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `model_bs_roformer_ep_317_sdr_12.9755.ckpt` | 人声* | **12.9755** | 16.5 | BS-Roformer 317，人声质量极高 ⭐⭐⭐ |
| `model_bs_roformer_ep_368_sdr_12.9628.ckpt` | 人声* | **12.9628** | 16.3 | BS-Roformer 368，人声质量极高 ⭐⭐⭐ |
| `model_bs_roformer_ep_937_sdr_10.5309.ckpt` | 无鼓贝斯* | - | - | 特殊用途：分离鼓和贝斯 |

**推荐：** `model_bs_roformer_ep_317_sdr_12.9755.ckpt` - **目前人声分离质量最高的模型**（SDR 12.9755）

### Mel-Roformer 系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `model_mel_band_roformer_ep_3005_sdr_11.4360.ckpt` | 人声* | **11.4360** | 15.1 | Mel-Roformer 基础版本 |
| `vocals_mel_band_roformer.ckpt` | 人声* | **12.6** | - | Kimberley Jensen 训练，人声质量优秀 ⭐⭐ |
| `mel_band_roformer_kim_ft_unwa.ckpt` | 人声* | **12.4** | - | Kim Fine-Tuned 版本 ⭐ |

### MelBand Roformer Inst 系列（高质量伴奏）

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `melband_roformer_inst_v1.ckpt` | 伴奏* | 9.8 | **15.9** | Inst V1，伴奏质量优秀 ⭐ |
| `melband_roformer_inst_v2.ckpt` | 伴奏* | 10.3 | **16.1** | Inst V2，伴奏质量最高 ⭐⭐ |
| `melband_roformer_inst_v1e.ckpt` | 伴奏* | 9.6 | **15.8** | Inst V1 (E) 版本 |
| `melband_roformer_instvoc_duality_v1.ckpt` | 人声 | **11.0** | **16.1** | Duality V1，平衡人声和伴奏 ⭐ |
| `melband_roformer_instvox_duality_v2.ckpt` | 人声 | **11.0** | **16.1** | Duality V2，平衡人声和伴奏 ⭐ |

### MelBand Roformer Big 系列（大模型）

| 模型文件名 | 主要输出 | 人声 SDR | 特点说明 |
|-----------|---------|---------|---------|
| `MelBandRoformerBigSYHFTV1.ckpt` | 人声* | **12.3** | Big SYHFT V1，大模型高质量 ⭐⭐ |
| `melband_roformer_big_beta4.ckpt` | 人声* | **12.5** | Big Beta 4，大模型高质量 ⭐⭐ |
| `melband_roformer_big_beta5e.ckpt` | 人声* | **12.4** | Big Beta 5e，大模型高质量 ⭐⭐ |

### MelBand Roformer SYHFT 系列

| 模型文件名 | 主要输出 | 人声 SDR | 特点说明 |
|-----------|---------|---------|---------|
| `MelBandRoformerSYHFT.ckpt` | 人声* | **8.0** | SYHFT 基础版本 |
| `MelBandRoformerSYHFTV2.ckpt` | 人声* | **8.6** | SYHFT V2 |
| `MelBandRoformerSYHFTV2.5.ckpt` | 人声* | **8.5** | SYHFT V2.5 |
| `MelBandRoformerSYHFTV3Epsilon.ckpt` | 人声* | **9.5** | SYHFT V3，质量最佳 ⭐ |

### MDXC 特殊用途系列

#### 去混响系列

| 模型文件名 | 主要输出 | SDR | 特点说明 |
|-----------|---------|-----|---------|
| `MDX23C-De-Reverb-aufr33-jarredou.ckpt` | 干声 | - | MDX23C 去混响 |
| `deverb_bs_roformer_8_384dim_10depth.ckpt` | 无混响* | - | BS-Roformer 去混响 |
| `dereverb_mel_band_roformer_anvuew_sdr_19.1729.ckpt` | 无混响* | **19.17** | 高质量去混响 ⭐⭐ |
| `dereverb_mel_band_roformer_less_aggressive_anvuew_sdr_18.8050.ckpt` | 无混响* | **18.81** | 温和去混响 ⭐ |
| `dereverb-echo_mel_band_roformer_sdr_10.0169.ckpt` | 干声 | 10.02 | 去混响+去回声 |
| `dereverb-echo_mel_band_roformer_sdr_13.4843_v2.ckpt` | 干声* | **13.48** | 去混响+去回声 V2 ⭐ |

#### 去噪系列

| 模型文件名 | 主要输出 | SDR | 特点说明 |
|-----------|---------|-----|---------|
| `denoise_mel_band_roformer_aufr33_sdr_27.9959.ckpt` | 干声* | **27.99** | 高质量去噪 ⭐⭐⭐ |
| `denoise_mel_band_roformer_aufr33_aggr_sdr_27.9768.ckpt` | 干声* | **27.98** | 激进模式去噪 ⭐⭐⭐ |

#### 卡拉 OK 系列

| 模型文件名 | 主要输出 | 人声 SDR | 伴奏 SDR | 特点说明 |
|-----------|---------|---------|---------|---------|
| `mel_band_roformer_karaoke_aufr33_viperx_sdr_10.1956.ckpt` | 人声* | **8.4** | 14.7 | 卡拉 OK 专用 ⭐ |

#### 其他特殊用途

| 模型文件名 | 主要输出 | 特点说明 |
|-----------|---------|---------|
| `MDX23C-DrumSep-aufr33-jarredou.ckpt` | 6 种鼓 | 分离 6 种鼓：kick, snare, toms, hh, ride, crash ⭐ |
| `mel_band_roformer_crowd_aufr33_viperx_sdr_8.7144.ckpt` | 人群* | 去除人群噪音 |
| `model_chorus_bs_roformer_ep_267_sdr_24.1275.ckpt` | 男声/女声 | 分离男声和女声 ⭐ |
| `aspiration_mel_band_roformer_sdr_18.9845.ckpt` | 呼吸声 | 分离呼吸声和其他声音 |
| `mel_band_roformer_bleed_suppressor_v1.ckpt` | 伴奏* | 抑制人声泄漏到伴奏中 |

### Gabox 系列（大量变体）

Gabox 训练了大量 MelBand Roformer 变体，包括多个版本的 Vocals 和 Instrumental 模型。这些模型的特点和 SDR 分数未完全公开，建议通过实际测试选择最适合的版本。

**Vocals 系列：**
- `mel_band_roformer_vocals_gabox.ckpt`
- `mel_band_roformer_vocals_v2_gabox.ckpt`
- `mel_band_roformer_vocals_fv1_gabox.ckpt` 到 `fv6_gabox.ckpt`（多个版本）

**Instrumental 系列：**
- `mel_band_roformer_instrumental_gabox.ckpt`
- `mel_band_roformer_instrumental_2_gabox.ckpt`
- `mel_band_roformer_instrumental_3_gabox.ckpt`
- `mel_band_roformer_instrumental_bleedless_v1/v2/v3_gabox.ckpt`（无泄漏版本）
- `mel_band_roformer_instrumental_fullness_v1/v2/v3_gabox.ckpt`（饱满度版本）
- `mel_band_roformer_instrumental_instv5/v6/v7/v8_gabox.ckpt`（INSTV 系列）

---

## 📊 模型选择指南

### 按用途选择

| 用途 | 推荐模型 | 理由 |
|------|---------|------|
| **人声分离（最高质量）** | `model_bs_roformer_ep_317_sdr_12.9755.ckpt` | SDR 12.9755，目前最高 |
| **伴奏分离（最高质量）** | `melband_roformer_inst_v2.ckpt` | SDR 16.1，伴奏质量最高 |
| **平衡人声和伴奏** | `MDX23C-8KFFT-InstVoc_HQ_2.ckpt` | 人声 10.5，伴奏 15.9 |
| **多声部分离** | `htdemucs_6s.yaml` | 支持 6 个声部（人声、鼓、贝斯、吉他、钢琴、其他） |
| **卡拉 OK 伴奏制作** | `UVR_MDXNET_KARA_2.onnx` | 强去除人声，伴奏 SDR 14.8 |
| **去噪** | `denoise_mel_band_roformer_aufr33_sdr_27.9959.ckpt` | SDR 27.99，去噪效果极佳 |
| **去混响** | `dereverb_mel_band_roformer_anvuew_sdr_19.1729.ckpt` | SDR 19.17，去混响效果优秀 |
| **鼓分离** | `MDX23C-DrumSep-aufr33-jarredou.ckpt` | 分离 6 种鼓声部 |

### 按架构选择

| 架构 | 特点 | 适用场景 |
|------|------|---------|
| **VR** | 速度快，质量中等 | 快速处理，对质量要求不高 |
| **MDX** | 质量高，速度中等 | 平衡质量和速度 |
| **Demucs** | 多声部分离，质量优秀 | 需要分离多个声部 |
| **MDXC** | 质量最高，速度较慢 | 追求最高质量 |

### SDR 分数说明

- **SDR (Signal-to-Distortion Ratio)**：信号失真比，分数越高表示分离质量越好
- **10+**：优秀质量
- **12+**：极高质量
- **15+**：顶级质量（通常用于伴奏）
- **20+**：专业级质量（通常用于去噪、去混响等特殊处理）

---

## 💡 使用建议

1. **首次使用**：建议从 `model_bs_roformer_ep_317_sdr_12.9755.ckpt` 开始，这是目前人声分离质量最高的模型
2. **批量处理**：如果处理大量文件，可以考虑使用 MDX 架构模型，平衡质量和速度
3. **多声部分离**：使用 Demucs 模型（如 `htdemucs_6s.yaml`）
4. **特殊处理**：根据需求选择对应的特殊用途模型（去噪、去混响等）

---

*最后更新：基于 audio-separator 最新模型列表*
