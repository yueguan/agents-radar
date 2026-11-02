# Hugging Face 热门模型周报 2026-11-02

> 数据来源: [Hugging Face Hub](https://huggingface.co/) | 共 30 个模型 | 生成时间: 2026-11-02 01:47 UTC

---

### **今日亮点**

2026年末，Hugging Face 的模型生态以多模态生成的爆发式增长和超高效、量化模型的兴起为特征。表现最突出的是 **Qwen/Qwen3.8-27B**，其下载量近 800 万次，点赞数超过 1.8 万，巩固了 Qwen 作为领先开源多模态架构的地位。与此同时，**Lightricks/LTX-2.5** 以 240 万次下载打破纪录，标志着图像转视频合成在社区中的广泛采纳。值得注意的是，LiquidAI 与 ISTA-DASLab 推出的 GGUF 量化模型正推动模型可及性提升，而 NVIDIA 与百度则持续在语音和 OCR 专业化领域拓展边界。

---

### **热门模型**

#### 🧠 语言模型（LLM、聊天模型、指令微调）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [deepseek-ai/DeepSeek-V4.1-Flash-1017](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017) | deepseek-ai | 4,314 | 170,353 | DeepSeek-V4.1 的高性能 Flash 变体，针对速度与效率优化，广泛用于实时推理场景。 |
| [mistralai/Mistral-Large-4-1T-A52B](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B) | mistralai | 1,452 | 4,576 | Mistral 最新大型模型，具备先进推理能力，尽管下载量较低，但在研究人员中迅速获得关注。 |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 18,929 | 7,892,866 | Hugging Face 上下载量最高的模型，融合强大多模态理解能力与开放权重灵活性，适用于企业级与科研用途。 |
| [RecursiveSI/flint](https://huggingface.co/RecursiveSI/flint) | RecursiveSI | 1,294 | 26,703 | 轻量但功能强大的对话式 LLM，专为对话效率训练，深受边缘部署场景欢迎。 |
| [XHToken/Spark-X2.6-4B](https://huggingface.co/XHToken/Spark-X2.6-4B) | XHToken | 947 | 16,707 | 基于 Spark 架构的紧凑型快速推理模型，适用于移动端与低延迟应用。 |

#### 🎨 多模态与生成（图像、视频、音频、文本转多模态）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 10,226 | 2,431,846 | 革命性的图像转视频模型，可从单张图像生成电影级画面；本月 Hugging Face 上增长最快的模型。 |
| [black-forest-labs/flux-3-image-klein-comfy](https://huggingface.co/black-forest-labs/flux-3-image-klein-comfy) | black-forest-labs | 2,267 | 970,633 | Flux-3 的高度高效 ComfyUI 兼容版本，专为大规模图像编辑与生成优化。 |
| [abenzerps/Qwen-Image-2.1-Uncensored-Quantized](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-Quantized) | abenzerps | 5,474 | 2,548,468 | 本地运行的 Qwen Image 2.1 未审查量化版（GGUF），在私有 AI 艺术工作流中广受欢迎。 |
| [facebook/sam3](https://huggingface.co/facebook/sam3) | facebook | 5,635 | 1,891,055 | SAM3 实现顶尖掩码生成能力并支持视频输入，广泛应用于各行业的分割流水线。 |
| [baidu/Unlimited-OCR-2](https://huggingface.co/baidu/Unlimited-OCR-2) | baidu | 520 | 109,519 | 超高可扩展性 OCR 模型，支持多语言文档解析且预处理极简——企业数字化的关键工具。 |

#### 🔧 专用模型（代码、数学、医疗、嵌入）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [ccore/Qwen3.5-4B-3D-Coder](https://huggingface.co/ccore/Qwen3.5-4B-3D-Coder) | ccore | 483 | 44,825 | 针对 Three.js 与 3D 网页开发的专用代码生成器，助力交互式图形快速原型设计。 |
| [burner22/numbat](https://huggingface.co/burner22/numbat) | burner22 | 202 | 5,846 | 通过 ModelOpt 优化的定制 8 位模型，专为低内存环境设计，在数值任务中保持高精度。 |
| [openbmb/MiniCPM-OCR-1.0](https://huggingface.co/openbmb/MiniCPM-OCR-1.0) | openbmb | 383 | 3,513 | 轻量且高精度的 OCR 模型，专为资源受限环境下的文档解析打造。 |

#### 📦 微调与量化（社区微调、GGUF、AWQ）

| 模型 | 作者 | 点赞数 | 下载量 | 简述 |
| :--- | :--- | ---: | ---: | :--- |
| [ISTA-DASLab/DeepSeek-V4.1-Flash-1017-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/DeepSeek-V4.1-Flash-1017-GSQ-RCO-GGUF) | ISTA-DASLab | 534 | 2,294,642 | Hugging Face 上下载量最高的 DeepSeek-V4.1 GGUF 版本之一，采用 GSQ+RCO 量化技术，实现最佳 CPU 性能。 |
| [AtomicChat/LFM3-1.7B-Uncensored-GGUF](https://huggingface.co/AtomicChat/LFM3-1.7B-Uncensored-GGUF) | AtomicChat | 177 | 46,155 | 完全无审查的 GGUF 量化版 LFM3-1.7B 模型，适合无限制的创意写作与角色扮演。 |
| [LiquidAI/LFM3-1.7B-GGUF](https://huggingface.co/LiquidAI/LFM3-1.7B-GGUF) | LiquidAI | 172 | 150,260 | 广泛采用的 LFM3-1.7B GGUF 变体，因其在本地硬件上的尺寸与性能平衡而备受赞誉。 |
| [OliviaRossi/Minimax-h3_Singularity-x-10Eros-Max_PRISM](https://huggingface.co/OliviaRossi/Minimax-h3_Singularity-x-10Eros-Max_PRISM) | OliviaRossi | 180 | 167,612 | 高端合并模型，融合 Minimax-H3 与 TenStrip 10Eros-Max 基础模型，视频生成输出质量达到顶级水平。 |

---

### **生态信号**

2026 年末的 Hugging Face 生态系统清晰地呈现出向 **效率、可及性与多模态融合** 的转型。尤其是来自 **Qwen、DeepSeek 与 LiquidAI** 的开源权重模型，在下载量与用户互动方面占据主导地位，进一步强化了由社区驱动的创新势头。尤为关键的是，**基于 GGUF 的量化模型** 已成为本地部署的默认标准，例如 ISTA-DASLab 提供的 DeepSeek 变体已突破 220 万次下载。这一趋势凸显了对 **低资源、高性能推理** 的日益增长需求，尤其体现在面向消费者的应用程序与边缘设备中。

与此同时，**多模态生成模型** 的受欢迎程度已超越纯语言模型，图像转视频与图像转 3D 工具获得巨大关注。Lightricks/LTX-2.5 与 black-forest-labs/flux-3 的成功，表明内容创作正从静态图像生成转向动态、可控的创作范式。此外，**微调与合并技术** 日益复杂——如 OliviaRossi 的 PRISM 模型所示——反映出生态系统日趋成熟，社区协作正推动前沿成果不断涌现。尽管专有模型仍具影响力（如 NVIDIA Nemotron），但开源替代方案在性能与可用性上正迅速缩小差距。

---

### **值得探索**

1. **[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** – 下载量超 240 万次，该图像转视频模型代表了未来 AI 驱动的视觉叙事方向。其从静态图像生成流畅电影级片段的能力，使其成为创作者、营销人员与动画师探索生成媒体的必备工具。

2. **[ISTA-DASLab/DeepSeek-V4.1-Flash-1017-GSQ-RCO-GGUF](https://huggingface.co/ISTA-DASLab/DeepSeek-V4.1-Flash-1017-GSQ-RCO-GGUF)** – 本月 Hugging Face 上下载量最高的量化模型，体现了先进量化技术（GSQ+RCO）如何在消费级硬件上实现高质量推理。非常适合构建本地 AI 应用的开发者。

3. **[Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B)** – Hugging Face 上最受欢迎的模型，这一多模态核心兼具视觉、语言与推理能力，以开放权重形式集成于一体。任何构建 AI 助手、内容生成器或 RAG 系统的人都应优先尝试。

---
*本日报由 [agents-radar](https://github.com/yueguan/agents-radar) 自动生成。*