FROM centos:7

LABEL maintainer="Tyr Chen <tyr.chen@gmail.com>"

ENV LC_ALL=en_US.utf8

ENV ELIXIR_VERSION=v1.8.1

WORKDIR /tmp/elixir-build

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
	libffi \
	libffi-devel \
	libsqlite3x-devel \
	libzstd-devel \
	libtool \
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

RUN /usr/bin/curl --silent --location https://rpm.nodesource.com/setup_11.x | bash && \
	yum install nodejs -q -y

RUN yum localinstall -q -y https://packages.erlang-solutions.com/erlang-solutions-1.0-1.noarch.rpm && \
	yum install esl-erlang -q -y

RUN git clone https://github.com/elixir-lang/elixir --depth 1 --branch $ELIXIR_VERSION && \
	cd elixir && \
	make && make install && \
	mix local.hex --force && \
	mix local.rebar --force && \
	cd $HOME && \
	rm -rf /tmp/elixir-build

# RUN curl -O -L https://www.openssl.org/source/openssl-1.1.1.tar.gz && \
# 	tar xvf openssl-1.1.1.tar.gz && cd openssl-1.1.1 && \
# 	./config -Wl,--enable-new-dtags,-rpath,'$(LIBRPATH)' && make && \
# 	make install && cd .. && rm -rf openssl-1.1.1

RUN yum clean all

RUN mkdir -p /opt/app

WORKDIR /opt/app

CMD ["/bin/sh"]
