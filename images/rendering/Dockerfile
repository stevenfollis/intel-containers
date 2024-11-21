# TODO: Finish / WIP
FROM stevenfollis/intel-containers:intel-gpu-lts

ARG CMAKE_VERSION='3.31.0'
ARG CMAKE_INSTALLER_URL="https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh"

ARG DPCPP_VERSION='2025.0.1'
ARG DPCPP_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/4fd7c6b0-853f-458a-a8ec-421ab50a80a6/intel-dpcpp-cpp-compiler-2025.0.1.46_offline.sh'

ARG EMBREE_VERSION='4.3.3'
ARG EMBREE_INSTALLER_URL="https://github.com/embree/embree/releases/download/v${EMBREE_VERSION}/embree-${EMBREE_VERSION}.x86_64.linux.tar.gz"

ARG OIDN_VERSION='2.3.1'
ARG OIDN_INSTALLER_URL="https://github.com/OpenImageDenoise/oidn/releases/download/v${OIDN_VERSION}/oidn-${OIDN_VERSION}.x86_64.linux.tar.gz"

ARG OPENVKL_VERSION='2.0.1'
ARG OPENVKL_INSTALLER_URL="https://github.com/RenderKit/openvkl/releases/download/v${OPENVKL_VERSION}/openvkl-${OPENVKL_VERSION}.sycl.x86_64.linux.tar.gz"

# https://github.com/hadolint/hadolint/wiki/DL4006
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Install Pre-Requisites
# ============================================
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  g++ \
  make \
  && rm -rf /var/lib/apt/lists/* 

# Install CMake via binary installer script and add to path
RUN mkdir /opt/cmake && \
  wget --quiet --output-document /opt/cmake/cmake.sh ${CMAKE_INSTALLER_URL} && \
  sh /opt/cmake/cmake.sh --prefix=/opt/cmake --skip-license && \
  rm /opt/cmake/cmake.sh && \
  ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

# Install Intel® oneAPI DPC++/C++ Compiler
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compiler-download.html?operatingsystem=linux&distribution-linux=online
RUN wget --quiet --output-document dpcpp.sh ${DPCPP_INSTALLER_URL} && \
  /bin/sh dpcpp.sh -a --silent --eula accept && \
  rm -rf intel && \
  rm dpcpp.sh

# ============================================
# Intel® Embree
# https://github.com/RenderKit/embree?tab=readme-ov-file#linux-installation
# ============================================
RUN mkdir --parents /opt/intel/embree && \
  wget --quiet -O- ${EMBREE_INSTALLER_URL} | tar xvz --directory /opt/intel/embree

# ============================================
# Intel® Open Image Denoise
# Perequisites: https://www.openimagedenoise.org/downloads.html
# ============================================
# RUN wget ${OIDN_INSTALLER_URL}
# TODO: Finish OIDN Install
# RUN mkdir --parents /opt/intel/oidn && \
#   wget --quiet -O- ${EMBREE_INSTALLER_URL} | tar xvz --directory /opt/intel/embree


# ============================================
# Intel® Open Volume Kernel Library (Intel® Open VKL)
# ============================================
# RUN wget ${OPENVKL_INSTALLER_URL}}
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
CMD ["/bin/bash", "-c", "source /opt/intel/embree/embree-vars.sh; /usr/local/bin/start-notebook.py"]

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan
