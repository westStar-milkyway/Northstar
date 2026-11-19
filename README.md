# Northstar

> High-performance graphics recompilation and rendering layer for Grand Theft Auto VI on PC

![Status](https://img.shields.io/badge/status-pre--alpha%20%2F%20assets%20extracted-orange)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20Linux-blue)
![API](https://img.shields.io/badge/graphics-DirectX%2012%20%7C%20Vulkan-informational)
![License](https://img.shields.io/badge/license-MIT-green)

## Overview

Northstar is a native PC recompilation and graphics optimization runtime engineered for Grand Theft Auto VI, targeting both Windows and Linux environments. The primary objective of the project is to eliminate rendering bottlenecks, maximize GPU utilization, and deliver deterministic frame times across modern desktop hardware.

Currently in its foundational phase, the project holds the extracted game files and is undergoing static analysis to build a scalable, high-framerate multi-platform execution pipeline from the ground up.

## Planned Architecture & Graphics Focus

### GPU Pipeline & Draw Call Optimization

- **Command Buffer Restructuring:** Merging redundant draw calls and optimizing command list submission to reduce driver overhead.
- **Asynchronous Compute Scheduling:** Offloading post-processing, volumetric lighting, and particle simulations to dedicated async compute queues.
- **Shader Bytecode Recompilation:** Translating native shaders into optimized DXIL and SPIR-V binaries ahead of time to eliminate runtime compilation stutter.
- **Bindless Resource Management:** Reducing descriptor heap switching during dense urban rendering.

### Frame Pacing & Latency Reduction

- **Deterministic Frame Queueing:** Dynamic pre-rendered frame caps to minimize input latency without starving the GPU.
- **VRAM Defragmentation & Residency:** Prioritizing high-resolution geometry and shadow cascades within local video memory to prevent PCIe bus saturation.
- **Sub-millisecond Framelimit Control:** Built-in precision frame pacing independent of external driver limiters.

### Temporal Reconstruction & Upscaling

Planned native integration hooks for modern spatial and temporal upscalers across platforms:

- NVIDIA DLSS / DLSS-G (Windows & Linux via Proton/Native VK)
- AMD FidelityFX Super Resolution (FSR 3.1+)
- Intel XeSS
- Native TAAU (Temporal Anti-Aliasing Upsampling) override

## Target Hardware Specifications

### Baseline (1080p / 60 FPS Target)

| Subsystem | Specification |
| :--- | :--- |
| **Processor** | AMD Ryzen 5 5600X / Intel Core i5-12600K |
| **Memory** | 16 GB DDR4 (3200 MHz Dual-Channel) |
| **Graphics** | NVIDIA RTX 3060 Ti / AMD Radeon RX 6750 XT (8 GB VRAM) |
| **Storage** | PCIe Gen3 NVMe SSD |
| **OS** | Windows 10/11 or Linux (Kernel 6.x+, Mesa latest) |

### Enthusiast (1440p - 4K / 120+ FPS Target)

| Subsystem | Specification |
| :--- | :--- |
| **Processor** | AMD Ryzen 7 7800X3D / Intel Core i7-14700K |
| **Memory** | 32 GB DDR5 (6000 MHz) |
| **Graphics** | NVIDIA RTX 4080 Super / AMD Radeon RX 7900 XTX (16+ GB VRAM) |
| **Storage** | PCIe Gen4 NVMe SSD (DirectStorage capable) |
| **OS** | Windows 11 (23H2 / 24H2) or Linux (Wayland / X11) |

## Target Deployment & File Layout

*Note: Runtime binaries are not yet compiled. The structure below represents the target layout once the initial executable lifting is complete.*

```text
Grand Theft Auto VI/
├── northstar/
│   ├── bin/
│   ├── shader_cache/
│   └── telemetry/
├── d3d12.dll (Windows) / libnorthstar.so (Linux)
├── NorthstarLauncher
├── northstar.toml
└── GTA6.exe
```

## Target Runtime Configuration (`northstar.toml`)

```toml
[graphics]
async_compute = true
precompile_shaders = true
bindless_textures = true
vram_budget_ratio = 0.90
renderer_backend = "auto" # Options: dx12, vulkan

[frame_pacing]
max_frame_latency = 1
internal_fps_cap = 0
reflex_low_latency = "enabled_boost"

[upscaling]
backend = "auto" # Options: dlss, fsr, xess, native
quality_preset = "quality"
```

## Stability Roadmap (Path to v1.0)

Achieving a crash-free, consistent multi-platform port requires building the runtime layer step-by-step from the raw game files. Below are the critical milestones required for full build stability:

### Phase 1: File Extraction & Static Analysis (In Progress)

- [x] Extraction and decryption of base Grand Theft Auto VI game archives and asset containers.
- [ ] Mapping of proprietary archive structures, texture dictionaries, and audio banks.
- [ ] Disassembly and static analysis of the primary executable binaries.
- [ ] Identification of engine subsystem entry points and memory allocation routines across targets.

### Phase 2: Binary Translation & Boot Stability (Planned)

- [ ] Instruction lifting of the core executable to native x64 instructions (Windows and Linux ELF formats).
- [ ] Memory mapping alignment for 64-bit virtual address spaces.
- [ ] Basic window creation, swapchain initialization (Win32/X11/Wayland), and input polling.
- [ ] Reaching a stable main menu and world-loading state without immediate access violations.

### Phase 3: Graphics Pipeline & Shader Parity (Planned)

- [ ] Translation of primary G-Buffer and deferred lighting passes to DX12 and Vulkan SPIR-V.
- [ ] Resolution of broken compute shaders causing vertex explosions on foliage and water meshes.
- [ ] Elimination of pipeline state object (PSO) creation hitches during high-speed traversal.
- [ ] Fix memory leaks occurring during prolonged ray-traced reflection bounces.

### Phase 4: Thread Synchronization & Cross-Platform Validation (Planned)

- [ ] Eliminate race conditions between the asset streaming thread and the render thread.
- [ ] Implement graceful fallback mechanisms when VRAM limits are exceeded on 8 GB cards.
- [ ] Multi-vendor and cross-platform driver stress testing (NVIDIA proprietary, AMD RADV, Intel Mesa).
- [ ] Long-duration soak testing (12+ hours continuous runtime) on both Windows and Linux native environments.

## Current Limitations

- **No Playable Build:** Only the extracted raw game files are currently available; the recompilation layer and cross-platform launchers are under active early development.
- **Asset Format Research:** Several proprietary compression and shader container formats are still being reverse-engineered.

## Author

- **westStar-milkyway** (Project Creator and Lead Maintainer)

## Contributor(s)

- **Zetsukae** (Create markdown)

## License

MIT License

Copyright (c) 2026 westStar-milkyway

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

---

### Legal Notice

Northstar is an independent reverse-engineering and performance research project. It contains no proprietary game assets, copyrighted code, or DRM circumvention tools. Users must provide their own legally obtained copy of the software. Grand Theft Auto and Rockstar Games are registered trademarks of Take-Two Interactive Software, Inc.
