ARG CUDA_VERSION="11.8.0"
ARG CUDNN_VERSION="8"
ARG UBUNTU_VERSION="22.04"
ARG DOCKER_FROM=nvidia/cuda:$CUDA_VERSION-cudnn$CUDNN_VERSION-devel-ubuntu$UBUNTU_VERSION

# Base NVidia CUDA Ubuntu image
FROM $DOCKER_FROM AS base

# Install Python plus openssh, which is our minimum set of required packages.
RUN \
    --mount=type=cache,target=/root/.cache,sharing=locked \
    --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/cache/mirror,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    <<EOF
rm -f /etc/apt/apt.conf.d/docker-clean
echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' \
    > /etc/apt/apt.conf.d/keep-cache
apt-get update -y && \
    apt-get install -y python3 python3-pip python3-venv && \
    apt-get install -y --no-install-recommends openssh-server openssh-client git git-lfs && \
    python3 -m pip install --upgrade pip
EOF

ENV PATH="/usr/local/cuda/bin:${PATH}"

# Install pytorch
ARG PYTORCH="2.0.1"
ARG CUDA="118"
RUN \
    --mount=type=cache,target=/root/.cache,sharing=locked \
    --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/cache/mirror,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    <<EOF
pip3 install -U torch==$PYTORCH torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu$CUDA
EOF

COPY --chmod=755 start-ssh-only.sh /start.sh

WORKDIR /workspace

CMD [ "/start.sh" ]
