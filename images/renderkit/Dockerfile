FROM stevenfollis/intel-containers:intel-gpu-lts

ARG CMAKE_VERSION='3.31.0'
ARG CMAKE_INSTALLER_URL="https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh"

ARG DPCPP_VERSION='2025.0.1'
ARG DPCPP_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/4fd7c6b0-853f-458a-a8ec-421ab50a80a6/intel-dpcpp-cpp-compiler-2025.0.1.46_offline.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Install pre-requisites
# ============================================
RUN apt-get update && \
    apt-get install --yes --no-install-recommends \
    g++ \
    libxrandr-dev \
    libxinerama-dev \
    libxcursor-dev \
    libxi-dev \
    make \
    && rm -rf /var/lib/apt/lists/* 

# Install CMake via binary installer script and add to path
RUN mkdir /opt/cmake && \
    wget --quiet --output-document /opt/cmake/cmake.sh ${CMAKE_INSTALLER_URL} && \
    sh /opt/cmake/cmake.sh --prefix=/opt/cmake --skip-license && \
    rm /opt/cmake/cmake.sh && \
    ln -s /opt/cmake/bin/cmake /usr/local/bin/cmake

# Install Intel® oneAPI DPC++/C++ Compiler
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/dpc-compiler-download.html?operatingsystem=linux&distribution-linux=offline
RUN wget --quiet --output-document dpcpp.sh ${DPCPP_INSTALLER_URL} && \
    /bin/sh dpcpp.sh -a --silent --eula accept && \
    rm -rf intel && \
    rm dpcpp.sh

# Intel® Implicit SPMD Program Compiler ("ISPC")
# https://github.com/ispc/ispc
RUN wget --quiet --output-document=- https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB | gpg --dearmor | tee /usr/share/keyrings/oneapi-archive-keyring.gpg > /dev/null && \
    echo "deb [signed-by=/usr/share/keyrings/oneapi-archive-keyring.gpg] https://apt.repos.intel.com/oneapi all main" | tee /etc/apt/sources.list.d/oneAPI.list && \
    apt-get update && \
    apt-get install --yes --no-install-recommends intel-oneapi-ispc && \
    rm -rf /var/lib/apt/lists/*

# ============================================
# User RenderKit Superbuild to build all libaries
# https://github.com/RenderKit/superbuild 
# ============================================
RUN git clone https://github.com/RenderKit/superbuild && \
    cd superbuild && \
    mkdir build && \
    cd build && \
    cmake .. && \
    cmake --build . && \
    cp --recursive install/* /opt/intel/oneapi/ && \
    cd /home/jovyan && \
    rm -rf /home/jovyan/superbuild

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan
