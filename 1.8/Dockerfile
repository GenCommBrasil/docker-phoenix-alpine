FROM erlang:21-alpine

MAINTAINER Rakuten Brasil

# elixir expects utf8.
ENV ELIXIR_VERSION="v1.8.1" \
	LANG=C.UTF-8

RUN set -xe \
	&& ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/archive/${ELIXIR_VERSION}.tar.gz" \
	&& ELIXIR_DOWNLOAD_SHA256="de8c636ea999392496ccd9a204ccccbc8cb7f417d948fd12692cda2bd02d9822" \
	&& buildDeps=' \
		ca-certificates \
		curl \
		make \
	' \
	&& apk add --no-cache --virtual .build-deps $buildDeps \
	&& curl -fSL -o elixir-src.tar.gz $ELIXIR_DOWNLOAD_URL \
	&& echo "$ELIXIR_DOWNLOAD_SHA256  elixir-src.tar.gz" | sha256sum -c - \
	&& mkdir -p /usr/local/src/elixir \
	&& tar -xzC /usr/local/src/elixir --strip-components=1 -f elixir-src.tar.gz \
	&& rm elixir-src.tar.gz \
	&& cd /usr/local/src/elixir \
	&& make install clean \
	&& apk del .build-deps

# install the Phoenix Mix archive
RUN mix archive.install --force https://github.com/phoenixframework/archives/raw/master/phx_new.ez
RUN mix local.hex --force \
    && mix local.rebar --force

# install Node.js and NPM in order to satisfy brunch.io dependencies
RUN apk add --update nodejs nodejs-npm

# install git as a dependecy for fetching dependencies when using wercker
RUN apk add git

# Dependencies for the AppSignal for Elixir package version 1.7.0 and newer
RUN apk add --update alpine-sdk coreutils

WORKDIR /code
