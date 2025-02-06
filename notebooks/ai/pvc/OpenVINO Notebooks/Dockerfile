# Using a multi-stage build to first download the entire notebooks repo, 
# then pluck specific books for the final stage

# ============================================
# Gitter stage
# ============================================
FROM alpine:latest AS gitter
RUN apk update && apk upgrade && apk add git
RUN git clone --depth=1 https://github.com/openvinotoolkit/openvino_notebooks.git

# ============================================
# Done stage
# ============================================
FROM stevenfollis/intel-containers:openvino-11d0903

# Setup project pre-requisites
# https://github.com/openvinotoolkit/openvino_notebooks/wiki/Ubuntu

USER root

# https://github.com/openvinotoolkit/openvino_notebooks/wiki/Ubuntu
RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  python3-venv \
  build-essential \
  python3-dev \
  git-all \
  libgl1-mesa-dev \
  ffmpeg && \
  rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir ipywidgets

# Copy in notebooks
COPY --from=gitter --chown=jovyan:100 /openvino_notebooks/notebooks/ notebooks/

USER jovyan