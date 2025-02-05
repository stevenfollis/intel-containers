# https://dgpu-docs.intel.com/driver/installation-rolling.html#ubuntu
FROM stevenfollis/intel-containers:minimal-notebook-2024-08-26

ARG INTEL_GPU_DRIVER_URL='https://repositories.intel.com/gpu/ubuntu/dists/jammy/intel-gpu-ubuntu-jammy.run'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Install Intel GPU Drivers
# https://dgpu-docs.intel.com/driver/installation-rolling.html#ubuntu
# ============================================

# 1. Make sure prerequisites to add repository access are available
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  gpg-agent \
  gpg \
  wget && \
  rm -rf /var/lib/apt/lists/*

# 2. Optionally, instead of using the online network package repository, you can download and use the offline repository
RUN wget --quiet --output-document intel-gpu-ubuntu.run ${INTEL_GPU_DRIVER_URL} && \
  chmod +x intel-gpu-ubuntu.run && \
  ./intel-gpu-ubuntu.run && \
  rm intel-gpu-ubuntu.run

# 3. Install kernel and Intel® XPU System Management Interface (XPU-SMI) packages on a bare metal system. 
# Installation on the host is sufficient for hardware management and support of the runtimes in containers and bare metal.
# Not Applicable

# 4. Install packages responsible for computing and media runtimes.
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  intel-opencl-icd \
  intel-level-zero-gpu \
  level-zero \
  intel-media-va-driver-non-free \
  libmfx1 \
  libmfxgen1 \
  libvpl2 \
  libegl-mesa0 \
  libegl1-mesa \
  libegl1-mesa-dev \
  libgbm1 \
  libgl1-mesa-dev \
  libgl1-mesa-dri \
  libglapi-mesa \
  libgles2-mesa-dev \
  libglx-mesa0 \
  libigdgmm12 \
  libxatracker2 \
  mesa-va-drivers \
  mesa-vdpau-drivers \
  mesa-vulkan-drivers \
  va-driver-all \
  vainfo \
  hwinfo \
  clinfo \
  && rm -rf /var/lib/apt/lists/*

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan