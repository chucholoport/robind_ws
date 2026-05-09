FROM osrf/ros:jazzy-desktop

ENV DEBIAN_FRONTEND=noninteractive

# Core dev tools
RUN apt update && apt install -y \
    python3-colcon-common-extensions \
    python3-rosdep \
    python3-pip \
    git \
    nano \
    tmux \
    iputils-ping \
    ros-jazzy-rplidar-ros \
    ros-jazzy-ros-gz \
    ros-jazzy-xacro \
    ros-jazzy-joint-state-publisher \
    ros-jazzy-robot-state-publisher \
    ros-jazzy-ros2-control \
    ros-jazzy-ros2-controllers \
    ros-jazzy-controller-manager \
    && rm -rf /var/lib/apt/lists/*

# Initialize rosdep
RUN rosdep init || true
RUN rosdep update

# Clone TurtleBot3 (fixed version)
RUN git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3.git /ros2_ws/src/turtlebot3 && \
    git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git /ros2_ws/src/turtlebot3_msgs && \
    git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git /ros2_ws/src/turtlebot3_simulations

# Clone UR Robotics
RUN git clone -b jazzy https://github.com/UniversalRobots/Universal_Robots_ROS2_Description.git /ros2_ws/src/ur_description && \
    git clone -b kilted https://github.com/UniversalRobots/Universal_Robots_ROS2_GZ_Simulation.git /ros2_ws/src/ur_simulation_gz

# Workspace
WORKDIR /ros2_ws

# Ensure ROS environment always available
SHELL ["/bin/bash", "-c"]
RUN echo "source /opt/ros/jazzy/setup.bash" >> /root/.bashrc && \
    echo "export TURTLEBOT3_MODEL=burger" >> /root/.bashrc && \
    echo "export GAZEBO_MODEL_PATH=\$GAZEBO_MODEL_PATH:/ros2_ws/src/turtlebot3_simulations/turtlebot3_gazebo/models" >> /root/.bashrc

CMD ["bash"]
