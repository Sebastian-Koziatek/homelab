FROM ubuntu:latest

# Ustawienie zmiennych środowiskowych
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

# Instalacja wymaganych pakietów
RUN apt update && apt install -y \
    wget \
    lsb-release \
    gnupg2 \
    mariadb-server \
    mariadb-client \
    curl \
    locales

# Generowanie lokalizacji
RUN locale-gen en_US.UTF-8 && update-locale

# Pobranie i instalacja repozytorium Zabbixa 7.2 dla Ubuntu 24.04
RUN wget https://repo.zabbix.com/zabbix/7.2/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.2+ubuntu24.04_all.deb && \
    dpkg -i zabbix-release_latest_7.2+ubuntu24.04_all.deb && \
    apt update

# Instalacja serwera Zabbix, interfejsu webowego i agenta
RUN apt install -y \
    zabbix-server-mysql \
    zabbix-frontend-php \
    zabbix-apache-conf \
    zabbix-sql-scripts \
    zabbix-agent

# Tworzenie katalogów dla MySQL
RUN mkdir -p /run/mysqld && chown -R mysql:mysql /run/mysqld

# Kopiowanie konfiguracji Zabbixa
COPY zabbix_server.conf /etc/zabbix/zabbix_server.conf
COPY zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf

# Eksponowanie portów
EXPOSE 10051 10050 80 443

# **Start MySQL i inicjalizacja bazy przy starcie kontenera**
CMD set -e && \
    mysqld_safe --skip-networking & \
    sleep 5 && \
    mysql -uroot -e "CREATE DATABASE IF NOT EXISTS zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;" && \
    mysql -uroot -e "CREATE USER IF NOT EXISTS 'zabbix'@'localhost' IDENTIFIED BY 'password';" && \
    mysql -uroot -e "GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';" && \
    mysql -uroot -e "SET GLOBAL log_bin_trust_function_creators = 1;" && \
    zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -ppassword zabbix && \
    mysql -uroot -e "SET GLOBAL log_bin_trust_function_creators = 0;" && \
    mysql -uzabbix -ppassword -e "SELECT * FROM zabbix.dbversion;" && \
    service apache2 start && \
    service zabbix-server start && \
    service zabbix-agent start && \
    tail -f /var/log/zabbix/zabbix_server.log
