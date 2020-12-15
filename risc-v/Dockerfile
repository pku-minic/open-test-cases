# Dockerfile for building an image to compile & run RISC-V programs

# we love Debian
FROM debian:buster-slim

# some arguments
ARG HOME_DIR=/root
ARG SYSYRT_REPO_URL=https://github.com/pku-minic/sysy-runtime-lib.git
ARG SYSYRT_REPO_PATH=${HOME_DIR}/sysy-runtime-lib
ARG SYSYRT_PATH=${HOME_DIR}/libsysy.a
ARG RISCV32_PATH=/opt/riscv

# change APT sources
# for Mainland China users only, otherwise you can delete the following lines
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian buster main contrib non-free" > /etc/apt/sources.list && \
    echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian buster-updates main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian-security/ buster/updates main contrib non-free" >> /etc/apt/sources.list

# install dependencies
RUN apt-get update && apt-get install -y \
    build-essential git flex bison cmake qemu-user-static

# pull SysY runtime library
RUN git clone --recursive --shallow-submodules --single-branch --depth 1 \
    ${SYSYRT_REPO_URL} ${SYSYRT_REPO_PATH}
RUN cp ${SYSYRT_REPO_PATH}/libsysy.a ${SYSYRT_PATH} && \
    rm -rf ${SYSYRT_REPO_PATH}

# copy 32-bit RISC-V GNU toolchain from 'riscv32-toolchain'
COPY --from=maxxing/riscv32-toolchain ${RISCV32_PATH} ${RISCV32_PATH}
ENV RISCV=${RISCV32_PATH}
ENV PATH=$RISCV/bin:$PATH

WORKDIR ${HOME_DIR}
