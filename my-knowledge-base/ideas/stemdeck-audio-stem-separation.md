---
title: StemDeck：免費本機音訊分軌工具
date: 2026-06-25
tags: [tools/audio]
source: https://github.com/stemdeckapp/stemdeck
type: reference
status: processed
---

StemDeck 是開源的本機音訊6軌分離工具，對標 Moises/LALAL.AI 的免費替代方案，隱私優先（所有處理在本機完成）。

- 使用 Demucs `htdemucs_6s` 模型分離：人聲、鼓、貝斯、吉他、鋼琴、其他
- 支援 MP3/WAV/FLAC 匯入及 YouTube URL
- DAW 風格多軌介面（mute/solo/音量/VU meter/波形縮放）
- 自動偵測運算裝置（NVIDIA CUDA / Apple Silicon MPS / CPU fallback）
- 功能：BPM、key、LUFS、sample peak 分析；可匯出單軌或自訂 mix.wav
- 缺點：處理速度受硬體影響，GPU 較快

GitHub：https://github.com/stemdeckapp/stemdeck
DeTools 介紹：https://tools.wingzero.tw/article/sn/3958
