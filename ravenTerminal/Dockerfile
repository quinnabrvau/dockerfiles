# install ros
FROM ros:kinetic-ros-base-xenial
RUN apt-get update
RUN apt-get install -y ros-kinetic-perception=1.3.1-0*
RUN apt-get install -y nano

# add ros path to bash profile
RUN echo "source /opt/ros/kinetic/setup.bash" >> /root/.bashrc

# create catkin workspace
RUN /bin/bash -c 'source /opt/ros/kinetic/setup.bash && \
    mkdir -p /root/catkin_ws/src && cd /root/catkin_ws && catkin_make && \
    echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc'

# install raven source
RUN /bin/bash -c 'source /opt/ros/kinetic/setup.bash && \
    cd /root/catkin_ws/src && \
    git clone https://github.com/uw-biorobotics/raven2.git -b test'

#install ros repos that may be help
RUN /bin/bash -c 'source /opt/ros/kinetic/setup.bash &&\
    cd /root/catkin_ws/src && \
    git clone https://github.com/ros-drivers/usb_cam.git && \
    git clone https://github.com/pal-robotics/aruco_ros.git -b aruco-3.0.4 && \
    cd .. && catkin_make'

ENTRYPOINT /bin/bash

EXPOSE 11111

# These will help debug network connection issues that may arise 
RUN apt-get install -y iputils-ping tcpdump openssh-client

RUN /bin/bash -c 'cd /root/catkin_ws/src && mkdir work'

RUN rm -rf /var/lib/apt/lists/*



