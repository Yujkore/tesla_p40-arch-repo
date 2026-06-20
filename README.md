```markdown
# Tesla P40 Arch Linux Archive Repository

This repository serves as an immutable, long-term binary archive designed to sustain the compute, machine learning, and AI workloads of an **NVIDIA Tesla P40** (Pascal Architecture) GPU running on an Arch Linux base.

## Why This Repository Exists

Arch Linux is a rolling-release distribution that aggressively tracks the latest upstream packages, compilers, and kernel developments. While this is ideal for modern hardware, it poses a critical challenge for legacy enterprise hardware like the Tesla P40:

1. **Driver Deprecation:** NVIDIA has moved mainstream and long-term support branches forward, dropping native Pascal architecture support from newer driver releases (590xx+). This forces the system onto the legacy `580xx` branch.
2. **Toolchain Drift:** As Arch upgrades core packages like `gcc` (moving to GCC 15+), building historical frameworks or older CUDA versions (like CUDA 12.9) from source becomes increasingly difficult or completely broken due to strict syntax and compiler mismatches.
3. **AUR Volatility:** Relying on the Arch User Repository (AUR) for structural dependencies means relying on a moving target. Eventually, AUR maintainers orphan packages, or upstream source URLs go offline.

By freezing a verified, harmonized snapshot of the entire **Driver + OpenCL + CUDA + Container Toolkit** stack at version **580.159.04** / **CUDA 12.9**, this machine is permanently insulated from breaking upstream updates.

## Repository Architecture

To bypass GitHub's 100MB file limit on standard pushes while maintaining an active package registry, this repository splits database indexing and binary hosting:

* **Main Branch:** Houses only the lightweight Pacman package database indexes (`tesla_p40-repo.db.tar.gz` and `tesla_p40-repo.files.tar.gz`).
* **GitHub Releases:** Houses the large-scale compiled binary packages (`*.pkg.tar.zst`). Pacman pulls directly from the release assets mirror.

## Contained Packages

The repository archives the following precisely matched components:

* `cuda-12.9-12.9.1-1-x86_64.pkg.tar.zst` — The core development toolkit targeted for legacy compute workloads.
* `nvidia-580xx-dkms-580.159.04-1-x86_64.pkg.tar.zst` — The kernel module framework, ensuring local compilation capacity against future mainline kernels.
* `nvidia-580xx-utils-580.159.04-1-x86_64.pkg.tar.zst` — The matching core userspace libraries.
* `opencl-nvidia-580xx-580.159.04-1-x86_64.pkg.tar.zst` — Version-locked OpenCL runtime for targeted application support.
* `nvidia-580xx-settings-580.159.04-1-x86_64.pkg.tar.zst` & `libxnvctrl-580xx` — Unified configuration utilities.
* `nvidia-container-toolkit-1.19.1-1-x86_64.pkg.tar.zst` & `libnvidia-container` — Isolation layers to securely pass the Tesla P40 into Docker or Podman environments.

## Deployment Configuration

To use this archive on a clean installation or disaster-recovery environment, append the repository definition to the bottom of `/etc/pacman.conf`:

```ini
[tesla_p40-repo]
SigLevel = Optional TrustAll
Server = [https://github.com/YOUR_USERNAME/tesla_p40-arch-repo/releases/download/v1.0.0](https://github.com/yujkore/tesla_p40-arch-repo/releases/download/v1.0.0)
