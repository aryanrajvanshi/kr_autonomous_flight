FROM kumarrobotics/autonomy:base

# Fix ROS apt repo key (prevents EXPKEYSIG / 404 failures)
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl gnupg2 ca-certificates lsb-release \
 && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc \
    | gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg \
 && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" \
    > /etc/apt/sources.list.d/ros1-latest.list

RUN apt-get update \
    && apt-get install -y \
    ros-noetic-mavros \
    ros-noetic-mavros-msgs \
    ros-noetic-mavros-extras \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir -p /root/control_ws/src
WORKDIR /root/control_ws
COPY autonomy_core/control/ src/
RUN catkin init && catkin config --extend /opt/ros/noetic
RUN vcs import --input src/external-repos.yaml src/
RUN catkin build -j2 --no-status -DCMAKE_BUILD_TYPE=Release

COPY autonomy_core/control/docker/entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
