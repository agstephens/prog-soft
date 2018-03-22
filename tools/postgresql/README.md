# Installing and setting up Postgresql

## Installation

As root:

```
yum install -y postgresql-server postgresql postgresql-devel
```

## Initialise DB

As root:

```
service postgresql initdb
service postgresql start
```

## Create a db and a role 

```
# postgres automatically creates the local postgres user, use that to create db
sudo -u postgres createdb mytest
sudo -u postgres createuser --no-createrole --no-superuser --no-createdb --pwprompt mytest
```

## Check we can login

```
psql -U mytest
```

## Script to load a db from an SQL dump

```
DB_HOST=localhost
DB_USER=mytest
DB_NAME=mytest

db_dump=mytest_dump.sql

echo "Dropping all content from the db."
psql -At  \
    -c "select 'drop table \"' || tablename || '\" cascade;' from pg_tables where schemaname='public';" \
    -U $DB_USER $DB_NAME > drop_all.sql

echo "Running: drop_all.sql"
psql -U $DB_USER -f drop_all.sql $DB_NAME

echo "Loading content into db from: $db_dump"
psql -d $DB_NAME -U $DB_USER -f $db_dump
```

## Managing access by user/group/IP

As root, edit this file.

```
/var/lib/pgsql/data/pg_hba.conf
```

And if you need users to login from remote servers then update:

```
/etc/sysconfig/iptables
```

With lines like:

```
# Postgresql access
-A INPUT -p tcp --dport 5432 -s 172.16.150.178/32 -j ACCEPT
```

And restart:

```
service iptables restart
```
