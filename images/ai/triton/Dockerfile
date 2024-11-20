FROM stevenfollis/intel-containers:minimal-notebook-2024-08-26

ARG INTEL_GPU_DRIVER_URL='https://repositories.intel.com/gpu/ubuntu/dists/jammy/lts/2350/intel-gpu-ubuntu-jammy-2350.run'

# 2025.0.0
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=dl-essentials&dl-lin=offline&dl-essentials-os=linux
ARG DEEP_LEARNING_ESSENTIALS_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/5b4aa67a-9679-4d02-865b-137d0f767d82/intel-deep-learning-essentials-2025.0.0.345_offline.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Install Intel GPU Drivers
# https://dgpu-docs.intel.com/driver/installation.html#ubuntu
# ============================================

RUN wget --quiet --output-document driver.run ${INTEL_GPU_DRIVER_URL} && \
  chmod +x driver.run && \
  ./driver.run && \
  rm driver.run

# 1. Make sure prerequisites to add repository access are available
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  gpg-agent \
  gpg \
  wget && \
  rm -rf /var/lib/apt/lists/*

# 2. Add the online network package repository.
RUN wget --quiet --output-document - https://repositories.intel.com/gpu/intel-graphics.key | \
  gpg --yes --dearmor --output /usr/share/keyrings/intel-graphics.gpg && \
  echo "deb [arch=amd64 signed-by=/usr/share/keyrings/intel-graphics.gpg] https://repositories.intel.com/gpu/ubuntu jammy/lts/2350 unified" > /etc/apt/sources.list.d/intel-gpu-jammy.list && \
  apt-get update && \
  rm -rf /var/lib/apt/lists/*

# 3. Install kernel and Intel® XPU System Management Interface (XPU-SMI) packages on a bare metal system. Installation on the host is sufficient for hardware management and support of the runtimes in containers and bare metal.
# N/A for a container

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

# 5. Install development packages
# RUN apt-get install --yes --no-install-recommends \
#   libigc-dev \
#   intel-igc-cm \
#   libigdfcl-dev \
#   libigfxcmrt-dev \
#   level-zero-dev \
#   && \
#   rm -rf /var/lib/apt/lists/*

# ============================================
# Install Intel Deep Learning Essentials
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=dl-essentials&dl-lin=offline&dl-essentials-os=linux
# ============================================

# Download and install offline DLE package  
RUN wget --quiet --output-document intel-deep-learning-essentials.sh ${DEEP_LEARNING_ESSENTIALS_URL} && \
  sh ./intel-deep-learning-essentials.sh -a --silent --cli --eula accept && \
  rm intel-deep-learning-essentials.sh && \
  rm -rf intel

# ============================================
# Install PyTorch and Triton from nightly wheels
# https://github.com/intel/intel-xpu-backend-for-triton?tab=readme-ov-file#install-pytorch-and-triton-from-nightly-wheels
# ============================================

# RUN pip install torch-*.whl triton-*.whl
COPY wheels-pytorch-py3.11-20241119/ wheels-pytorch
RUN cd ./wheels-pytorch && \
  pip install torch-*.whl triton-*.whl

# ============================================
# Configure System
# https://www.intel.com/content/www/us/en/docs/oneapi-base-toolkit/get-started-guide-linux/2024-0/before-you-begin.html
# ============================================

# 1. Install CMake*, pkg-config, and GNU* Dev Tools
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  build-essential \
  cmake \
  pkg-config \
  && rm -rf /var/lib/apt/lists/*

# 2. Set Environment Variables for CLI Development
# https://github.com/intel/oneapi-containers/blob/master/images/docker/basekit/Dockerfile.ubuntu-22.04
# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/2025.0/oneapi-vars.sh; /usr/local/bin/start-notebook.py"]

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan