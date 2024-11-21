FROM stevenfollis/intel-containers:intel-gpu-lts

ARG OPENNVINO_VERSION='2024.4.0'
ARG OPENVINO_INSTALLER_URL='https://storage.openvinotoolkit.org/repositories/openvino_genai/packages/2024.4/linux/openvino_genai_ubuntu22_2024.4.0.0_x86_64.tar.gz'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

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
RUN python3 -m pip install --no-cache-dir --requirement /opt/intel/openvino_2024.4.0/python/requirements.txt
USER root

# Create a symbolic link as below:
RUN cd /opt/intel && ln -s openvino_2024.4.0 openvino_2024

# Install OpenCV
# https://github.com/opencv/opencv/wiki/BuildOpenCV4OpenVINO#installing-on-ubuntu
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  libopencv-dev \
  python3-opencv && \
  rm -rf /var/lib/apt/lists/*

# Extends the default CMD behavior by first running the oneAPI environment variable script then continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/openvino_2024/setupvars.sh; /usr/local/bin/start-notebook.py"]

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan