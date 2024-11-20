# TODO: Finish / WIP

# https://github.com/jupyter/docker-stacks/blob/main/images/minimal-notebook/Dockerfile
# FROM jupyter/minimal-notebook:latest
FROM stevenfollis/intel-containers:minimal-notebook-2024-08-26

ARG INTEL_GPU_DRIVER_URL='https://repositories.intel.com/gpu/ubuntu/dists/jammy/lts/2350/intel-gpu-ubuntu-jammy-2350.run'

ARG CMAKE_VERSION='3.30.4'
ARG CMAKE_INSTALLER_URL="https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh"

ARG DPC_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/74587994-3c83-48fd-b963-b707521a63f4/l_dpcpp-cpp-compiler_p_2024.2.1.79.sh'

ARG EMBREE_VERSION='4.3.3'
ARG EMBREE_INSTALLER_URL="https://github.com/embree/embree/releases/download/v${EMBREE_VERSION}/embree-${EMBREE_VERSION}.x86_64.linux.tar.gz"

ARG OIDN_VERSION='2.3.0'
ARG OIDN_INSTALLER_URL="https://github.com/OpenImageDenoise/oidn/releases/download/v${OIDN_VERSION}/oidn-${OIDN_VERSION}.x86_64.linux.tar.gz"

ARG OPENVKL_VERSION='2.0.1'
ARG OPENVKL_INSTALLER_URL="https://github.com/RenderKit/openvkl/releases/download/v${OPENVKL_VERSION}/openvkl-${OPENVKL_VERSION}.sycl.x86_64.linux.tar.gz"

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Install Pre-Requisites
# ============================================
RUN apt update && \
  apt install --yes \
  g++ \
  make \
  && rm -rf /var/lib/apt/lists/* 

# Install Intel GPU Drivers
# https://dgpu-docs.intel.com/driver/installation.html#ubuntu
RUN wget --quiet --output-document driver.run ${INTEL_GPU_DRIVER_URL} && \
  chmod +x driver.run && \
  sudo ./driver.run && \
  rm driver.run

# Install CMake via binary installer script and add to path
RUN mkdir /opt/cmake && \
  wget --quiet --output-document /opt/cmake/cmake.sh ${CMAKE_INSTALLER_URL} && \
  sh /opt/cmake/cmake.sh --prefix=/opt/cmake --skip-license && \
  rm /opt/cmake/cmake.sh && \
  ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

# Install Intel® oneAPI DPC++/C++ Compiler
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compiler-download.html?operatingsystem=linux&distribution-linux=online
RUN wget --output-document dpcpp.sh ${DPC_INSTALLER_URL} && \
  /bin/sh dpcpp.sh -a --silent --eula accept && \
  rm -rf intel && \
  rm dpcpp.sh

# ============================================
# Intel® Embree
# https://github.com/RenderKit/embree?tab=readme-ov-file#linux-installation
# ============================================
RUN mkdir --parents /opt/intel/embree && \
  wget --quiet -O- ${EMBREE_INSTALLER_URL} | tar xvz --directory /opt/intel/embree && \
  source /opt/intel/embree/embree-vars.sh

# ============================================
# Intel® Open Image Denoise
# ============================================
RUN wget ${OIDN_INSTALLER_URL}
# TODO: Finish OIDN Install

# ============================================
# Intel® Open Volume Kernel Library (Intel® Open VKL)
# ============================================
RUN wget ${OPENVKL_INSTALLER_URL}}
# TODO: Finish Open VKL Install

# ============================================
# Intel® Open Path Guiding Library (Intel® Open PGL) CPU Only
# ============================================

# ============================================
# Intel® OSPRay
# ============================================

# ============================================
# Intel® OSPRay Studio
# ============================================

# ============================================
# Rendering Toolkit Utilities (Superbuild)
# ============================================

# ============================================
# Intel® OSPRay for Hydra* (Open Source GitHub)
# ============================================

# ============================================
# Intel® Implicit SPMD Program Compiler (Intel® ISPC)
# ============================================


# 2. Set Environment Variables for CLI Development
# https://github.com/intel/oneapi-containers/blob/master/images/docker/basekit/Dockerfile.ubuntu-22.04
# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/setvars.sh; /usr/local/bin/start-notebook.py"]
