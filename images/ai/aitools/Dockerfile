FROM stevenfollis/intel-containers:base-toolkit-2025.0.1

ARG AI_TOOLS_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/491d5c2a-67fe-48d0-884f-6aecd88f5d8a/ai-tools-2025.0.0.75_offline.sh'

# Set root user to configure environment
USER root
ENV DEBIAN_FRONTEND=noninteractive

# Download the AI Tools offline installer package
RUN wget --quiet --output-document aitools.sh ${AI_TOOLS_INSTALLER_URL} && \
  sh ./aitools.sh \
    -b \
    -f \
    -p '/opt/intel/oneapi' && \
  rm aitools.sh

# Extends the default CMD behavior by first running the oneAPI environment variable script then activating AI Tools before continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/2025.0/oneapi-vars.sh; source /home/jovyan/intel/oneapi/intelpython/bin/activate; /usr/local/bin/start-notebook.py"]

# ============================================
# Cleanup and reset values back to base image
# ============================================

# Reset default user
ENV DEBIAN_FRONTEND=interactive
USER jovyan
