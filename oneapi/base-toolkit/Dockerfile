

# https://github.com/jupyter/docker-stacks/blob/main/images/minimal-notebook/Dockerfile
FROM jupyter/minimal-notebook:latest
ARG ONEAPI_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/e6ff8e9c-ee28-47fb-abd7-5c524c983e1c/l_BaseKit_p_2024.2.1.100_offline.sh'
ARG INTEL_GPU_DRIVER_URL='https://repositories.intel.com/gpu/ubuntu/dists/jammy/lts/2350/intel-gpu-ubuntu-jammy-2350.run'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Intel® oneAPI Base Toolkit: Offline Installer
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?operatingsystem=linux&linux-install-type=offline
# https://www.intel.com/content/www/us/en/docs/oneapi/installation-guide-linux/current/install-with-command-line.html
# ============================================

# Download and install offline oneAPI package  
RUN wget --quiet --output-document oneapi.sh ${ONEAPI_INSTALLER_URL} && \
    sh ./oneapi.sh -a --silent --cli --eula accept && \
    rm oneapi.sh && \
    rm -rf intel

# ============================================
# Install Intel GPU Drivers
# https://dgpu-docs.intel.com/driver/installation.html#ubuntu
# ============================================

RUN wget -qO driver.run ${INTEL_GPU_DRIVER_URL} && \
    chmod +x driver.run && \
    sudo ./driver.run && \
    rm driver.run


# 1. Make sure prerequisites to add repository access are available
RUN apt update && \
    apt install --yes \
        gpg-agent \
        wget

# 2. Add the online network package repository.
RUN wget -qO - https://repositories.intel.com/gpu/intel-graphics.key | \
    sudo gpg --yes --dearmor --output /usr/share/keyrings/intel-graphics.gpg && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/intel-graphics.gpg] https://repositories.intel.com/gpu/ubuntu jammy/lts/2350 unified" > /etc/apt/sources.list.d/intel-gpu-jammy.list && \
    sudo apt update

# 3. Install kernel and Intel® XPU System Management Interface (XPU-SMI) packages on a bare metal system. Installation on the host is sufficient for hardware management and support of the runtimes in containers and bare metal.

# 4. Install packages responsible for computing and media runtimes.
RUN apt install --yes \
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
        clinfo && \
    rm -rf /var/lib/apt/lists/*

# 5. Install development packages
RUN apt install --yes \
        # libigc-dev \
        intel-igc-cm \
        # libigdfcl-dev \
        # libigfxcmrt-dev \
        # level-zero-dev \
        && \
    rm -rf /var/lib/apt/lists/*

# ============================================
# Configure System
# https://www.intel.com/content/www/us/en/docs/oneapi-base-toolkit/get-started-guide-linux/2024-0/before-you-begin.html
# ============================================

# 1. Install CMake*, pkg-config, and GNU* Dev Tools to Build Samples
RUN apt update && \
    apt install --yes \
        cmake \
        pkg-config \
        build-essential \
        && \
    rm -rf /var/lib/apt/lists/*

# 2. Set Environment Variables for CLI Development
# https://github.com/intel/oneapi-containers/blob/master/images/docker/basekit/Dockerfile.ubuntu-22.04
# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/2024.2/oneapi-vars.sh; /usr/local/bin/start-notebook.py"]

# ============================================
# Install 
# Clone the oneAPI Sample Repository
# ============================================
RUN git clone https://github.com/oneapi-src/oneAPI-samples.git work/oneAPI-samples

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan
