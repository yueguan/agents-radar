# Hugging Face 热门模型周报 2026-11-09

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-11-09 01:56 UTC

---

### **今日亮点**

2026年末，Hugging Face 生态系统由高影响力多模态与量化模型主导，其中 *Lightricks/LTX-2.5* 以超过 260 万次下载位居榜首，凸显图像转视频生成技术的爆发式增长。通义家族持续领跑，尤其在微调与量化版本中表现突出——*Qwen/Qwen3.8-27B* 与 *unsloth/Qwen3.8-Flash-Next-GGUF* 在点赞数和下载量上均名列前茅。值得注意的是，“无审查”与“异端”变体正大量涌现，反映出社区对不受限模型的强烈需求。与此同时，开源视觉模型如 *facebook/sam3* 与 *PrunaAI/P-H3-Image-Comfy* 也彰显出在可访问、高质量图像与掩码生成领域的强劲势头。

---

### **热门模型**

#### 🧠 语言模型（LLM、聊天模型、指令微调）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 19,419 | 7,668,187 | 一款强大的多模态 LLM，专为图文理解优化；其庞大的下载量反映了其在生产环境与科研中的广泛应用。 |
| [DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-...](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) | DavidAU | 2,711 | 1,553,252 | 一款高度优化的无审查 GGUF 版本，专为本地推理与代码生成设计——是追求性能与自由度的开发者的理想选择。 |
| [unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF) | unsloth | 1,537 | 1,548,621 | 基于 Unsloth 优化推理栈构建，以速度与效率为核心；作为最流行的 GGUF 模型之一，广泛用于低延迟部署。 |
| [LiquidAI/LFM3-1.7B](https://huggingface.co/LiquidAI/LFM3-1.7B) | LiquidAI | 1,558 | 73,562 | 一款轻量但功能强大的文本生成模型，专为边缘设备部署设计；在爱好者与移动 AI 开发者中迅速获得关注。 |

#### 🎨 多模态与生成模型（图像、视频、音频、文本转 X）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 11,137 | 2,661,026 | 业界领先的图像转视频模型，具备无缝运动合成能力；超 260 万次下载表明其已成为创作者与内容工作室的首选。 |
| [black-forest-labs/flux-3-image-klein-comfy](https://huggingface.co/black-forest-labs/flux-3-image-klein-comfy) | black-forest-labs | 2,761 | 1,167,959 | Flux 3 系列的一部分，兼容 ComfyUI，擅长图像到图像编辑，细节丰富且艺术控制力强——在创意工作流中广受欢迎。 |
| [abenzerps/Qwen-Image-2.1-Turbo-Quantized](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized) | abenzerps | 5,726 | 76,804 | Qwen Image 2.1 的量化版，已适配 ComfyUI，实现快速、高保真文本转图像生成——适合本地推理场景。 |
| [facebook/sam3](https://huggingface.co/facebook/sam3) | facebook | 6,111 | 1,982,669 | SAM 系列最新成员，支持视频与复杂场景的高级掩码生成；近 200 万次下载反映出其在计算机视觉流水线中的深度集成。 |

#### 🔧 专用模型（代码、数学、医疗、嵌入）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [ccore/Qwen3.5-4B-3D-Coder](https://huggingface.co/ccore/Qwen3.5-4B-3D-Coder) | ccore | 921 | 254,053 | 一款紧凑的代码导向模型，可从文本提示生成 Three.js 3D 资产——非常适合构建交互式 3D 体验的网页开发者。 |
| [ccore/Qwen3.5-4B-GLB-3D-Coder](https://huggingface.co/ccore/Qwen3.5-4B-GLB-3D-Coder) | ccore | 275 | 33,641 | 可通过代码指令直接生成 GLB 文件——面向游戏开发者与 AR/VR 创作者，提供高效的资产管线支持。 |
| [nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B) | nvidia | 3,107 | 67,230 | 高精度语音转文本模型，专为实时转录优化；属于 NVIDIA Nemo 生态系统，广泛应用于企业级语音系统。 |

#### 📦 微调与量化模型（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [xtools-at/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic](https://huggingface.co/xtools-at/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic) | xtools-at | 855 | 7,088 | 一款激进的无审查 DeepSeek 与 Qwen3.5 融合模型——吸引希望在提示处理中实现最大自由度的用户。 |
| [AtomicChat/DeepSeek-V4.1-Flash-1017-Uncensored-GSQ-RCO-GGUF](https://huggingface.co/AtomicChat/DeepSeek-V4.1-Flash-1017-Uncensored-GSQ-RCO-GGUF) | AtomicChat | 391 | 130,020 | 一款经 GGUF 优化的无审查 DeepSeek-V4.1 Flash 版本——深受自托管用户欢迎，兼顾性能与最小限制。 |
| [mradermacher/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic-i1-GGUF](https://huggingface.co/mradermacher/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic-i1-GGUF) | mradermacher | 187 | 136,706 | 另一款社区驱动的 i1 优化 GGUF 变体——展现了细分调优层如何逐渐成为本地 AI 堆栈的标准配置。 |
| [Scantist/GLM-5.3-SLIM-GGUF](https://huggingface.co/Scantist/GLM-5.3-SLIM-GGUF) | Scantist | 1,292 | 4,938 | 采用神经架构搜索构建的精简版 MoE-GGUF 模型——体现了人们对高效、可扩展推理日益增长的兴趣。 |

---

### **生态信号**

2026年末的 Hugging Face 生态系统清晰地呈现出向**开放权重、社区驱动创新**的转变，尤其体现在通义、深度求索与 GLM 系列模型中。这些模型家族如今在流行度与使用率上占据主导地位，其分支、量化版本与微调模型在 GGUF、safetensors 以及自定义格式间广泛传播。特别是通义与深度求索生态中“异端”与“无审查”变体的兴起，表明用户对无过滤、可定制化模型的需求强烈，这些模型通常通过 llama.cpp 或 vLLM 在本地部署。量化技术仍居核心地位：GGUF 已成为本地推理的事实标准，而 Unsloth、GSQ 与 RCO 等工具则进一步实现了更快、更低内存占用的运行。与此同时，多模态模型如 LTX-2.5 与 flux-3 正推动应用从研究走向创作生产、视频编辑与实时场景。这表明生态系统正在成熟，用户更重视可及性、性能与定制化，而非单纯的模型规模。

---

### **值得探索**

1. **[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** – 下载量超 260 万次，该图像转视频模型已成为生成视频创作的标杆。其从静态图像生成流畅连贯序列的能力，使其成为内容创作者、电影制作人与社交媒体专业人士的必备工具。

2. **[unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF)** – 当前最快速、最高效的 GGUF 模型之一，特别适合在消费级硬件上部署 LLM。其速度、低内存占用与高质量输出的结合，重新定义了本地推理的标准。

3. **[abenzerps/Qwen-Image-2.1-Turbo-Quantized](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized)** – 一款高度优化、原生适配 ComfyUI 的文本转图像模型，在质量和速度之间取得良好平衡。对于希望摆脱云依赖、实现快速迭代的艺术家与设计师而言，是理想之选。

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*