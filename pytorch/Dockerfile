# Get the nvidia cuda base container
FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04

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

RUN /bin/bash -c "\
    conda install -y -c conda-forge nodejs jupyterlab && \
    jupyter labextension install @jupyterlab/toc"



RUN /bin/bash -c "\
    conda install -y pytorch torchvision -c pytorch"

# Solves: `libjpeg.so.8: cannot open shared object file: No such file or directory`
#          after `from PIL import Image`
RUN apt-get install -y libjpeg-turbo8

RUN jupyter notebook --generate-config --allow-root && \
    echo "c.NotebookApp.ip = '0.0.0.0'" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.open_browser = False" >> ~/.jupyter/jupyter_notebook_config.py &&\
    echo "c.NotebookApp.token = ''" >> ~/.jupyter/jupyter_notebook_config.py

WORKDIR /code

CMD /bin/bash -c "source activate fastai && CUDA_VISIBLE_DEVICES=0 jupyter notebook --port=8888 --allow-root"


# Install basic dependencies
# or [magma-cuda80 | magma-cuda91] depending on your cuda version
# RUN /bin/bash -c "\
#     export CMAKE_PREFIX_PATH='$(dirname $(which conda))/../' && \
#     conda install -y numpy pyyaml mkl mkl-include setuptools cmake cffi typing && \
#     conda install -y -c pytorch magma-cuda90 && \
#     git clone --recursive https://github.com/pytorch/pytorch"

# RUN /bin/bash -c "\
#     export CMAKE_PREFIX_PATH=/opt/conda/bin/../ && \
#     cd pytorch && \
#     python setup.py install"


