FROM mysql:8.4

ENV TZ=Asia/Tokyo

# MySQL 設定ファイルをコピー
# 公式mysqlイメージは /etc/mysql/my.cnf が !includedir /etc/mysql/conf.d/ を読み込む構成のため
# /etc/my.cnf を直接上書きせず、conf.d配下に追加ファイルとして配置する
COPY ./docker/mysql/my.cnf /etc/mysql/conf.d/mysql.cnf
# データ初期化用 SQL ファイルらをコピー
COPY ./docker/mysql/initdb.d /docker-entrypoint-initdb.d/

RUN chmod 644 /etc/mysql/conf.d/mysql.cnf
