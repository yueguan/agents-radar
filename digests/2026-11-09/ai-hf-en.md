# Hugging Face Trending Models Weekly 2026-11-09

> Source: [Hugging Face Hub](https://huggingface.co/) | 30 models | Generated: 2026-11-09 01:56 UTC

---

---

### **Today's Highlights**

Hugging Face’s ecosystem in late 2026 is dominated by high-impact multimodal and quantized models, with *Lightricks/LTX-2.5* leading in downloads with over 2.6 million, underscoring the explosive growth of image-to-video generation. The Qwen family continues to dominate, especially in fine-tuned and quantized variants—*Qwen/Qwen3.8-27B* and *unsloth/Qwen3.8-Flash-Next-GGUF* are top performers across likes and downloads. Notably, "uncensored" and "heretic" variants are proliferating, indicating strong community demand for unrestricted models. Meanwhile, open-source vision models like *facebook/sam3* and *PrunaAI/P-H3-Image-Comfy* highlight growing momentum in accessible, high-quality image and mask generation.

---

### **Trending Models**

#### 🧠 Language Models (LLMs, chat models, instruction-tuned)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) | Qwen | 19,419 | 7,668,187 | A powerful multimodal LLM tuned for image-text understanding; its massive download count reflects widespread adoption in production and research. |
| [DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-...](https://huggingface.co/DavidAU/Qwen3.8-27B-TURBO-Fable-Cold-Fusion-735-882-Heretic-Uncensored-NEO-CODER-MAX-MTP-GGUF) | DavidAU | 2,711 | 1,553,252 | A highly optimized, uncensored GGUF variant of Qwen3.8-27B tailored for local inference and code generation—ideal for developers seeking performance and freedom. |
| [unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF) | unsloth | 1,537 | 1,548,621 | Built for speed and efficiency using Unsloth’s optimized inference stack; one of the most downloaded GGUF models, popular for low-latency deployment. |
| [LiquidAI/LFM3-1.7B](https://huggingface.co/LiquidAI/LFM3-1.7B) | LiquidAI | 1,558 | 73,562 | A lightweight yet capable text-generation model designed for edge deployment; gaining traction among hobbyists and mobile AI developers. |

#### 🎨 Multimodal & Generation (image, video, audio, text-to-X)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5) | Lightricks | 11,137 | 2,661,026 | A state-of-the-art image-to-video model with seamless motion synthesis; its 2.6M+ downloads show it’s a go-to for creators and content studios. |
| [black-forest-labs/flux-3-image-klein-comfy](https://huggingface.co/black-forest-labs/flux-3-image-klein-comfy) | black-forest-labs | 2,761 | 1,167,959 | Part of the Flux 3 series, this ComfyUI-compatible model excels in image-to-image editing with rich detail and artistic control—popular in creative workflows. |
| [abenzerps/Qwen-Image-2.1-Turbo-Quantized](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized) | abenzerps | 5,726 | 76,804 | A quantized, ComfyUI-ready version of Qwen Image 2.1 that delivers fast, high-fidelity text-to-image generation—ideal for local inference. |
| [facebook/sam3](https://huggingface.co/facebook/sam3) | facebook | 6,111 | 1,982,669 | The latest in the SAM series, SAM3 offers advanced mask generation for videos and complex scenes; its 1.9M downloads reflect deep integration into computer vision pipelines. |

#### 🔧 Specialized Models (code, math, medical, embeddings)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [ccore/Qwen3.5-4B-3D-Coder](https://huggingface.co/ccore/Qwen3.5-4B-3D-Coder) | ccore | 921 | 254,053 | A compact, code-focused model generating Three.js 3D assets from text prompts—ideal for web developers building interactive 3D experiences. |
| [ccore/Qwen3.5-4B-GLB-3D-Coder](https://huggingface.co/ccore/Qwen3.5-4B-GLB-3D-Coder) | ccore | 275 | 33,641 | Generates GLB files directly via code instructions—targeted at game devs and AR/VR creators needing efficient asset pipelines. |
| [nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B) | nvidia | 3,107 | 67,230 | A high-accuracy speech-to-text model optimized for real-time transcription; part of NVIDIA’s Nemo ecosystem, used in enterprise voice systems. |

#### 📦 Fine-tunes & Quantizations (community fine-tunes, GGUF, AWQ)

| Model | Author | Likes | Downloads | Summary |
| :--- | :--- | ---: | ---: | :--- |
| [xtools-at/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic](https://huggingface.co/xtools-at/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic) | xtools-at | 855 | 7,088 | An aggressive, uncensored fusion of DeepSeek and Qwen3.5—appeals to users wanting maximal freedom in prompt handling. |
| [AtomicChat/DeepSeek-V4.1-Flash-1017-Uncensored-GSQ-RCO-GGUF](https://huggingface.co/AtomicChat/DeepSeek-V4.1-Flash-1017-Uncensored-GSQ-RCO-GGUF) | AtomicChat | 391 | 130,020 | A GGUF-optimized, uncensored version of DeepSeek-V4.1 Flash—popular among self-hosters seeking performance and minimal restrictions. |
| [mradermacher/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic-i1-GGUF](https://huggingface.co/mradermacher/DeepSeek-V4-Pro-Qwen3.5-9B-UNCENSORED-heretic-i1-GGUF) | mradermacher | 187 | 136,706 | Another community-driven, i1-optimized GGUF variant—shows how niche tuning layers are becoming standard in local AI stacks. |
| [Scantist/GLM-5.3-SLIM-GGUF](https://huggingface.co/Scantist/GLM-5.3-SLIM-GGUF) | Scantist | 1,292 | 4,938 | A slimmed-down, MoE-based GGUF model using neural architecture search—demonstrates rising interest in efficient, scalable inference. |

---

### **Ecosystem Signal**

The Hugging Face ecosystem in late 2026 reveals a clear shift toward **open-weight, community-driven innovation**, particularly around the Qwen, DeepSeek, and GLM families. These model families now dominate both popularity and usage, with extensive forks, quantizations, and fine-tunes circulating across GGUF, safetensors, and custom formats. The rise of "heretic" and "uncensored" variants—especially within Qwen and DeepSeek ecosystems—signals strong user demand for unfiltered, customizable models, often deployed locally via llama.cpp or vLLM. Quantization remains central: GGUF has become the de facto standard for local inference, with tools like Unsloth, GSQ, and RCO enabling faster, lower-memory runs. Meanwhile, multimodal models like LTX-2.5 and flux-3 are driving adoption beyond research—into creative production, video editing, and real-time applications. This indicates a maturing ecosystem where accessibility, performance, and customization are prioritized over raw size.

---

### **Worth Exploring**

1. **[Lightricks/LTX-2.5](https://huggingface.co/Lightricks/LTX-2.5)** – With over 2.6 million downloads, this image-to-video model is a benchmark for generative video creation. Its ability to produce smooth, coherent sequences from static images makes it essential for content creators, filmmakers, and social media professionals.

2. **[unsloth/Qwen3.8-Flash-Next-GGUF](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF)** – One of the fastest and most efficient GGUF models available, ideal for developers deploying LLMs on consumer hardware. Its combination of speed, low memory footprint, and high-quality output sets a new standard for local inference.

3. **[abenzerps/Qwen-Image-2.1-Turbo-Quantized](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized)** – A highly optimized, ComfyUI-native text-to-image model that balances quality and speed. Perfect for artists and designers seeking rapid iteration without cloud dependency.

---
*This digest is auto-generated by [agents-radar](https://github.com/yueguan/agents-radar).*