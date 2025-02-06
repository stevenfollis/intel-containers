FROM stevenfollis/intel-containers:openvino-2024.4.0

# Install pre-requisites
USER root

RUN apt-get update && \
  apt-get install --yes --no-install-recommends \
  libgl1 && \
  rm -rf /var/lib/apt/lists/* && \
  pip install --no-cache-dir ipywidgets

USER jovyan

# Copy notebook and supporting files
RUN mkdir /home/jovyan/notebook/
COPY --chown=jovyan:100 /vision-monodepth/ /home/jovyan/notebook/