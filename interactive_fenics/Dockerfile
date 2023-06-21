FROM --platform=linux/amd64 firemarmot/xerus AS xerus

FROM quay.io/fenicsproject/stable:2019.1.0.r3
USER root
WORKDIR "/tmp/"

# Non-Python utilities and libraries
RUN apt-get -qq update && \
    apt-get -y install \
    libboost-filesystem-dev \
    libboost-system-dev \
    libboost-python-dev \
    liblapack* \
    libopenblas-dev \
    libiberty-dev \
    binutils-dev \
    zlib1g-dev

# Install Python3 based environment
RUN apt-get -y install \
    python3-dev \
    python3-numpy \
    python3-pip

# Clean apt
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN which python

# Create directories
RUN mkdir -p /usr/local/lib && \
    mkdir -p /usr/local/include

COPY --from=xerus /usr/local/include/suitesparse /usr/local/include/suitesparse
COPY --from=xerus /usr/local/lib/suitesparse /usr/local/lib/suitesparse
COPY --from=xerus /usr/local/include/xerus.h /usr/local/include
COPY --from=xerus /usr/local/include/xerus /usr/local/include/xerus
COPY --from=xerus /usr/local/lib/libxerus_misc.so /usr/local/lib
COPY --from=xerus /usr/local/lib/libxerus.so /usr/local/lib
COPY --from=xerus /tmp/wheels /tmp/wheels

# Install custom Python packages
RUN python3 -m pip install --no-cache --no-deps -vv /tmp/wheels/*

# Set environment variables
USER fenics
ENV LD_LIBRARY_PATH "/usr/local/lib/:/usr/local/lib/suitesparse:$LD_LIBRARY_PATH"

WORKDIR "/home/fenics/"
USER root
