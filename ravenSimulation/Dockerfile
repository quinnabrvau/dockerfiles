FROM ros:kinetic-robot-xenial

RUN apt-get update

RUN apt-get install -y software-properties-common python-software-properties apt-utils
RUN add-apt-repository ppa:paul-bartell/biorobotics
RUN apt-get update && apt-get install -y brl-usb-dkms apt-utils nano

RUN cd /root/ && git clone https://github.com/eigenteam/eigen-git-mirror.git eigen && \
    cd eigen && mkdir build && cd build && cmake .. && make install

# #NEW CATKIN_MAKE BUILD CMD
RUN /bin/bash -c 'echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc'

RUN /bin/bash -c 'mkdir -p /root/catkin_ws/src && cd /root/catkin_ws/src && \
    git clone https://github.com/uw-biorobotics/raven2.git -b test'

RUN /bin/bash -c 'echo "export PATH=/opt/ros/kinetic/bin:$PATH" >> /root/.bashrc && \
    source /root/.bashrc'

RUN /bin/bash -c 'source /opt/ros/kinetic/setup.bash && \
    export PATH=/opt/ros/kinetic/bin:$PATH && \
    echo $PATH && source /root/.bashrc && \
    cd /root/catkin_ws && catkin_make'
RUN /bin/bash -c 'echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc && \
    source /root/.bashrc'

# #Install simulation
ENV NB_USER=jovyan
ENV HOME=/home/$NB_USER
RUN mkdir -p /home/$NB_USER/work

RUN /bin/bash -c 'cd /$HOME/ && \
    git clone https://github.com/UVA-DSA/raven2_sim.git raven_2'

RUN /bin/bash -c 'cd /$HOME/raven_2 && \
    tar zxvf ./teleop_data/new_test_data.tgz'

RUN echo 'export ROS_PACKAGE_PATH=/$HOME/raven_2:$HOME/raven_2/raven_visualization:ROS_PACKAGE_PATH' >> ~/.bashrc 

EXPOSE 11111
EXPOSE 8080

# These will help debug network connection issues that may arise 
RUN apt-get install -y iputils-ping tcpdump iproute

RUN apt-get install -y python-pip
RUN pip install jupyter 
RUN pip install numpy matplotlib scipy ipython pandas sympy nose
RUN pip install jupyterlab jupyterlab-launcher 

RUN apt-get install -y wget

RUN apt-get install -yq --no-install-recommends \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    fonts-liberation

ENV DEBIAN_FRONTEND noninteractive

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

# Install Tini
RUN wget --quiet https://github.com/krallin/tini/releases/download/v0.10.0/tini && \
    echo "1361527f39190a7338a0b434bd8c88ff7233ce7b9a4876f3315c22fce7eca1b0 *tini" | sha256sum -c - && \
    mv tini /usr/local/bin/tini && \
    chmod +x /usr/local/bin/tini

ENV SHELL=/bin/bash  \
    NB_UID=1000 \
    NB_GID=100 \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8

ADD fix-permissions /usr/local/bin/fix-permissions

RUN chmod +x /usr/local/bin/fix-permissions

RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    chown $NB_USER:$NB_GID $HOME && \
    chmod g+w /etc/passwd /etc/group && \
    fix-permissions $HOME

USER $NB_UID
RUN fix-permissions /home/$NB_USER

USER root
WORKDIR $HOME
ENTRYPOINT ["tini", "--"]
CMD ["start-notebook.sh"]

COPY start.sh /usr/local/bin/
COPY start-notebook.sh /usr/local/bin/
COPY start-singleuser.sh /usr/local/bin/
COPY jupyter_notebook_config.py /etc/jupyter/
RUN fix-permissions /etc/jupyter/
RUN rm -rf /var/lib/apt/lists/*
USER $NB_UID
