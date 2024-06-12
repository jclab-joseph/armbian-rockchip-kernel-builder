VERSION 0.6

FROM debian:bullseye
	
all:
	RUN apt-get update -y && \
	    apt-get install -y \
	    git lzop build-essential gcc bc libncurses5-dev libc6-i386 lib32stdc++6 zlib1g \
	    wget python3 python-is-python3 \
	    flex bison bc fakeroot devscripts \
	    kmod cpio libelf-dev libssl-dev \
	    rsync \
	    device-tree-compiler gcc-aarch64-linux-gnu
	
	RUN useradd -m -s /bin/bash build
	
	USER build
	
	RUN cd /home/build && \
	    git clone --depth 1 -b linux-6.1-stan-rkr1 https://github.com/armbian/linux-rockchip.git linux
	
	WORKDIR /home/build/linux
	
	ARG ARCH=arm64
	ARG CROSS_COMPILE=aarch64-linux-gnu-
	
	COPY "config" "/home/build/linux/.config"
	
	ARG EXTRAVERSION=-odroid-arm64
	RUN make -j4 bindeb-pkg

	RUN mkdir -p /tmp/output/ && \
	    cp -rf /home/build/linux-* /tmp/output/ && \
	    cd /tmp/output && \
	    tar -cvf /tmp/output.tar *
		
	SAVE ARTIFACT /tmp/output/* AS LOCAL output/

