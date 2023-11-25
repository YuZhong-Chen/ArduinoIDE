################################################################################################
# Base Image : https://hub.docker.com/_/ubuntu/tags?page=1&name=22.04
################################################################################################

FROM ubuntu:22.04 as base

LABEL org.opencontainers.image.authors="yuzhong1214@gmail.com"

ARG USERNAME=user
ARG USER_UID=1000
ARG USER_GID=$USER_UID

ENV TERM=xterm-256color
ENV SHELL=/bin/bash
ENV LC_ALL=C.UTF-8

# Create the user and add to dialout group to access serial ports
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && usermod -aG dialout $USERNAME \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && rm -rf /var/lib/apt/lists/*

# Install tools
RUN apt-get update && apt-get install -y \
    curl \
    git \
    vim \
    wget \
    #
    # For arduino
    unzip \
    xz-utils \
    libgtk-3-0 \
    libgbm-dev \
    libsecret-1-0 \
    openjdk-19-jre \
    && rm -rf /var/lib/apt/lists/*

# Copy .bashrc
COPY .bashrc /home/$USERNAME/.bashrc

################################################################################################
# amd64
################################################################################################

FROM base as amd64

# Install arduino
ARG ARDUINO_VERSION=2.2.1
RUN wget https://downloads.arduino.cc/arduino-ide/arduino-ide_${ARDUINO_VERSION}_Linux_64bit.zip \
    && unzip arduino-ide_${ARDUINO_VERSION}_Linux_64bit.zip \
    && rm arduino-ide_${ARDUINO_VERSION}_Linux_64bit.zip \
    && mv arduino-ide_${ARDUINO_VERSION}_Linux_64bit /opt/arduino \
    && ln -s /opt/arduino/arduino-ide /usr/local/bin/arduino

# Other settings
USER $USERNAME
CMD ["/bin/bash"]

################################################################################################
# arm64
################################################################################################

FROM base as arm64

# Install arduino
ARG ARDUINO_VERSION=1.8.19
RUN wget https://downloads.arduino.cc/arduino-${ARDUINO_VERSION}-linuxaarch64.tar.xz \
    && tar -xvf arduino-${ARDUINO_VERSION}-linuxaarch64.tar.xz \
    && rm arduino-${ARDUINO_VERSION}-linuxaarch64.tar.xz \
    && mv arduino-${ARDUINO_VERSION} /opt/arduino \
    && ln -s /opt/arduino/arduino /usr/local/bin/arduino

# Other settings
USER $USERNAME
CMD ["/bin/bash"]