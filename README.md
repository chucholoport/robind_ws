# robind_ws

ROS 2 Kilted development environment for robotics projects using Docker.

This repository provides a preconfigured Ubuntu desktop container with:

* ROS 2 Kilted Kaiju
* Gazebo Ionic
* Development utilities
* Docker helper scripts
* Persistent ROS 2 workspace support

The main goal of this repository is to simplify robotics development by abstracting Docker commands behind a lightweight helper command:

```bash
robot dev
```

This allows students and developers to start a complete robotics development environment without manually managing Docker containers.

---

# Features

* Preconfigured ROS 2 Kilted environment
* Gazebo Ionic support
* Desktop-enabled Docker container
* Persistent ROS 2 workspace mounting
* Simple container lifecycle management
* Minimal setup for robotics courses and labs

---

# Repository Structure

```text
robind_ws/
├── ros2_ws/
│   └── src/
│
└── .docker/
    ├── scripts/
    │   ├── robot
    │   ├── dev
    │   └── rm
    │
    └── ubuntu_desktop/
        ├── Dockerfile
        └── docker-compose.yml
```

---

# Requirements

* Ubuntu 24.04
* Docker Engine
* Docker Compose

---

# Installation

Clone the repository:

```bash
git clone https://github.com/chucholoport/robind_ws.git
cd robind_ws
```

---

# Enable the `robot` Command

Add the following function to your shell configuration file:

For Bash:

```bash
nano ~/.bashrc
```

For Zsh:

```bash
nano ~/.zshrc
```

Append:

```bash
# Automatically enable the robot command inside robotics projects
robot() {
    if [ -f ".docker/scripts/robot" ]; then
        bash .docker/scripts/robot "$@"
    else
        echo "robot: not inside a robotics project"
    fi
}
```

Reload the shell:

```bash
source ~/.bashrc
```

or

```bash
source ~/.zshrc
```

---

# Usage

## Start Development Environment

Inside the repository root:

```bash
robot dev
```

The script automatically:

* Builds the Docker image if necessary
* Starts the container
* Reuses existing containers
* Opens an interactive ROS 2 shell
* Sources the ROS 2 environment

---

## Remove Container

```bash
robot rm
```

This removes the development container and frees Docker resources.

---

# Workspace

The ROS 2 workspace is located at:

```text
ros2_ws/
```

Inside the container, the workspace is mounted at:

```text
/ws/ur_gz_ws
```

---

# Included Environment

The development container includes:

* Ubuntu 24.04
* ROS 2 Kilted Kaiju
* Gazebo Ionic
* Colcon
* ROS development tools
* GUI forwarding support through X11

---

# Example

Launch the development environment:

```bash
cd robind_ws
robot dev
```

Inside the container:

```bash
ros2 --version
```

Launch a simulation:

```bash
ros2 launch ur_simulation_gz ur_sim_control.launch.py
```

---

# Design Philosophy

This repository is designed for:

* Robotics laboratories
* Embedded and robotics courses
* Rapid ROS 2 onboarding
* Reproducible development environments
* Simplified Docker workflows for students

The helper scripts intentionally minimize the amount of Docker knowledge required to begin developing robotics applications.

---

# License

This project is licensed under the MIT License.

---

# Author

**Jesus Salvador Lopez Ortega**

Digital Systems & Robotics Engineer, graduated from [Tecnologico de Monterrey Campus Queretaro](https://tec.mx/es/queretaro/)

Software & Robotics professor at [Universidad Politecnica de Santa Rosa](https://upsrj.edu.mx/)

**Contact:**
- [LinkedIn](https://www.linkedin.com/in/jesus-salvador-lopez-ortega/)
- [GitHub](https://github.com/chucholoport)