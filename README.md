# ComfyUI RTX 5090 Optimized Docker Image

[![Watch the video](https://i3.ytimg.com/vi/JovhfHhxqdM/hqdefault.jpg)](https://www.youtube.com/watch?v=JovhfHhxqdM)

Docker image optimized for **NVIDIA RTX 5090** with CUDA 12.8, PyTorch 2.6.0+cu128, xFormers, and TorchAO acceleration libraries. Pre-configured with latest image generation models.

## 🚀 Core Features

### RTX 5090 Exclusive Optimizations
- ✅ **CUDA 12.8** - Full RTX 5090 architecture support
- ✅ **PyTorch 2.6.0+cu128** - Latest CUDA 12.8 build
- ✅ **xFormers** - Efficient attention mechanism acceleration
- ✅ **TorchAO** - Model optimization and quantization acceleration
- ✅ **Optimized Environment Variables** - PYTORCH_CUDA_ALLOC_CONF, TORCH_CUDNN_V8_API_ENABLED, CUDA_MODULE_LOADING

### Pre-installed Model Suites

#### 1. **Z-Image-Turbo** (Fast Generation)
- 6B parameter model, 8-step fast generation
- ControlNet Union support (Canny, HED, Depth, Pose, MLSD)
- De-Turbo LoRA (enhanced details)
- AIO LoRA (All-In-One enhancement)

#### 2. **Qwen-Image-Edit-2511** (Image Editing)
- **Q5_0 Quantized Version** (14.4 GB) - RTX 5090 optimized
- Multi-image editing with consistency preservation
- Lightning LoRA (4-step/8-step fast generation)
- Professional editing LoRAs:
  - Next-Scene-v2 (Scene transitions)
  - Multiple-Angles (Multi-angle camera control)
  - Light-Migration (Light transfer)
  - Best-Face-Swap (Face swapping)

### Automatic Model Download
- 📦 Auto-download all models on first startup
- 📂 Smart directory management
- 🔄 Resume support
- ⚡ Parallel download acceleration

## 📋 Quick Start

### Build Image

```bash
# Clone repository
git clone https://github.com/Neneka448/comfyui-base.git
cd comfyui-base

# Build RTX 5090 optimized image
docker buildx bake devpush5090
```

### Run Container

```bash
docker run -d \
  --gpus all \
  -p 8188:8188 \
  -p 8189:8189 \
  -p 8080:8080 \
  -p 8888:8888 \
  -p 22:22 \
  -v /path/to/workspace:/workspace \
  soyoanon/comfyui:dev-5090
```

### First Startup

1. Container automatically installs dependencies on startup (~5-10 minutes)
2. Auto-downloads all models to ComfyUI directories (~50 GB)
3. Ready when you see: `[ComfyUI-Manager] All startup tasks have been completed.`

## 🌐 Access Ports

| Port | Service | Description |
|------|---------|-------------|
| `8188` | ComfyUI Web UI | Main interface |
| `8189` | Helper Web | Model downloader, logs viewer |
| `8080` | FileBrowser | File management (admin / adminadmin12) |
| `8888` | JupyterLab | Development environment (token via `JUPYTER_PASSWORD`) |
| `22` | SSH | Remote access (set `PUBLIC_KEY` or check logs for password) |

## 📁 Directory Structure

```
/workspace/
├── runpod-slim/
│   ├── ComfyUI/                    # ComfyUI main directory
│   │   ├── models/
│   │   │   ├── checkpoints/        # Checkpoints
│   │   │   ├── unet/               # UNet models
│   │   │   ├── vae/                # VAE models
│   │   │   ├── text_encoders/      # Text Encoders (Qwen2.5-VL, Qwen3)
│   │   │   ├── loras/              # LoRAs (Lightning, editing enhancements)
│   │   │   ├── controlnet/         # ControlNet Union
│   │   │   └── diffusion_models/   # Z-Image-Turbo
│   │   ├── custom_nodes/           # Custom nodes
│   │   │   ├── ComfyUI-Manager/
│   │   │   ├── ComfyUI-GGUF/       # GGUF support
│   │   │   ├── ComfyUI-KJNodes/
│   │   │   └── Civicomfy/
│   │   └── .models_downloaded      # Model download flag
│   ├── comfyui_args.txt            # Custom startup arguments
│   └── filebrowser.db              # FileBrowser database
├── download_models.sh              # Model download script
└── webui/                          # Helper Web UI
```

## ⚙️ Custom Configuration

### ComfyUI Startup Arguments

Edit `/workspace/runpod-slim/comfyui_args.txt` (one argument per line):

```bash
--max-batch-size 8
--preview-method auto
--highvram
--bf16-vae
```

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PUBLIC_KEY` | - | SSH public key |
| `JUPYTER_PASSWORD` | - | JupyterLab Token |

## 🔧 Pre-installed Custom Nodes

- **ComfyUI-Manager** - Node package manager
- **ComfyUI-GGUF** - GGUF model support (Q5_0 quantization)
- **ComfyUI-KJNodes** - Common utility nodes
- **Civicomfy** - CivitAI model downloader

## 📊 Model Details

### Z-Image-Turbo
- **Main Model**: diffusion_pytorch_model.safetensors
- **Parameters**: 6B
- **Generation Steps**: 8 steps (Turbo)
- **Features**: Fast generation, multiple ControlNet support

### Qwen-Image-Edit-2511
- **Quantized Version**: FP8 Lightning (20.4 GB)
- **Original Size**: 57.7 GB → 65% compression
- **VRAM Requirement**: ~18-20 GB
- **Features**: Multi-image editing, consistency preservation, Lightning 4-step acceleration

## 💾 Disk Space Requirements

| Item | Size |
|------|------|
| Z-Image-Turbo Suite | ~22.5 GB |
| Qwen-Image-Edit (FP8) | ~21 GB |
| ComfyUI + Dependencies | ~10 GB |
| **Total** | ~55-60 GB |

**Recommended Disk Space**: 80-100 GB

## 🎯 Performance Optimization

### RTX 5090 32GB VRAM Configuration
```
Z-Image-Turbo: 12-15 GB VRAM (FP16)
Qwen-Image-Edit FP8: 18-20 GB VRAM

Can load multiple models simultaneously!
```

### Acceleration Features
- **xFormers**: 2-3x attention mechanism acceleration
- **TorchAO**: Model optimization and quantization
- **Lightning LoRA**: 4-step/8-step fast generation
- **GGUF Q5_0**: 75% VRAM reduction through quantization

## 🔄 Re-download Models

To re-download all models:

```bash
# Enter container
docker exec -it <container_id> bash

# Remove download flag
rm /workspace/runpod-slim/ComfyUI/.models_downloaded

# Manually run download script
bash /workspace/download_models.sh

# Or restart container for auto-download
docker restart <container_id>
```

## 🐛 Troubleshooting

### Models Not Loading
Check if model files exist in `/workspace/runpod-slim/ComfyUI/models/` directory

### GGUF Models Not Showing
Confirm ComfyUI-GGUF node is installed:
```bash
ls /workspace/runpod-slim/ComfyUI/custom_nodes/ComfyUI-GGUF
```

### Out of VRAM
- Use Q5_0 quantized models instead of FP16
- Enable `--lowvram` parameter
- Reduce batch size

## 📝 Changelog

### v1.0 (2025-12-10)
- ✅ RTX 5090 exclusive optimizations (CUDA 12.8)
- ✅ Integrated xFormers and TorchAO
- ✅ Pre-configured Z-Image-Turbo
- ✅ Pre-configured Qwen-Image-Edit-2511 (FP8 Lightning)
- ✅ Automatic model download script
- ✅ ComfyUI-GGUF node support

## 📄 License

This project is based on the following licenses:
- ComfyUI: GPL-3.0
- Z-Image-Turbo: Apache 2.0
- Qwen-Image-Edit: Apache 2.0

## 🤝 Contributing

Issues and Pull Requests are welcome!

## 🔗 Related Links

- [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- [Z-Image-Turbo](https://huggingface.co/Tongyi-MAI/Z-Image-Turbo)
- [Qwen-Image-Edit](https://huggingface.co/Qwen/Qwen-Image-Edit-2511)
- [models.json](./models.json) - Complete model list

---

**Note**: This image is optimized specifically for RTX 5090. Other GPUs may not achieve optimal performance.

Run the latest ComfyUI. First start installs dependencies (takes a few minutes), then when you see this in the logs, ComfyUI is ready to be used: `[ComfyUI-Manager] All startup tasks have been completed.`

## Access

- `8188`: ComfyUI web UI
- `8189`: Helper web page (model downloaders, logs)
- `8080`: FileBrowser (admin / adminadmin12)
- `8888`: JupyterLab (token via `JUPYTER_PASSWORD`, root at `/workspace`)
- `22`: SSH (set `PUBLIC_KEY` or check logs for generated root password)

## Pre-installed custom nodes

- ComfyUI-Manager
- ComfyUI-KJNodes
- Civicomfy

## Custom Arguments

Edit `/workspace/runpod-slim/comfyui_args.txt` (one arg per line):

```
--max-batch-size 8
--preview-method auto
```

## Directory Structure

- `/workspace/runpod-slim/ComfyUI`: ComfyUI install
- `/workspace/runpod-slim/comfyui_args.txt`: ComfyUI args
- `/workspace/runpod-slim/filebrowser.db`: FileBrowser DB
