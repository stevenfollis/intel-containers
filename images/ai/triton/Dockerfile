FROM stevenfollis/intel-containers:intel-gpu-lts

# 2025.0.0
# https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?packages=dl-essentials&dl-lin=offline&dl-essentials-os=linux
ARG DEEP_LEARNING_ESSENTIALS_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/5b4aa67a-9679-4d02-865b-137d0f767d82/intel-deep-learning-essentials-2025.0.0.345_offline.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# Download and install offline DLE package  
RUN wget --quiet --output-document intel-deep-learning-essentials.sh ${DEEP_LEARNING_ESSENTIALS_URL} && \
  sh ./intel-deep-learning-essentials.sh -a --silent --cli --eula accept && \
  rm intel-deep-learning-essentials.sh && \
  rm -rf intel

# ============================================
# Install PyTorch and Triton from nightly wheels
# https://github.com/intel/intel-xpu-backend-for-triton?tab=readme-ov-file#install-pytorch-and-triton-from-nightly-wheels
# ============================================

# NOTE: install file not committed to repo. 
# TODO: move to a more dynamic pull of files; GH Actions requires auth to GH
WORKDIR /triton-install
COPY wheels-pytorch-py3.11-20241122.zip /triton-install
RUN unzip wheels-pytorch-py3.11-20241122.zip && \
  pip --no-cache-dir install torch-*.whl triton-*.whl 
WORKDIR /home/jovyan

# 2. Set Environment Variables for CLI Development
# https://github.com/intel/oneapi-containers/blob/master/images/docker/basekit/Dockerfile.ubuntu-22.04
# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/2025.0/oneapi-vars.sh; /usr/local/bin/start-notebook.py"]

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan