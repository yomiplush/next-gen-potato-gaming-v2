# next-gen-potato-gaming-v2
CachyOS-based Next-Gen Potato Gaming Setup Guide

![Install CachyOS](https://img-backend-potato.pages.dev/myrigs.png)

## Overview

This guide helps you set up a high-performance Potato gaming environment <br>
on **CachyOS** using **Proton-CachyOS-SLR** with **FSR4** support. <br>
<br>
**Single-boot** is strongly recommended.<br>

## Proof of Concept
![Diablo IV on No GPU Thinkpad](https://img-backend-potato.pages.dev/proof.png)

## My Rigs (Thinkpad X13 Gen1) <br>
<br>
OS: CachyOS x86_64 <br>
Host: LENOVO 20UGS2Q500 <br>
Kernel: 7.1.4-1-cachyos-bore-lto <br>
Shell: fish 4.8.1 <br>
DE: GNOME 50.3 (Wayland) <br>
CPU: AMD Ryzen 5 PRO 4650U with Radeo <br>
GPU: AMD ATI Radeon Vega Series / Rad <br>
Memory: 7142MiB / 29793MiB <br>
<br>

## * [60s Gameplay Collection (5 Titles)
* **Clips:** [© Blizzard Entertaiment / Diablo IV](https://www.dailymotion.com/video/xardf1e)
* * **Clips:** [© FROMSOFTWARE / Eldenring](https://www.dailymotion.com/video/xardj8a)
  * * **Clips:** [© KONAMI / Metal Gear Solid V The Phantom Pain](https://www.dailymotion.com/video/xardktm)
    * * **Clips:** [© KOJIMA PRODUCTION / Death Strandings 2](https://www.dailymotion.com/video/xardlki)
      * * **Clips:** [© ArtPlay (505 Games) / BloodStained Ritual of the Night](https://www.dailymotion.com/video/xardm9e)

## Prerequisites

1. Install [CachyOS](https://cachyos.org/) for your rigs.
2. Fully update the system after installation

```bash
sudo pacman -Syu
```

![Install CachyOSl](https://img-backend-potato.pages.dev/cachyos.png)

## Swap CachyOS's Kernel
1. Open Menu > Search "CachyOS Kernel"
2. Select "cachyos-v3/linux-cachyos-bore-lto" (Gaming Optimize Focus CachyOS's Kernel)
3. Execute > Reboot

![Swap Kernel](https://img-backend-potato.pages.dev/kernel.png)

## Dependency & Installation
```bash
sudo pacman -S steam
```

2. Install Proton-CachyOS-SLR (Critical)This is the key package for FSR4 support. Install it first.
```bash
sudo pacman -S proton-cachyos-slr
mkdir -p ~/.local/share/Steam/compatibilitytools.d/
ln -s /usr/share/steam/compatibilitytools.d/proton-cachyos-slr ~/.local/share/Steam/compatibilitytools.d/proton-cachyos-slr
```

3. Install DependenciesCommon Required Packages
```bash
sudo pacman -S --needed \
    mangohud lib32-mangohud \
    gamemode lib32-gamemode \
    proton-cachyos-slr \
    vulkan-icd-loader lib32-vulkan-icd-loader \
    steam
```

🟥For AMD GPUs
```bash
sudo pacman -S --needed \
    mesa lib32-mesa \
    vulkan-radeon lib32-vulkan-radeon \
    libva-mesa-driver lib32-libva-mesa-driver \
    mesa-vdpau lib32-mesa-vdpau \
    amdvlk lib32-amdvlk            # Optional (use if you want more stability than RADV)
```

🟩For NVIDIA GPUs
```bash
sudo pacman -S --needed \
    nvidia nvidia-utils lib32-nvidia-utils \
    libvdpau lib32-libvdpau
```

Note:
For newer GPUs (Turing+), the nvidia-open series is also an option.
CachyOS’s chwd tool can help with automatic driver configuration.

🔷Additional Packages (Common to Both GPUs)
```bash
sudo pacman -S --needed \
    gamescope \                     # Useful for FSR + frame limiting
    lib32-pipewire pipewire \
    vkd3d lib32-vkd3d \
    openal lib32-openal
```

## Quick Variable Reference

- `PROTON_FSR4_RDNA3_UPGRADE=1` &rarr; Enables FSR4 on RDNA3 GPUs (main strength of Proton-CachyOS-SLR)
- `gamescope` can be added manually (e.g. `gamescope -f -e -- ...`)

## Important Notes

- Single-boot is recommended (especially with NVIDIA)
- Reboot after installing drivers
- Always run `sudo pacman -Syu` first if you encounter issues



## 🔧Steam Launch Options (NVIDIA / AMD Common)
```bash
mangohud gamemoderun PROTON_FSR4_RDNA3_UPGRADE=1 WINE_FULLSCREEN_FSR=1 WINE_FULLSCREEN_FSR_STRENGTH=2
WINE_FULLSCREEN_FSR_MODE=performance DXVK_CONFIG="dxgi.maxDeviceMemory=4096" DXVK_ASYNC=1 PROTON_SET_GAME_RESOLUTION=640x360 %command%
```

## 💎Improved Trick1
1. Set "Performance" when you going to games (improved Framerate stuff)
2. Set "1280x720" when you going to games
3. then, don't setup over "Scale 100%" keep 100% good.

![Power Mode](https://img-backend-potato.pages.dev/power.png)
![Resolution](https://img-backend-potato.pages.dev/resolution.png)

## Technical Explanations
Why Proton-CachyOS-SLR? CachyOS-maintained fork of Proton Experimental.

Includes curated patches from Proton-GE, Proton-EM, and CachyOS-specific improvements.
Excellent upscaler injection system (automatically replaces old DLLs with newer versions).
Better Wayland support, video playback fixes, and low-latency DXVK options.
The -slr variant runs inside Valve’s Steam Linux Runtime container for maximum compatibility.

## Key Environment Variables

| Variable | Purpose |
| :--- | :--- |
| `PROTON_FSR4_RDNA3_UPGRADE=1` | Enables AMD FSR 4 on RDNA 3 GPUs (e.g. RX 7000/9000 series). Downloads and injects the latest FSR 4 DLLs at runtime. |
| `WINE_FULLSCREEN_FSR=1` + `STRENGTH=2` + `MODE=performance` | Legacy FSR 1/2/3 fullscreen upscaling fallback with sharpening control. |
| `DXVK_ASYNC=1` | Enables asynchronous shader compilation in DXVK (Vulkan translation layer for DirectX). Reduces stutter, especially on first run. |
| `DXVK_CONFIG="dxgi.maxDeviceMemory=4096"` | Limits reported VRAM to 4 GB (helps some older engines or memory-hungry titles avoid crashes). |
| `PROTON_SET_GAME_RESOLUTION=640x360` | Forces a low internal render resolution. Combined with FSR/gamescope, this boosts performance while maintaining visual quality via upscaling. |
| `mangohud gamemoderun` | MangoHUD provides FPS, frametime, GPU/CPU usage overlay. GameMode applies automatic CPU governor, I/O priority, and scheduler tweaks for lower latency. |

## Other Components

- **Gamescope**: Lightweight Wayland compositor. Excellent for integer scaling, FSR/ NIS upscaling inside the compositor, frame rate limiting, and HDR.
- **Mesa + RADV (AMD)**: Open-source Vulkan driver. Very fast and feature-rich. `amdvlk` is AMD's official closed-source alternative (sometimes more stable in specific titles).
- **NVIDIA proprietary drivers**: Provide the best performance and feature set on Linux for NVIDIA cards. `nvidia-utils` includes the OpenGL/Vulkan implementation.
- **PipeWire + OpenAL**: Modern audio stack with low latency and good spatial audio support
- **VKD3D**: DirectX 12 &rarr; Vulkan translation layer (required for modern DX12 titles).

## Performance Strategy

The launch options deliberately use a very low base resolution (`640x360`) + aggressive FSR 4 / FSR upscaling. This is the classic "Potato" approach: render cheaply internally, then upscale intelligently for smooth high-refresh-rate Potato gameplay.

# Happy Potato Gaming!

![This is real](https://img-backend-potato.pages.dev/endmemes.png)
