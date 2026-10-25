FROM osrf/ros:humble-desktop

ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=1000

ENV DEBIAN_FRONTEND=noninteractive

# ros-gz = current Gazebo (Fortress) bridge; Gazebo Classic is end-of-life
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo \
    python3-pip \
    python3-colcon-common-extensions \
    python3-rosdep \
    ros-humble-ros-gz \
    ros-humble-joint-state-publisher \
    ros-humble-robot-state-publisher \
    ros-humble-xacro \
    ros-humble-teleop-twist-keyboard \
    mesa-utils \
    x11-apps \
    libgl1-mesa-dri \
    libegl1 \
    vulkan-tools \
    bc \
    && rm -rf /var/lib/apt/lists/*

# Non-root user so files written to /workspace are not root-owned
RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && (getent group render >/dev/null || groupadd -r render) \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m -s /bin/bash ${USERNAME} \
    && usermod -aG video,render ${USERNAME} \
    && echo "${USERNAME} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}

RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc \
    && echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc \
    && echo '[ -f /workspace/ros2_ws/install/setup.bash ] && source /workspace/ros2_ws/install/setup.bash' >> ~/.bashrc

WORKDIR /workspace
