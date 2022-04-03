FROM elixir:1.13.1

# Build Args
#ARG PHOENIX_VERSION=1.6.6
ARG PHOENIX_VERSION=
ARG NODEJS_VERSION=16.x

# Local ENV
#ENV MIX_HOME=/app_home/.mix
#ENV HEX_HOME=/app_home/.hex
ENV MIX_BUILD_ROOT=/app_home/_build
ENV MIX_DEPS_PATH=/app_home/mix_deps
ENV MIX_INSTALL_DIR=/app_home/mix_install
# Apt
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y apt-utils
RUN apt-get install -y build-essential
RUN apt-get install -y inotify-tools

# Nodejs
RUN curl -sL https://deb.nodesource.com/setup_${NODEJS_VERSION} | bash
RUN apt-get install -y nodejs

# App Directory
ENV APP_HOME /app
RUN mkdir -p $APP_HOME && mkdir -p /app_home chown 2001:2000 /app && chown 2001:2000 /app_home
WORKDIR $APP_HOME

# Create User
RUN groupadd -g 2000 appgroup && useradd -r -u 2001 -g 2000 -b /app_home app

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

