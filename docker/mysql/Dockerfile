FROM mysql/mysql-server:8.0

ENV MYSQL_DATABASE=testdb \
    MYSQL_USER=test \
    MYSQL_PASSWORD=secret \
    MYSQL_ROOT_PASSWORD=secret \
    TZ=Asia/Tokyo

COPY ./docker/mysql/my.cnf /etc/my.cnf

RUN chmod 644 /etc/my.cnf
