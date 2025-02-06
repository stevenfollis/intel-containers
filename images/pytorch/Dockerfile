FROM stevenfollis/intel-containers:intel-gpu-lts

# Install from binaries
# https://pytorch.org/docs/stable/notes/get_start_xpu.html#binaries
RUN pip install \
  torch==2.6.0 \
  torchvision \
  torchaudio \
  --index-url https://download.pytorch.org/whl/test/xpu