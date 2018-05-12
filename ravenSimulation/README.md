## Install
start by downloading the root of the system

    docker pull abrahq/raven_simulation
or build from source (not recommended). Run

    git clone https://github.com/quinnabrvau/dockerfiles.git
    cd dockerfiles/ravenSimulation
    docker build -t <TAG> .

## Run the simulation
The simulation has a graphical component so in order to get around this requirement, we are going to use jupyter network to allow us to view the results in the host machine. 

    docker run -it --rm -p 8888:8888 <TAG>
*`<TAG>` can be replaced with abrahq/raven_simulation if you used pull*

This will return a line that looks like 

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://da80702d9df5:8888/?token=5cda8eaf59bb2ee89df98181c5ae53ef4fd1a43f03fb6ab4&token=5cda8eaf59bb2ee89df98181c5ae53ef4fd1a43f03fb6ab4

In a web browser goto

    http://localhost:8888/?token=5cda8eaf59bb2ee89df98181c5ae53ef4fd1a43f03fb6ab4&token=5cda8eaf59bb2ee89df98181c5ae53ef4fd1a43f03fb6ab4