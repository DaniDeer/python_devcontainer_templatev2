# Use a distro with minimal configuration, e.g. Debian slim as a base image
FROM debian:bookworm-20231030-slim as devcontainer
# Operate APT in a non-interactive mode to prevent prompting the user for input
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        ca-certificates=20230311 \
        curl=7.88.1-10+deb12u4 \
        git=1:2.39.2-1.1 \
        gnupg=2.2.40-1.1 \
        libcurl4-openssl-dev=7.88.1-10+deb12u4 \
        locales-all=2.36-9+deb12u3 \
        locales=2.36-9+deb12u3 \
        # nvi: the only editor you need...
        nvi=1.81.6-17+b2 \
        openssh-client=1:9.2p1-2+deb12u1 \
        software-properties-common=0.99.30-4 \
        sudo=1.9.13p3-1+deb12u1 \
        zip=3.0-13 \
        zlib1g-dev=1:1.2.13.dfsg-1 && \
        # add other dependencies ....
    rm -rf /var/lib/apt/lists/*

# Set the locale
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Add some certificates if needed...
#COPY My_Certs.crt /usr/local/share/ca-certificates/My_Certs.crt

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && mkdir /workspace \
    && chown $USERNAME /workspace \
    && mkdir /workspace/tmp \
    && chown $USERNAME /workspace/tmp \
    # add other directories ...

# Install the rest as user and set the working directory to /workspace/tmp
USER $USERNAME
WORKDIR /workspace/tmp

# Copy the local code to the container
COPY . /workspace/tmp

# Add some RUN statements that have now access to the repository files...

# Remove temporary files for the build
RUN rm -rf workspace/tmp
