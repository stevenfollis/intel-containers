FROM stevenfollis/intel-containers:base-toolkit-2024.2.1

ARG AI_TOOLS_INSTALLER_URL='https://registrationcenter-download.intel.com/akdlm/IRC_NAS/f27e9e0e-ec27-4024-a4bf-b30c48c99564/l_AITools.2024.2.0.156.sh'

# Download the AI Tools offline installer package
RUN wget --quiet --output-document aitools.sh ${AI_TOOLS_INSTALLER_URL} && \
  sh ./aitools.sh -b && \
  rm aitools.sh

# Extends the default CMD behavior by first running the oneAPI environment variable script then activating AI Tools before continuing to the Jupyter startup
CMD ["/bin/bash", "-c", "source /opt/intel/oneapi/2024.2/oneapi-vars.sh; source /home/jovyan/intel/oneapi/intelpython/bin/activate; /usr/local/bin/start-notebook.py"]
