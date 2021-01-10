---
title: "Install Postgresql 1C - Ubuntu 18.04"
date: "2019-09-22"
---

```
#Install Postgresql
# Set Russian Locale
dpkg-reconfigure locales

#Install Prerequisites
wget http://archive.ubuntu.com/ubuntu/pool/main/i/icu/libicu55_55.1-7_amd64.deb
sudo dpkg -i libicu55_55.1-7_amd64.deb

sudo apt install postgresql-common

# cd postgresql-10.5-24.1C_amd64_deb
sudo dpkg -i libpq5*1C_amd64.deb
sudo dpkg -i postgresql-client*1C_amd64.deb
sudo dpkg -i postgresql-*1C_amd64.deb

# Установит блокировку обновления
sudo apt-mark hold libpq5 postgresql-client-10 postgresql-10

sudo service postgresql start
sudo update-rc.d postgresql enable
sudo service postgresql restart
```

## Преднастройка

### Настройка доступа

```
nano /etc/postgresql/10/main/pg_hba.conf
# make sure to have this: 'local all postgres trust'
sudo service postgresql restart

# Установка пароля
psql -U postgres -d template1 -c "ALTER USER postgres PASSWORD 'password'"

# Edit /etc/postgresql/10/main/postgresql.conf
listen_addresses = 'localhost'
```

```
#add cache
echo "tmpfs /var/lib/pgsql/10/data/pg_stat_tmp tmpfs size=1G,uid=postgres,gid=postgres 0 0" >> /etc/fstab
```

### Настройки базы данных под 1С

```
nano /etc/postgresql/10/main/postgresql.conf  
insert settings from here: https://infostart.ru/public/983170/
sudo service postgresql restart
```

```
effective_io_concurrency = 2
* shared_buffers = 1/8 RAM или больше (но не более 1/4);
* work_mem в 1/20 RAM;
* maintenance_work_mem в 1/4;
* fsync = true;
* wal_sync_method = fdatasync;
* commit_delay = от 10 до 100 ;
 commit_siblings = от 5 до 10;
* random_page_cost = 2 ;
* cpu_tuple_cost = 0.001 ;
* cpu_index_tuple_cost = 0.0005 ;
         * autovacuum = on

         * autovacuum_vacuum_threshold = 1800

* autovacuum_analyze_threshold = 900
```

### Ссылки

Установка PostgreSQL 1C: [https://adminguide.ru/2019/03/09/ubuntu-18-04-установка-postgresql-10-для-1с/](https://adminguide.ru/2019/03/09/ubuntu-18-04-установка-postgresql-10-для-1с/)

Описание настроек Постгре и 1С [https://infostart.ru/public/970225/](https://infostart.ru/public/970225/)

Файл настроек Постгре [https://infostart.ru/public/983170/](https://infostart.ru/public/983170/)

Бэкапы и перезапуски сервисов по расписанию [https://infostart.ru/public/1051601/](https://infostart.ru/public/1051601/)

Установка и настройка Постгре 1С [https://adminguide.ru/2019/03/09/ubuntu-18-04-установка-postgresql-10-для-1с/](https://adminguide.ru/2019/03/09/ubuntu-18-04-установка-postgresql-10-для-1с/)

Еще установка всего 1с + посере на убунту [https://kuharbogdan.com/stati-po-1s/ustanovka-postgresql-10-5-na-ubuntu-server-18-04-lts-dlya-1s-2/](https://kuharbogdan.com/stati-po-1s/ustanovka-postgresql-10-5-na-ubuntu-server-18-04-lts-dlya-1s-2/)

[https://www.kost.su/установка-сервера-postgresql-10-для-1с-предприяти/](https://www.kost.su/установка-сервера-postgresql-10-для-1с-предприяти/)
