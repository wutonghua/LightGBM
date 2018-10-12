FROM microsoft/vsts-agent:ubuntu-16.04

# Install basic command-line utilities
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    curl \
    locales \
    sudo \
    unzip \
    wget \
    zip \
    graphviz \
 && rm -rf /var/lib/apt/lists/*

# Setup the locale
ENV LANG en_US.UTF-8
ENV LC_ALL $LANG
RUN locale-gen $LANG \
 && update-locale

# Install essential build tools
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    build-essential \
 && rm -rf /var/lib/apt/lists/*

# Downgrade gcc version
RUN apt-get remove -y gcc \ 
 && add-apt-repository ppa:ubuntu-toolchain-r/test -y \
 && sudo apt-get update \
 && sudo apt-get install --no-install-recommends -y g++-4.8 \
 && sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 100 \
 && sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 100

# Install CMake
RUN curl -sL https://cmake.org/files/v3.12/cmake-3.12.0-Linux-x86_64.sh -o cmake.sh \
 && chmod +x cmake.sh \
 && ./cmake.sh --prefix=/usr/local --exclude-subdir \
 && rm cmake.sh

# Install Miniconda
RUN curl -sL https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh -o miniconda.sh \
 && chmod +x miniconda.sh \
 && ./miniconda.sh -b -p /usr/local/miniconda \
 && rm miniconda.sh \
 && chmod -R 777 /usr/local/miniconda \
 && /usr/local/miniconda/bin/conda install python=3 -q -y \
 && /usr/local/miniconda/bin/conda clean -a -y
ENV CONDA=/usr/local/miniconda

# Clean system
RUN apt-get clean \
 && rm -rf /var/lib/apt/lists/* \
 && rm -rf /etc/apt/sources.list.d/* \
 && rm -rf /tmp/*
