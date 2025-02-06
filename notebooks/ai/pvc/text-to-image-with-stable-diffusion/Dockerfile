FROM stevenfollis/intel-containers:aitools-2024.2.0

# Software pre-requisites
# RUN conda install --yes --channel conda-forge \
#   accelerate==0.23.0 \
#   validators==0.22.0 \
#   diffusers==0.18.2 \
#   transformers==4.32.1 \
#   pillow \
#   ipywidgets \
#   ipython

# RUN pip install invisible-watermark transformers huggingface-hub

# Copy notebook and supporting files
RUN mkdir /home/jovyan/notebook/
COPY --chown=jovyan:100 /notebook/ /home/jovyan/notebook/
