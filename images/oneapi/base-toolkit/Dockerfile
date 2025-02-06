FROM stevenfollis/intel-containers:intel-gpu-lts

ARG ONEAPI_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/dfc4a434-838c-4450-a6fe-2fa903b75aa7/intel-oneapi-base-toolkit-2025.0.1.46_offline.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Intel® oneAPI Base Toolkit: Offline Installer
# https://www.intel.com/content/www/us/en/docs/oneapi-base-toolkit/get-started-guide-linux/2025-0/overview.html
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=oneapi-toolkit&oneapi-toolkit-os=linux&oneapi-lin=offline
# ============================================

# Download and install offline oneAPI package then tidy up
RUN wget --quiet --output-document oneapi.sh ${ONEAPI_INSTALLER_URL} && \
    mkdir --parents /opt/intel/oneapi && \
    sh ./oneapi.sh \
        -a \
        --silent \
        --cli \
        --eula accept \
        --install-dir '/opt/intel/oneapi' && \
    rm -rf intel && \
    rm oneapi.sh

# ============================================
# Configure System
# https://www.intel.com/content/www/us/en/docs/oneapi-base-toolkit/get-started-guide-linux/2025-0/before-you-begin.html
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
