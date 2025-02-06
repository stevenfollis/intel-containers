# https://github.com/jupyter/docker-stacks/blob/main/images/minimal-notebook/Dockerfile
# FROM jupyter/minimal-notebook:latest
FROM stevenfollis/intel-containers:minimal-notebook-2024-08-26

LABEL mode="MPI" version="1.0" description="intel-qs built with MPI, no py interface, with Examples"

ARG CMAKE_INSTALLER_URL='https://github.com/Kitware/CMake/releases/download/v3.15.2/cmake-3.15.2-Linux-x86_64.tar.gz'
ARG MINIFORGE_NAME=Miniforge3
ARG MINIFORGE_VERSION=24.7.1-2

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive
ENV CONDA_DIR=/opt/conda

# Install the GNU Make utility & generic MPI implementation
RUN apt-get update && \
  apt-get install --yes \
  build-essential \
  g++ \
  gnupg \
  make \
  mpich && \
  rm -rf /var/lib/apt/lists/*

# Fetch and install CMake 3.15 as required by Intel-QS build process. 
RUN wget --quiet --output-document cmake.tar.gz ${CMAKE_INSTALLER_URL} && \
  tar -xzf cmake.tar.gz --directory /usr/local/ --strip-components=1  && \
  rm cmake.tar.gz

# Fetch and install the Intel MKL libraries required for building the Intel-QS simulator.
RUN wget --quiet https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB -O- | apt-key add - && \
  echo 'deb https://apt.repos.intel.com/mkl all main' > /etc/apt/sources.list.d/intel-mkl.list && \
  apt-get update && \
  apt-get install --yes --no-install-recommends \
  intel-mkl-64bit-2019.2-057 && \
  rm -rf /var/lib/apt/lists/* && \
  echo 'export MKLROOT=/opt/intel/mkl' >> /etc/bash.bashrc

# Install libraries for OpenMP.
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \ 
  libomp-dev && \
  rm -rf /var/lib/apt/lists/*

# Setup the local build environment for the simulation framework.
RUN git clone https://github.com/intel/intel-qs.git /root/intelqs

# Install Intel Quantum Simulator
RUN cd /root/intelqs && \
  source /opt/intel/mkl/bin/mklvars.sh intel64 ilp64 && \
  mkdir build; \
  cd build; \
  CXX=g++ cmake -DIqsMPI=ON -DBuildExamples=ON -DIqsUtest=ON -DIqsPython=OFF ..; \
  make

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan