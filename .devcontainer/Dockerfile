# ubuntu22.04 ros humble
FROM osrf/ros:humble-desktop-full

# 设置环境变量，防止 apt-get 交互式弹窗
ENV DEBIAN_FRONTEND=noninteractive

# # 设置 Locale
# RUN locale-gen en_US en_US.UTF-8
# RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

# 安装基础依赖
RUN apt-get update && apt-get install -y \
    curl \
    gnupg \
    lsb-release \
    locales \
    sudo \
    build-essential \
    git \
    vim \
    clang-format \
    cpplint \
    bash-completion \
    libcgal-dev \
    python3-pip \
    librange-v3-dev \
    && rm -rf /var/lib/apt/lists/*

# 安装其他常用 ROS 2 包（根据需要添加）
RUN apt-get update && apt-get install -y \
    ros-humble-navigation2 \
    ros-humble-sophus \
    ros-humble-geographic-msgs \
    ros-humble-proxsuite \
    ros-humble-osqp-vendor \
    ros-humble-lanelet2 \
    ros-humble-point-cloud-msg-wrapper \
    ros-humble-ros-testing \
    ros-humble-tl-expected \
    ros-humble-generate-parameter-library \
    ros-humble-rosbag2-storage-mcap \
    && rm -rf /var/lib/apt/lists/* \
    && rm /etc/apt/apt.conf.d/docker-clean

# 其他常用开发工具(根据需要添加)


# --- 创建非 root 用户 (关键部分) ---
# 接收来自 devcontainer.json 的构建参数
ARG USER_UID=1000
ARG USER_GID=1000
ARG USERNAME=ros

# 创建组和用户
RUN groupadd --gid $USER_GID $USERNAME
RUN useradd --uid $USER_UID --gid $USER_GID -m -s /bin/bash $USERNAME

# 授予 sudo 权限
RUN echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME

# 在切换到普通用户前创建 /workspace 并授予权限
RUN mkdir -p /workspace && chown $USER_UID:$USER_GID /workspace

#  --- 切换到新用户 ---
USER $USERNAME
ENV HOME=/home/$USERNAME

# 为新用户创建工作空间目录
RUN mkdir -p /workspace/src
WORKDIR /workspace

# 9. 在新用户的 .bashrc 中配置 ROS 环境
RUN echo "source /opt/ros/humble/setup.bash" >> /home/$USERNAME/.bashrc
RUN echo "if [ -f /workspace/install/setup.bash ]; then source /workspace/install/setup.bash; fi" >> /home/$USERNAME/.bashrc