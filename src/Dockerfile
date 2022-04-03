FROM elixir:1.13.1

# Build Args
ARG PHOENIX_VERSION=1.6.6
ARG NODEJS_VERSION=16.x

# Local ENV
#ENV MIX_HOME=/.deps/.mix
ENV HEX_HOME=/.deps/.hex
ENV MIX_BUILD_ROOT=/.deps/_build
ENV MIX_DEPS_PATH=/.deps/mix_deps
ENV MIX_INSTALL_DIR=/.deps/mix_install
# Apt
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y apt-utils
RUN apt-get install -y build-essential
RUN apt-get install -y inotify-tools

# Nodejs
RUN curl -sL https://deb.nodesource.com/setup_${NODEJS_VERSION} | bash
RUN apt-get install -y nodejs
RUN groupadd -g 2000 appgroup && useradd -r -u 2001 -g 2000 -b /.deps app

# App Directory
ENV APP_HOME /app
RUN mkdir -p $APP_HOME && mkdir /.deps
WORKDIR $APP_HOME
RUN chown 2001:2000 /app && chown 2001:2000 /.deps

#Switch users
USER app

# Preinstall esbuild
RUN npm install esbuild

# Phoenix
RUN mix local.hex --force
RUN mix archive.install --force hex phx_new #{PHOENIX_VERSION}
RUN mix local.rebar --force



# App Port
EXPOSE 4000
COPY --chown=2001:2000 . /app
# get deps
RUN mix deps.get
#RUN mix assets.deploy


# Default Command
CMD ["mix", "phx.server"]

