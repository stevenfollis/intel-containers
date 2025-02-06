# https://github.com/jupyter/docker-stacks/blob/main/images/minimal-notebook/Dockerfile
FROM stevenfollis/intel-containers:base-toolkit-2025.0.1

ARG ONEAPI_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/d461a695-6481-426f-a22f-b5644cd1fa8b/l_HPCKit_p_2024.2.1.79_offline.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# ============================================
# Intel® oneAPI Base Toolkit: Offline Installer
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/hpc-toolkit-download.html?operatingsystem=linux&linux-install-type=offline
# ============================================

# Download and install offline oneAPI package  
RUN wget --quiet --output-document oneapi.sh ${ONEAPI_INSTALLER_URL} && \
    sh ./oneapi.sh -a --silent --cli --eula accept && \
    rm oneapi.sh && \
    rm -rf intel

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan
