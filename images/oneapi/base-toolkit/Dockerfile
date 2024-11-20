FROM stevenfollis/intel-containers:intel-gpu-lts

ARG ONEAPI_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/e6ff8e9c-ee28-47fb-abd7-5c524c983e1c/l_BaseKit_p_2024.2.1.100_offline.sh'

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
# Configure System
# https://www.intel.com/content/www/us/en/docs/oneapi-base-toolkit/get-started-guide-linux/2024-0/before-you-begin.html
# ============================================

# 1. Install CMake*, pkg-config, and GNU* Dev Tools to Build Samples
RUN apt-get update && \
    apt-get install --yes --no-install-recommends \
    build-essential \
    cmake \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# 2. Set Environment Variables for CLI Development
# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/setvars.sh; /usr/local/bin/start-notebook.py"]

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan
