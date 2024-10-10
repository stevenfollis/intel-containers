FROM stevenfollis/intel-containers:aitools-2024.2.0

# Software pre-requisites
RUN conda install --yes --quiet --channel conda-forge \
  accelerate==0.23.0 \
  validators==0.22.0 \
  diffusers==0.18.2 \
  transformers==4.32.1 \
  pillow \
  ipywidgets \
  ipython

# Copy notebook and supporting files
RUN mkdir /home/jovyan/notebook/
COPY /notebook/ /home/jovyan/notebook/
