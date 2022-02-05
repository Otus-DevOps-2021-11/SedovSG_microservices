FROM ubuntu:18.04

LABEL version="1.0.0" \
      description="Базовый образ Ubuntu 18.04 Bionic и Ruby" \
      maintainer="SedovSG@yandex.ru"

ENV BUILD_DEPS \
	mongodb-server \
	ruby-full \
	ruby-dev \
	build-essential \
	git

RUN apt-get update && \
	apt-get install -y --no-install-recommends ${BUILD_DEPS}; \
	gem update --system 3.2.3 && gem install bundler \
	&& git clone -b monolith https://github.com/express42/reddit.git \
	&& sed -i "s/'mongo'$/'mongo', '~> 2.0.0'/" reddit/Gemfile; \
	apt-get autoremove -y && apt-get autoclean && apt-get clean \
    && rm -rf /var/lib/apt/lists/* && rm -rf /tmp/* /var/tmp/*

COPY mongod.conf /etc/mongod.conf
COPY db_config /reddit/db_config
COPY start.sh /usr/local/bin

RUN cd /reddit && rm Gemfile.lock && bundle install; \
	chmod +x /usr/local/bin/start.sh

CMD ["start.sh"]