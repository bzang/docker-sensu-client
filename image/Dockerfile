# Use phusion/baseimage as base image. To make your builds
# reproducible, make sure you lock down to a specific version, not
# to `latest`! See
# https://github.com/phusion/baseimage-docker/blob/master/Changelog.md
# for a list of version numbers.
FROM phusion/baseimage:0.9.18
MAINTAINER Bernie Zang <bernie.zang@gmail.com>

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Install sensu, filebeat, and other pre-requisites
RUN curl http://repos.sensuapp.org/apt/pubkey.gpg | sudo apt-key add -
RUN echo "deb     http://repos.sensuapp.org/apt sensu main" | sudo tee -a /etc/apt/sources.list.d/sensu.list
RUN apt-get update && apt-get upgrade -y -o Dpkg::Options::="--force-confold" && apt-get install -y sensu build-essential

# Allow sensu to run docker commands
RUN usermod -a -G sudo,users sensu
ENV PATH "/opt/sensu/embedded/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

RUN gem install bundler

COPY etc/default/sensu /etc/default/sensu
COPY etc/sensu/conf.d/client.json /etc/sensu/conf.d/client.json

ENV APP_HOME /app
ADD Gemfile* $APP_HOME/
ENV BUNDLE_GEMFILE=$APP_HOME/Gemfile BUNDLE_JOBS=2

RUN bundle install

RUN mkdir /etc/service/sensu-client
ADD etc/service/sensu-client/run /etc/service/sensu-client/run

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
