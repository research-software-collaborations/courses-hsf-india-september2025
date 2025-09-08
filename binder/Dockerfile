FROM nvcr.io/nvidia/nvhpc:25.5-devel-cuda12.9-ubuntu22.04

RUN apt-get update -y
RUN apt-get install -y git pip wget

#a start to getting cvmfs to work - but it doesn't
RUN wget https://ecsft.cern.ch/dist/cvmfs/cvmfs-release/cvmfs-release-latest_all.deb
RUN dpkg -i cvmfs-release-latest_all.deb
RUN rm -f cvmfs-release-latest_all.deb
RUN apt-get update -y
RUN apt-get install -y cvmfs cvmfs-config-default fuse

RUN mkdir -p /cvmfs/cms.cern.ch && \
    echo "cms.cern.ch /cvmfs/cms.cern.ch cvmfs defaults 0 0" >> /etc/fstab && \
    mkdir -p /cvmfs/cms-opendata-conddb.cern.ch && \
    echo "cms-opendata-conddb.cern.ch /cvmfs/cms-opendata-conddb.cern.ch cvmfs defaults 0 0" >> /etc/fstab
ADD cvmfs/cms.cern.ch.local /etc/cvmfs/config.d/cms.cern.ch.local
ADD cvmfs/run-cvmfs.sh /etc/cvmfs/run-cvmfs.sh
RUN chmod uga+rx /etc/cvmfs/run-cvmfs.sh
ADD cvmfs/default.local /etc/cvmfs/default.local



#RUN mkdir -p /cvmfs/sft.cern.ch
#
#RUN echo "CVMFS_REPOSITORIES=sft.cern.ch" > /etc/cvmfs/default.local
#RUN echo "CVMFS_QUOTA_LIMIT=100" >> /etc/cvmfs/default.local

#RUN service autofs start
#RUN cvmfs_config killall
#RUN cvmfs_config setup
#RUN cvmfs_config chksetup
#RUN service autofs stop

#RUN mount -t cvmfs sft.cern.ch /cvmfs/sft.cern.ch

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

ADD     entrypoints/entrypoint-cvmfs.sh /opt/cms/entrypoint.sh
RUN     sudo chmod 755 /opt/cms/entrypoint.sh && \
        sudo chown -R cmsusr /home/cmsusr && \
        chmod 755 /home/cmsusr

ENTRYPOINT ["/opt/cms/entrypoint.sh"]

