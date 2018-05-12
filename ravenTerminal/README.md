
### Running the Raven II from a container

# Definitions
Raven Computer - The computer that will actually run the Raven II software and is attached to the Raven II
Host Computer  - The computer that we will open the container on and that we want to use to run the Raven from remotely
Docker         - Docker is a piece of software that allows on to run a command line of a virtual linux machine on many modern PCs

## Setup Raven II computer

# Install Raven II on machine
Follow the wiki instructions to install the Raven II source code

# find IP addresses
get the raven's ip address by running on the raven machine (or use URL if you know it)

    ifconfig
or 

    ip addr
and on the hose computer do the same thing

## Setup container
Follow Docker instructions to install Docker

# Download image
    docker pull abrahq:raven_terminal:latest
or build from source file (recommended <IMname>=raven <tag>=latest)

    docker build -t <IMname>:<tag> <path to dockerfile>

# Create container with mount and network conection (you can change the ip address if you desire, you will just need to know it later)

    docker create --mount <path to work>:/root/catkin_ws/src/<desired folder name> \
                  --name raven_terminal --ip 172:17.0.1 \
                  <IMname>

# Start container
    docker start -ait -p 11111:11111 <containerID>
start is the command to start running a container in persistant mode
-a will attach the container to the terminal
-i will make the terminal persistent
-t will use psuedo-tty for terminal
-ait = -a -i -t
-p 11111:11111 should forward all conections to port 11111 on the host computer to port 11111 in the container
*start a second terminal to the same container*

    docker exec -it <containerID>
exec will open a terminal to a container that is already started

## Run Project

# start the Raven II
*we will use one of the container terminals for this section*

The first thing we will need to do is set up the ros message address

    export ROS_MASTER_URI=<host ip address>:11111
    export ROS_IP=<raven ip address>:11111    

This tells the system where to listen for messages and where to send messages
Next we need to ssh into the raven computer.

    ssh user@<raven ip address>
Now we need to tell the raven computer where to send and listen for messages

    export ROS_MASTER_URI=<host ip address>:11111
    export ROS_IP==<host ip address>:11111

note: these should get forwarded into the container, this can be difficult to get right so if things aren't working look back to this section.

*start the raven*
Now we need to start the raven code, still in the ssh terminal

    sudo -i # if neccessary
    /etc/init.d/brl_usb start
    roscd raven_2
    roslaunch raven_2 raven_2.launch
Home the robot (take it out of estop and let it home)

*in the other container terminal*
check that the container is receiving the ros messages

    rostopic list
    rostopic echo /ravenstate -c
This should show the raven messages and then the current raven state. Stop rostopic with ctl+c.

# start project

    roscd <project>
    roslaunch project project.launch

## References
* https://github.com/melodysu83/AutoCircle_generater


