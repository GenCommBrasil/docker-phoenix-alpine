FROM erlang:21-alpine

MAINTAINER Rakuten Brasil

# elixir expects utf8.
ENV ELIXIR_VERSION="v1.9" \
	LANG=C.UTF-8

RUN set -xe \
	&& ELIXIR_DOWNLOAD_URL="https://github.com/elixir-lang/elixir/archive/${ELIXIR_VERSION}.tar.gz" \
	&& ELIXIR_DOWNLOAD_SHA256="10363ba253eaac253725a9101c082422586872cbc2b456b14e50e69466c0dbff" \
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

# install git as a dependency for fetching dependencies when using wercker
RUN apk add git


# install pip as a dependency in order to be able to install aws cli and deploy when using wercker
RUN apk add py-pip
RUN pip install awscli


# Dependencies for RKPCoreEx:
RUN apk add postgresql

# Dependencies for the AppSignal for Elixir package version 1.7.0 and newer
RUN apk add --update alpine-sdk coreutils

WORKDIR /code
