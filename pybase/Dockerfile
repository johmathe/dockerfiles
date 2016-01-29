FROM nvidia/cuda:7.0-cudnn2-runtime

MAINTAINER Johan Mathe <johan.mathe@gmail.com>

# Pick up some TF dependencies
RUN apt-get update && apt-get install -y \
        curl \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python-numpy \
        python-pip \
        python-scipy \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN curl -O https://bootstrap.pypa.io/get-pip.py && \
    python get-pip.py && \
    rm get-pip.py

RUN pip --no-cache-dir install \
        ipykernel \
        jupyter \
        matplotlib \
        && \
    python -m ipykernel.kernelspec

# Set up our notebook config.
COPY jupyter_notebook_config.py /root/.jupyter/
RUN curl -O http://us.download.nvidia.com/XFree86/Linux-x86_64/352.41/NVIDIA-Linux-x86_64-352.41.run
RUN chmod 755 ./NVIDIA-Linux-x86_64-352.41.run
RUN ./NVIDIA-Linux-x86_64-352.41.run -N -s --no-kernel-module
# Jupyter has issues with being run directly:
#   https://github.com/ipython/ipython/issues/7062
# We just add a little wrapper script.
COPY run_jupyter.sh /

# IPython
EXPOSE 8888

WORKDIR "/root"

CMD ["/bin/bash"]
