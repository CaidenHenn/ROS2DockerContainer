# https://hub.docker.com/_/ros
#FROM ros:galactic

# https://github.com/osrf/docker_images/blob/master/README.md#ros---1
# https://github.com/osrf/docker_images/blob/master/ros/galactic/ubuntu/focal/desktop/Dockerfile
FROM osrf/ros:galactic-desktop

ARG user_id=1000
ARG ros_ws=/home/dockeruser/dev_ws

ENV DEBIAN_FRONTEND=noninteractive
ENV DISPLAY=host.docker.internal:0.0
# Install packages.
RUN apt-get update && apt-get install -y \
    ros-${ROS_DISTRO}-gazebo-* \
    python-is-python3 \
    nano \
    less \
    xterm
#    && rm -rf /var/lib/apt/lists/*
RUN sudo apt update
RUN sudo apt install libxcb-icccm4 libxcb-image0 libxcb-keysyms1 libxcb-render-util0
RUN sudo apt install libxcb-cursor0
RUN sudo apt install ros-galactic-turtlesim
RUN sudo apt-get install libxcb-xinerama0
RUN sudo apt install libxcb-xinerama0
# RUN sudo apt-get -y install gedit



# Switch to a non-root user.
RUN useradd -m --uid ${user_id} dockeruser
USER dockeruser

# Working directory
WORKDIR /home/dockeruser

# ENVs
ENV HOME=/home/dockeruser
ENV PATH="/home/dockeruser/.local/bin:${PATH}"
ENV QT_DEBUG_PLUGINS = 1

#WORKDIR /home/dockeruser/dev_ws

#RUN chmod +x /zelij/zellij
#RUN ./zelij/zellij
# Force Python stdout and stderr streams to be unbuffered.
ENV PYTHONUNBUFFERED 1

# Add sourcing to your shell startup script
# https://docs.ros.org/en/galactic/Tutorials/Configuring-ROS2-Environment.html#add-sourcing-to-your-shell-startup-script
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

# Add `colcon_cd` to your shell startup script
# https://docs.ros.org/en/galactic/Tutorials/Configuring-ROS2-Environment.html#add-colcon-cd-to-your-shell-startup-script
RUN echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc \
    && echo "export _colcon_cd_root=${ros_ws}" >> ~/.bashrc

# This entrypoint is from base image
# See https://github.com/osrf/docker_images/blob/master/ros/galactic/ubuntu/focal/ros-core/ros_entrypoint.sh
ENTRYPOINT ["/ros_entrypoint.sh"]

CMD ["sleep", "infinity"]
WORKDIR /home/dockeruser/dev_ws
RUN mkdir -p src/my_robot_description/urdf
RUN mkdir -p src/my_robot_description/launch
RUN mkdir -p src/my_robot_description/meshes
RUN git clone https://github.com/ros/ros_tutorials.git -b galactic-devel
RUN rosdep update 
# !
# NOTE FOR WHEN YOU FORGET!!: SET THESE ENV MANUALLY
# UNTIL YOU FIGURE OUT HOW TO MAKE THIS WORK
ENV DISPLAY=host.docker.internal:0
RUN export DISPLAY=${DISPLAY:-host.docker.internal:0}

