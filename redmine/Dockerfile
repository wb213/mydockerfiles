# docker-centos7-postgresql9.3
#
# VERSION 0.1

FROM phusion/passenger-ruby20:0.9.11
MAINTAINER Barry Wang barry213@gmail.com

# Set correct environment variables.
ENV HOME /root

# Use baseimage-docker's init process.
CMD ["/sbin/my_init"]

RUN cd /home/app && \
    curl -OfSL http://www.redmine.org/releases/redmine-2.5.1.tar.gz && \
    tar xzvf redmine-2.5.1.tar.gz

ADD database.yml /home/app/redmine-2.5.1/config/database.yml

RUN cd /home/app/redmine-2.5.1 && \
    chown -R app:app /home/app/redmine-2.5.1 && \
    mkdir -p tmp tmp/pdf public/plugin_assets && \
    chown -R app:app files log tmp public/plugin_assets /home/app/redmine-2.5.1/config/database.yml && \
    chmod -R 755 files log tmp public/plugin_assets && \
    chmod 664 /home/app/redmine-2.5.1/config/database.yml

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    curl -fSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && \
    apt-get update && apt-get install -y postgresql-server-dev-9.3

USER app

WORKDIR /home/app/redmine-2.5.1 

RUN bundle install --path vendor/bundle --without development test

RUN bundle exec rake generate_secret_token
