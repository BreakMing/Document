FROM pytorch/pytorch:1.9.1-cuda11.1-cudnn8-runtime AS base

RUN sed -i "s@http://.*archive.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list && \
    sed -i "s@http://.*security.ubuntu.com@http://mirrors.huaweicloud.com@g" /etc/apt/sources.list



# install package
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        curl \
        sudo \
        less \
        emacs \
        apt-utils \
        tzdata \
        git \
        tmux \
        bash-completion \
        command-not-found \
        libglib2.0-0 \
        gstreamer1.0-plugins-* \
        libgstreamer1.0-* \
        libgstreamer-plugins-*1.0-* \
        vim \
        freeglut3-dev \
        liblcm-dev \
        libudev-dev \
        libgl1-mesa-dev \
        libudev1 \
        libudev-dev \
        gnupg \
        libgtk2.0-dev \
        pkg-config \
        rsync \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# LCM
RUN pip install --no-cache-dir lcm==1.4.4 opencv-python opencv-contrib-python

FROM base AS deploy

RUN git clone https://github.com/Improbable-AI/walk-these-ways.git

RUN cd walk-these-ways && \
        rsync -a --exclude=*.pt --exclude=*.mp4 \
        go1_gym_deploy \
        runs \
        go1_gym_deploy/setup.py \
        ../deploy

FROM base

COPY --from=deploy /workspace/deploy /workspace/deploy

COPY lcm_position_amd64 /workspace/deploy/go1_gym_deploy/unitree_legged_sdk_bin/

RUN cd deploy && \
        python setup.py install

