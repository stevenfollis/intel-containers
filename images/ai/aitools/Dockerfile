# https://github.com/jupyter/docker-stacks/blob/main/images/minimal-notebook/Dockerfile
FROM jupyter/minimal-notebook:latest

ARG INTEL_GPU_DRIVER_URL='https://repositories.intel.com/gpu/ubuntu/dists/jammy/lts/2350/intel-gpu-ubuntu-jammy-2350.run'
ARG AI_TOOLS_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/f27e9e0e-ec27-4024-a4bf-b30c48c99564/l_AITools.2024.2.0.156.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Install Intel GPU Drivers
# https://dgpu-docs.intel.com/driver/installation.html#ubuntu
# ============================================
RUN wget --quiet --output-document driver.run ${INTEL_GPU_DRIVER_URL} && \
  chmod +x driver.run && \
  sudo ./driver.run && \
  rm driver.run

# Download the installer package
RUN wget --quiet --output-document aitools.sh ${AI_TOOLS_INSTALLER_URL} && \
  sh ./aitools.sh -b && \
  rm aitools.sh && \
  source intel/oneapi/intelpython/bin/activate

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan