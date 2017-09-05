FROM ubuntu:16.04

RUN mkdir  -p /usr/local/etc \
  && { \
    echo 'install: --no-document'; \
    echo 'update: --no-document'; \
  } >> /usr/local/etc/gemrc

ENV RUBY_MAJOR 1.9
ENV RUBY_VERSION 1.9.3-p551
ENV REALLY_GEM_UPDATE_SYSTEM true
ENV RUBYGEMS_VERSION 2.6.13

RUN set -ex \
  && apt-get update \
  && buildDeps=' \
    software-properties-common \
  ' \
  && apt-get install -y --no-install-recommends $buildDeps \
  && rm -rf /var/lib/apt/lists/* \
  && apt-add-repository ppa:brightbox/ruby-ng \
  && apt-get update \
  && packages=' \
    g++ \
    gcc \
    make \
    ruby1.9.1-dev \
    ruby1.9.3 \
    ruby-switch \
  ' \
  && apt-get install -y --no-install-recommends $packages \
  && apt-get purge -y --auto-remove $buildDeps \
  && ruby-switch --set ruby1.9.1 \
  && gem update --system "$RUBYGEMS_VERSION"

ENV BUNDLER_VERSION 1.15.4
RUN gem install bundler --version "$BUNDLER_VERSION"
ENV GEM_HOME /usr/local/bundle

ENV BUNDLE_PATH="$GEM_HOME" \
  BUNDLE_BIN="$GEM_HOME/bin" \
  BUNDLE_SILENCE_ROOT_WARNING=1 \
  BUNDLE_APP_CONFIG="$GEM_HOME"

ENV PATH $BUNDLE_BIN:$PATH

RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \
  && chmod 777 "$GEM_HOME" "$BUNDLE_BIN"

CMD [ "irb" ]
