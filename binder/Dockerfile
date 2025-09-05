FROM nvcr.io/nvidia/nvhpc:25.5-devel-cuda12.9-ubuntu22.04

RUN apt-get update -y
RUN apt-get install -y git pip wget

RUN wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb
RUN dpkg -i cvmfs-release-latest_all.deb
RUN rm -f cvmfs-release-latest_all.deb
RUN apt-get update
RUN apt-get install cvmfs cvmfs-config-default
RUN mkdir -p /cvmfs/sft.cern.ch

# install the notebook package
RUN pip install --no-cache --upgrade pip && \
    pip install --no-cache notebook jupyterlab

RUN pip install --no-cache numba awkward uproot matplotlib h5py hist
RUN pip install --no-cache cupy-cuda12x

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

RUN cd $HOME; git clone --recurse-submodules https://github.com/research-software-collaborations/courses-hsf-india-september2025
RUN rm -rf $HOME/courses-hsf-india-september2025/binder    
WORKDIR ${HOME}
USER ${USER}
