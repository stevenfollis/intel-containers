FROM stevenfollis/intel-containers:intel-gpu-lts

# Install PyTorch for Intel® Data Center GPU Max Series
# https://pytorch.org/docs/main/notes/get_start_xpu.html
RUN pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/test/xpu

# Copy Jupyter notebook and supporting files into container image
COPY --chown=jovyan:100 . /home/jovyan/