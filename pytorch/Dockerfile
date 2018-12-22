# Get the nvidia cuda base container
FROM nvidia/cuda:9.2-devel-ubuntu18.04 

# To allow unicode characters in the terminal
ENV LANG C.UTF-8

# necessary tool
RUN apt-get update --fix-missing && apt-get install -y \
    wget \
    vim \
    git \
    unzip

# set up anaconda
RUN apt-get update --fix-missing && apt-get install -y \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda3-5.0.1-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

ENV PATH /opt/conda/bin:$PATH



# Install basic dependencies
# or [magma-cuda80 | magma-cuda91] depending on your cuda version
RUN /bin/bash -c "\
    export CMAKE_PREFIX_PATH='$(dirname $(which conda))/../' && \
    conda install -y numpy pyyaml mkl mkl-include setuptools cmake cffi typing && \
    conda install -y -c pytorch magma-cuda92 && \
    git clone --recursive https://github.com/pytorch/pytorch"


RUN /bin/bash -c "\
    conda install -y -c conda-forge nodejs jupyterlab && \
    jupyter labextension install @jupyterlab/toc"


RUN /bin/bash -c "\
    export CMAKE_PREFIX_PATH=/opt/conda/bin/../ && \
    cd pytorch && \
    python setup.py install"


