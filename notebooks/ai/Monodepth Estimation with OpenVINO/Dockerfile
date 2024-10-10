FROM stevenfollis/intel-containers:openvino-2024.4.0

# Install pre-requisites
USER root
RUN apt-get update && \
  apt-get install --yes libgl1 && \
  pip install ipywidgets

# Copy notebook and supporting files
RUN mkdir /home/jovyan/notebook/
COPY /vision-monodepth/ /home/jovyan/notebook/

WORKDIR /home/jovyan/notebook/
USER jovyan
