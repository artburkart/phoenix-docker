FROM artburkart/elixir:1.0.5
MAINTAINER Arthur Burkart <arturkart@gmail.com>

# Set up environment variables
ENV DEBIAN_FRONTEND noninteractive
ENV LANG C.UTF-8

ENV PHOENIX_VERSION 1.0.2
ENV NODE_VERSION 0.12.7
ENV NPM_VERSION 2.14.0

# Install inotify-tools for watching files
RUN apt-get update && apt-get install -y inotify-tools && rm -rf /var/lib/apt/lists/*

# Install Phoenix from source
RUN git clone --depth 1 --branch v$PHOENIX_VERSION https://github.com/phoenixframework/phoenix.git \
 && cd phoenix && mix local.hex --force && mix local.rebar --force \
 && mix do deps.get, compile

# Verify gpg and sha256: http://nodejs.org/dist/v0.12.7/SHASUMS256.txt.asc
# gpg keys listed at https://github.com/nodejs/node
RUN set -ex \
  && for key in \
    7937DFD2AB06298B2293C3187D33FF9D0246406D \
    114F43EE0176B71C7BC219DD50A3051F888C628D \
  ; do \
    gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \
  done

# Install Node.js and NPM in order to satisfy brunch.io dependencies
# the snippet below is borrowed from the official nodejs Dockerfile
# https://registry.hub.docker.com/_/node/
ADD http://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz /
ADD http://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc /
RUN gpg --verify SHASUMS256.txt.asc \
 && grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
 && tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
 && rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc \
 && npm install -g npm@"$NPM_VERSION" \
 && npm cache clear
