ARG CUDA_VERSION="11.8.0"
ARG CUDNN_VERSION="8"
ARG UBUNTU_VERSION="22.04"

# Base image
FROM chrisoei/cuda$CUDA_VERSION-ubuntu$UBUNTU_VERSION-oneclick:latest as base

RUN \
    --mount=type=cache,target=/root/.cache,sharing=locked \
    --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/cache/mirror,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    <<EOF
pip3 install runpod requests
EOF

COPY scripts/rp_handler.py /root/scripts

COPY --chmod=755 start-with-ui.sh /start.sh

WORKDIR /workspace

CMD [ "/start.sh" ]
