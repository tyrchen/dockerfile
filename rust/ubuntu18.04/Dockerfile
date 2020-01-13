FROM ubuntu:18.04

LABEL maintainer="Tyr Chen <tyr.chen@gmail.com>"

# Set the locale
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y locales && \
	sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
	dpkg-reconfigure --frontend=noninteractive locales && \
	update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8

WORKDIR /tmp/rust-build

RUN apt-get update
RUN apt-get install -y \
	autoconf \
	apt-utils \
	build-essential \
	cmake \
	curl \
	git \
	libsqlite3-dev \
	libgmp-dev \
	liblz4-dev \
	liblzma-dev \
	libtool \
	libzstd-dev \
	ncurses-dev \
	wget \
	zstd \
	unzip

RUN git clone https://github.com/facebook/zstd --depth 1 --branch zstd-0.4.2 && \
	cd zstd && \
	make EXTRA_CFLAGS=-fPIC && make install

RUN curl -sL https://deb.nodesource.com/setup_11.x | bash - && \
	apt-get install -y nodejs

RUN echo "LC_ALL=en_US.UTF-8\nLANG=en_US.UTF-8" >> /etc/environment && export LC_ALL="en_US.UTF-8" && export LANG="en_US.UTF-8"

RUN apt-get auto-remove

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > /tmp/rust-build/rustup.sh && chmod +x /tmp/rust-build

RUN sh /tmp/rust-build/rustup.sh -y

RUN . $HOME/.cargo/env

RUN mkdir -p /opt/app

WORKDIR /opt/app

CMD ["/bin/sh"]
