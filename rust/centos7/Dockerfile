FROM centos:7

LABEL maintainer="Tyr Chen <tyr.chen@gmail.com>"

ENV LC_ALL=en_US.utf8

WORKDIR /tmp/rust-build

RUN yum -y update && yum clean all

RUN yum install -y epel-release

RUN yum groups mark convert

RUN yum groupinstall -y 'Development Tools'

RUN yum install -y clang clang-devel llvm-devel cmake3 cmake python-testtools python-pip wget

RUN yum install -q -y \
	autoconf \
	automake \
	cmake3 \
	epel-release \
	git \
	gcc \
	gcc-c++ \
	gmp-devel \
	gmp-static \
	libffi \
	libffi-devel \
	libsqlite3x-devel \
	libzstd-devel \
	libtool \
	lzma \
	lz4-devel \
	m4 \
	make \
	ncurses-devel \
	openssl \
	openssl-devel \
	perl-core \
	rpm-build \
	tar \
	wget \
	zlib-devel \
	zstd

RUN git clone https://github.com/facebook/zstd --depth 1 --branch zstd-0.4.2 && \
	cd zstd && \
	make EXTRA_CFLAGS=-fPIC && make install

RUN /usr/bin/curl --silent --location https://rpm.nodesource.com/setup_11.x | bash && \
	yum install nodejs -q -y

RUN yum clean all

RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > /tmp/rust-build/rustup.sh && chmod +x /tmp/rust-build

RUN sh /tmp/rust-build/rustup.sh -y

RUN cp $HOME/.cargo/bin/* /usr/local/bin

RUN yum install -q -y llvm-devel clang-devel snappy-devel gflags-devel

ADD config $HOME/.cargo/

# RUN git clone https://github.com/rust-rocksdb/rust-rocksdb --depth 1 --branch v0.13.0 && \
#   cd rust-rocksdb && git submodule update --init --recursive && cargo build && cargo build --release

# RUN git clone https://github.com/serde-rs/serde --depth 1 --branch v1.0.104 && \
#   cd serde && cargo build && cargo build --release

RUN mkdir -p /opt/app && cd /opt/app && rm -rf /tmp/rust-build

RUN mkdir -p /opt/app

WORKDIR /opt/app

CMD ["/bin/sh"]
