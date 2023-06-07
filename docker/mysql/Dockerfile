FROM mysql/mysql-server:8.0

ENV TZ=Asia/Tokyo

COPY ./docker/mysql/my.cnf /etc/my.cnf
COPY ./docker/mysql/initdb.d /docker-entrypoint-initdb.d/

RUN chmod 644 /etc/my.cnf
