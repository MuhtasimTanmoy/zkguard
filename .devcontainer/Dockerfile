FROM ubuntu:22.04

# Non-interactive
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    curl \
    git \
    ca-certificates \
    python3 \
    python3-pip \
    python3-venv \
    pkg-config \
    libssl-dev \
    cmake \
    clang \
    libclang-dev \
    libgmp-dev \
    unzip \
    wget \
    locales \
    sudo \
    jq \
 && rm -rf /var/lib/apt/lists/*

# Set locale
RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8

# Install Rust (rustup) and set up stable toolchain
ENV RUSTUP_HOME=/usr/local/rustup
ENV CARGO_HOME=/usr/local/cargo
ENV PATH=$CARGO_HOME/bin:$PATH

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --no-modify-path
RUN rustup default stable

# Create a non-root user for container development
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
 && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
 && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
 && chmod 0440 /etc/sudoers.d/$USERNAME \
 && chown -R $USERNAME:$USERNAME /home/$USERNAME

 

USER $USERNAME
WORKDIR /workspace

SHELL ["/bin/bash", "-lc"]

## Note: Installing `nargo` (Noir) can be slow and may require extra build deps.
## We choose to run it in post-create (devcontainer.json) so the container builds fast
## but the toolchain is installed when first opening the container.
