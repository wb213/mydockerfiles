# docker-centos7-postgresql9.3
#
# VERSION 0.1

FROM centos:centos7
MAINTAINER Barry Wang barry213@gmail.com

# set environment var to UTF8 for later initdb usage
RUN localedef -i en_US -f UTF-8 en_US.UTF-8

ENV LANGUAGE en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# install pg repo
RUN rpm -i http://yum.postgresql.org/9.3/redhat/rhel-7-x86_64/pgdg-centos93-9.3-1.noarch.rpm

# install pgsql93
RUN yum update -y
RUN yum install -y postgresql93-server postgresql93-contrib

# set pgsql data default path
ENV PGDATA /var/lib/pgsql/data

#switch to user postgres
USER postgres

# initialize DB data files
RUN mkdir $PGDATA
#RUN chown postgres $PGDATA
#RUN su - postgres -c '/usr/pgsql-9.3/bin/initdb --locale=en_US.UTF-8 -D /var/lib/pgsql/data'
RUN ["/usr/pgsql-9.3/bin/initdb", "--locale=en_US.UTF-8"]

# set permissions to allow logins
#RUN echo "host    all             all             0.0.0.0/0            md5" >> /var/lib/pgsql/data/pg_hba.conf
RUN sed -i "s/^host *all *all *127.*/host    all             all             0.0.0.0\/0            md5/g" $PGDATA/pg_hba.conf

#listen on all interfaces
RUN echo "listen_addresses='*'" >> $PGDATA/postgresql.conf

# fire up db, create superuser, shut down
RUN /usr/pgsql-9.3/bin/pg_ctl -w start && \
    echo "ALTER USER postgres WITH PASSWORD 'postgres'" | psql && \
    /usr/pgsql-9.3/bin/pg_ctl -w stop

#expose port
EXPOSE 5432

#mount volume
VOLUME ["/var/lib/pgsql/data"]

# start the database
#CMD su - postgres -c 'PGDATA=/var/lib/pgsql/data /usr/pgsql-9.3/bin/postgres -D /var/lib/pgsql/data' 
CMD ["/usr/pgsql-9.3/bin/postgres"]
