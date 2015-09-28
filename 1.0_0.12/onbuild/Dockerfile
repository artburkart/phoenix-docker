FROM artburkart/phoenix:1.0_0.12
MAINTAINER Arthur Burkart <artburkart@gmail.com>

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

ONBUILD COPY mix.exs /usr/src/app/
ONBUILD RUN mix deps.get

ONBUILD COPY . /usr/src/app
