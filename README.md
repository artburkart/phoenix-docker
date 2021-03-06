## A Docker container for the Phoenix framework

It features all the latest versions of the Phoenix web framework, the Elixir language and the Erlang platform.

![Phoenix Logo](https://www.filepicker.io/api/file/9prSmznZTiaRRmI3t89E)

Phoenix is a framework for building scalable web applications with realtime connectivity across all your devices. It relies on the Elixir language for making the development of maintainable applications productive and fun.

## Create a Phoenix app

```console
$ docker run -it --rm -v "$PWD":/code -w /phoenix artburkart/phoenix mix phoenix.new /code/my_new_app
```

## Run Phoenix app
```console
$ docker run -it --rm --expose 4000 -P -v "$PWD":/code -w /code/my_new_app artburkart/phoenix bash -c "mix deps.get && mix phoenix.server"
```

# Image Variants

## `artburkart/phoenix:<version>`

This is the defacto image. If you are unsure about what your needs are, you probably want to use this one. It is designed to be used both as a throw away container (mount your source code and start the container to start your app), as well as the base to build other images off of.

## `artburkart/phoenix:<version>-onbuild`

This image makes building derivative images easier. For most use cases, creating a `Dockerfile` in the base of your project directory with the line `FROM artburkart/phoenix:onbuild` will be enough to create a stand-alone image for your project.

While the `onbuild` variant is really useful for "getting off the ground running" (zero to Dockerized in a short period of time), it's not recommended for long-term usage within a project due to the lack of control over *when* the `ONBUILD` triggers fire.

Once you've got a handle on how your project functions within Docker, you'll probably want to adjust your `Dockerfile` to inherit from a non-`onbuild` variant and copy the commands from the `onbuild` variant `Dockerfile` (moving the `ONBUILD` lines to the end and removing the `ONBUILD` keywords) into your own file so that you have tighter control over them and more transparency for yourself and others looking at your `Dockerfile` as to what it does. This also makes it easier to add additional requirements as time goes on (such as installing more packages before performing the previously-`ONBUILD` steps).

# Supported Docker versions

I built this image with docker 1.8.2

This Dockerfile borrows a ton from [marcelocg](https://hub.docker.com/r/marcelocg/phoenix/)'s image and from the [official](https://hub.docker.com/explore/) repos (there is no official image for elixir or phoenix)
