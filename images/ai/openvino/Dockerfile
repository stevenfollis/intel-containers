# https://github.com/jupyter/docker-stacks/blob/main/images/minimal-notebook/Dockerfile
# FROM jupyter/minimal-notebook:latest
FROM stevenfollis/intel-containers:minimal-notebook-2024-10-14

ARG INTEL_GPU_DRIVER_URL='https://repositories.intel.com/gpu/ubuntu/dists/jammy/lts/2350/intel-gpu-ubuntu-jammy-2350.run'

ARG OPENNVINO_VERSION='2024.4.0'
ARG OPENVINO_INSTALLER_URL="https://storage.openvinotoolkit.org/repositories/openvino_genai/packages/2024.4/linux/openvino_genai_ubuntu22_2024.4.0.0_x86_64.tar.gz"

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

# ============================================
# Install OpenVINO GenAi Archive Package
# https://docs.openvino.ai/2024/get-started/install-openvino/install-openvino-genai.html#archive-installation
# ============================================

# Download the OpenVINO Runtime archive file for your system, extract the files, rename the extracted folder and move it to the desired path:
RUN mkdir --parents /opt/intel/openvino_2024.4.0
RUN wget \
  --quiet \
  --output-document=- \
  ${OPENVINO_INSTALLER_URL} \
  | \
  tar \
  --extract \
  --gzip \
  --directory=/opt/intel/openvino_2024.4.0 \
  --strip-components=1

# Install required system dependencies on Linux. To do this, OpenVINO provides a script in the extracted installation directory. Run the following command:
RUN /opt/intel/openvino_2024.4.0/install_dependencies/install_openvino_dependencies.sh -y

# Install numpy Python Library:
USER jovyan
RUN python3 -m pip install --requirement /opt/intel/openvino_2024.4.0/python/requirements.txt
USER root

# Create a symbolic link as below:
RUN cd /opt/intel && ln -s openvino_2024.4.0 openvino_2024

# Install OpenCV
# https://github.com/opencv/opencv/wiki/BuildOpenCV4OpenVINO#installing-on-ubuntu
RUN apt update && \
  apt install --yes \
  libopencv-dev \
  python3-opencv && \
  rm -rf /var/lib/apt/lists/*

# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/openvino_2024/setupvars.sh; /usr/local/bin/start-notebook.py"]

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan