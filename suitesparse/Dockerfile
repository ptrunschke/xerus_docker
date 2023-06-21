FROM --platform=linux/amd64 firemarmot/base:latest
USER root
WORKDIR "/tmp/"

# RUN apt-get update
# RUN apt-get install libsuitesparse-dev

# Install Suitesparse
RUN git clone https://github.com/jluttine/suitesparse.git
WORKDIR "/tmp/suitesparse/"
RUN git checkout v5.2.0 && \
    make install -j $(cat /tmp/NCORES_1) && \
    mv include /usr/local/include/suitesparse && \
    mv lib/ /usr/local/lib/suitesparse
ENV LD_LIBRARY_PATH "/usr/local/lib/suitesparse:$LD_LIBRARY_PATH"
