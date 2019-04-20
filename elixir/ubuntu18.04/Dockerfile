FROM ubuntu:18.04

LABEL maintainer="Tyr Chen <tyr.chen@gmail.com>"

# Set the locale
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y locales && \
	sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
	dpkg-reconfigure --frontend=noninteractive locales && \
	update-locale LANG=en_US.UTF-8

ENV LANG en_US.UTF-8

ENV ELIXIR_VERSION=v1.8.1

WORKDIR /tmp/elixir-build

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

RUN wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb --no-check-certificate && \
	dpkg -i erlang-solutions_1.0_all.deb && \
	wget https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc --no-check-certificate && \
	apt-key add erlang_solutions.asc && \
	apt-get update && \
	apt-get install -y esl-erlang

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
# 	make install

RUN apt-get auto-remove

RUN mkdir -p /opt/app

WORKDIR /opt/app

CMD ["/bin/sh"]
