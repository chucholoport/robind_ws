FROM osrf/ros:kilted-desktop

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=kilted
ENV COLCON_WS=/ws/ur_gz_ws

SHELL ["/bin/bash", "-c"]

# Base tooling (ROS image already includes most things)
RUN apt-get update && apt-get install -y \
    git \
    python3-colcon-common-extensions \
    python3-vcstool \
    python3-rosdep \
    lsb-release \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Official Gazebo (gz) packages for ROS 2 integration
RUN apt-get update && apt-get install -y \
    ros-${ROS_DISTRO}-ros-gz \
    ros-${ROS_DISTRO}-gz-ros2-control \
    ros-${ROS_DISTRO}-ur-robot-driver \
    && rm -rf /var/lib/apt/lists/*

# rosdep
RUN rosdep update

# Workspace
RUN mkdir -p ${COLCON_WS}/src
WORKDIR ${COLCON_WS}

# Clone UR simulation repo
RUN git clone -b kilted \
    https://github.com/UniversalRobots/Universal_Robots_ROS2_GZ_Simulation.git \
    src/ur_simulation_gz

# Dependencies (handled by rosdep as in official workflow)
RUN source /opt/ros/${ROS_DISTRO}/setup.bash && \
    apt-get update && \
    rosdep install \
    --from-paths src \
    --ignore-src \
    -r \
    -y

# Build workspace
RUN source /opt/ros/${ROS_DISTRO}/setup.bash && \
    colcon build --symlink-install

# Environment
RUN echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> /root/.bashrc && \
    echo "source ${COLCON_WS}/install/setup.bash" >> /root/.bashrc

WORKDIR ${COLCON_WS}

CMD ["/bin/bash"]