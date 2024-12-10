FROM stevenfollis/intel-containers:intel-gpu-lts

ARG CMAKE_VERSION='3.31.0'
ARG CMAKE_INSTALLER_URL="https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh"

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# Install pre-requisites
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

# User RenderKit Superbuild to build all libaries
# https://github.com/RenderKit/superbuild 
RUN git clone https://github.com/RenderKit/superbuild && \
    cd superbuild && \
    mkdir build && \
    cd build && \
    cmake .. && \
    cmake --build . && \
    mkdir --parents /opt/intel/renderkit && \
    cp --recursive install/* /opt/intel/renderkit/ && \
    cd /home/jovyan && \
    rm -rf /home/jovyan/superbuild