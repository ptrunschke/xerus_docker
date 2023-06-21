FROM --platform=linux/amd64 firemarmot/suitesparse:latest
USER root
WORKDIR "/tmp/"

# Install Xerus
RUN git clone --recurse-submodules https://github.com/libxerus/xerus.git --branch conda
WORKDIR "/tmp/xerus/"
RUN cp ./docker/xerus/config.mk .
RUN make python -j $(cat /tmp/NCORES_1)
RUN mkdir -p /usr/local/include
RUN cp include/xerus.h /usr/local/include/
RUN cp -r include/xerus /usr/local/include/
RUN mkdir -p /usr/local/lib
RUN cp build/libxerus_misc.so /usr/local/lib/
RUN cp build/libxerus.so /usr/local/lib/
RUN python3 -m pip wheel --no-cache-dir --no-deps --wheel-dir /tmp/wheels -vv .
RUN python3 -m pip install --no-cache --no-deps -vv /tmp/wheels/*

ENV LD_LIBRARY_PATH "/usr/local/lib/:$LD_LIBRARY_PATH"
