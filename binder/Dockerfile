FROM nvcr.io/nvidia/nvhpc:25.5-devel-cuda12.9-ubuntu22.04

RUN apt-get update -y
RUN apt-get install -y git pip

# install the notebook package
RUN pip install --no-cache --upgrade pip && \
    pip install --no-cache notebook jupyterlab

# create user with a home directory
ARG NB_USER
ARG NB_UID
ENV USER ${NB_USER}
ENV HOME /home/${NB_USER}
ENV SHELL /bin/bash
RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${NB_UID} \
    ${NB_USER}

RUN cd $HOME; git clone --recurse-submodules https://github.com/iris-hep/codas-hep-2025
RUN rm -rf $HOME/codas-hep-2025/binder    
WORKDIR ${HOME}
USER ${USER}
