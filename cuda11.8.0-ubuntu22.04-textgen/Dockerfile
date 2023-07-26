ARG CUDA_VERSION="11.8.0"
ARG CUDNN_VERSION="8"
ARG UBUNTU_VERSION="22.04"
ARG DOCKER_FROM=chrisoei/cuda$CUDA_VERSION-ubuntu$UBUNTU_VERSION-pytorch:latest 

# Base pytorch image
FROM $DOCKER_FROM as base

WORKDIR /root

# Install text-generation-webui, including all extensions
# Also includes exllama
# We remove the ExLlama automatically installed by text-generation-webui
# so we're always up-to-date with any ExLlama changes, which will auto compile its own extension
RUN \
    --mount=type=cache,target=/root/.cache,sharing=locked \
    --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/cache/mirror,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    <<EOF
git clone https://github.com/oobabooga/text-generation-webui && \
    cd text-generation-webui && \
    pip3 install -r requirements.txt && \
    bash -c 'for req in extensions/*/requirements.txt ; do pip3 install -r "$req" ; done' && \
    pip3 uninstall -y exllama && \
    mkdir -p repositories && \
    cd repositories && \
    git clone https://github.com/turboderp/exllama && \
    pip3 install -r exllama/requirements.txt
EOF

# Install AutoGPTQ, overwriting the version automatically installed by text-generation-webui
ARG AUTOGPTQ="0.3.0"
ENV CUDA_VERSION=""
ENV GITHUB_ACTIONS=true
ENV TORCH_CUDA_ARCH_LIST="8.0;8.6+PTX;8.9;9.0" 
RUN \
    --mount=type=cache,target=/root/.cache,sharing=locked \
    --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/cache/mirror,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    <<EOF
pip3 uninstall -y auto-gptq && \
    pip3 install auto-gptq==$AUTOGPTQ
EOF
