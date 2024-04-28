FROM ghcr.io/linuxserver/baseimage-kasmvnc:debianbookworm@sha256:5b936c70a86b1752f9622b32ceee285be6892a4ddce21cbb464c3d31fb1e5417

RUN rm -f /etc/apt/apt.conf.d/docker-clean; echo 'Binary::apt::APT::Keep-Downloaded-Packages "true";' > /etc/apt/apt.conf.d/keep-cache
RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
    --mount=type=cache,target=/var/lib/apt,sharing=locked \
    apt update \
    && apt install -y --no-install-recommends \
    build-essential git patch wget unzip \
    gettext autoconf automake cmake libtool libtool-bin nasm luarocks lua5.1 libsdl2-dev \
    libssl-dev libffi-dev libc6-dev xutils-dev linux-libc-dev zlib1g \
    ccache

ARG USERNAME
ARG UID=1000

USER ${USERNAME}
ENV HOME=/home/${USERNAME}

RUN mkdir -p ~/.luarocks \
    && cp /etc/luarocks/config.lua $HOME/.luarocks/config.lua \
    && echo "wrap_bin_scripts = false" >> $HOME/.luarocks/config.lua \
    && luarocks --local install busted 2.0.0-1

# For running the linuxserver init
USER root
