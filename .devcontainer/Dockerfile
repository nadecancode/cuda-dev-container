ARG PYTHON_VERSION=python-3.9.5
FROM jupyter/base-notebook:$PYTHON_VERSION
USER root

# see https://github.com/phusion/baseimage-docker/issues/319#issuecomment-1058835363
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NOWARNINGS="yes"

RUN apt-get update -y && \
    apt-get -qq install -y --no-install-recommends \
    git \
    curl \
    rsync \
    unzip \
    less \
    nano \
    vim \
    cmake \
    tmux \
    screen \
    gnupg \
    htop \
    wget \
    openssh-client \
    openssh-server \
    p7zip \
    apt-utils \
    jq \
    p7zip-full \
    build-essential \
    netcat \
    && apt-get clean && rm -rf /var/lib/apt/lists/* && \
    chmod g-s /usr/bin/screen && \
    chmod 1777 /var/run/screen

######################################
# Now add in CUDA-11.8 tools/libraries
COPY --from=nvcr.io/nvidia/cuda:11.8.0-devel-ubuntu20.04 /usr/local/cuda-11.8 /usr/local/cuda-11.8
RUN ln -s cuda-11.8 /usr/local/cuda && ln -s cuda-11.8 /usr/local/cuda-11

# Configure dynamic library locations (similar to LD_LIBRARY_PATH)
RUN echo '/usr/local/cuda/targets/x86_64-linux/lib' >> /etc/ld.so.conf.d/000_cuda.conf && \
    echo '/usr/local/cuda-11/targets/x86_64-linux/lib' >> /etc/ld.so.conf.d/989_cuda-11.conf && \
    ( echo '/usr/local/nvidia/lib'; echo '/usr/local/nvidia/lib64' ) >> /etc/ld.so.conf.d/nvidia.conf && \
    ldconfig

ENV CUDA_HOME=/usr/local/cuda

ENV PATH="${CUDA_HOME}/bin:${PATH}"