# PostgreSQL Database

As I use PostgreSQL databases a lot I will collect some operation tasks around them.

## Root access

As defined in the default configuration of PostgreSQL on Linux, a user called `postgres` is made and only this user may connect without password and has super admin rights to the entire PostgreSQL instance.

So to change the configuration or administrate it you have to be `postgres` user:

```bash
sudo -u postgres psql
```

For security reasons `sudo -u` may not generally be allowed but a setting like:

```bash
# setup once as root
echo "su -s /bin/bash -l postgres" >> /usr/local/sbin/go-user-postgres
chmod 755 /usr/local/sbin/go-user-postgres
echo "<user> ALL=NOPASSWD: /usr/local/sbin/go-user-postgres" >> /etc/sudoers.d/divibib
```

This allows a specific user to switch to `postgres` user but not to other users:

```bash
sudo /usr/local/sbin/go-user-postgres
```

And to make it easier for you to call it, add it in your users path:

```bash
echo "export PATH=$PATH:/usr/local/sbin" >> /home/divibib/.bashrc
source /home/divibib/.bashrc
# now you only need to call
sudo go-user-postgres
```

## Configuration

To generally calculate how to configure [PGTune](https://pgtune.leopard.in.ua/#/) can be used. But this is only a start point you should always check that the values suggested here are working on the specific scenario.

## Setup Database

1.  Creating user

    ```bash
    sudo -u postgres createuser <username>
    ```

2.  Creating Database

    ```bash
    sudo -u postgres createdb <dbname>
    ```

3.  Giving the user a password

    ```bash
    sudo -u postgres psql -c "alter user <username> with encrypted password '<password>';"
    ```

4.  Granting privileges on database

    ```bash
    sudo -u postgres psql -c "grant all privileges on database <dbname> to <username>;"
    ```

And yeah, that should be it!

Or do it completely as SQL:

```sql
CREATE DATABASE <your db name>;
CREATE USER <your user> WITH ENCRYPTED PASSWORD '<password>';
GRANT ALL PRIVILEGES ON DATABASE <your db name> TO <your user>;
```

## Maintenance

A quick overview of what is going on can be seen using `pg_top` which is like the system top, but for the database. Install it in debian using: `apt-get install pgtop`.

### Databases and Schemas

Get a list of all databases using `\l` in psql or the following SQL:

```sql
SELECT datname AS db, datconnlimit AS connlimit FROM pg_database WHERE datistemplate = false;
```

The schemas can be retrieved for each database with size (connect to the database first):

```sql
SELECT schema_name,
       pg_size_pretty(sum(table_size)::bigint) AS size,
       round((sum(table_size) / pg_database_size(current_database())) * 100, 2) AS percent
FROM (
  SELECT pg_catalog.pg_namespace.nspname as schema_name,
         pg_relation_size(pg_catalog.pg_class.oid) as table_size
  FROM   pg_catalog.pg_class
     JOIN pg_catalog.pg_namespace ON relnamespace = pg_catalog.pg_namespace.oid
) t
GROUP BY schema_name
ORDER BY percent DESC;
```

And the extensions installed in each database are shown using `\dx` or:

```sql
SELECT * FROM pg_extension;
```

### Connections

To get the current connections for each database, use:

```sql
select datname, numbackends from pg_stat_database;
```

This will show a table like:

```text
  datname   | numbackends
------------+-------------
 template0  |           0
 system1    |          10
 template1  |           0
```

To get some more information over the whole database cluster the connections used on the whole database server use:

```sql
select max_conn,used,res_for_super,max_conn-used-res_for_super res_for_normal
from
  (select count(*) used from pg_stat_activity) t1,
  (select setting::int res_for_super from pg_settings where name=$$superuser_reserved_connections$$) t2,
  (select setting::int max_conn from pg_settings where name=$$max_connections$$) t3;
```

This will give you something like:

```text
max_conn | used | res_for_super | res_for_normal
---------+------+---------------+----------------
  100    |    2 |             3 |             95
(1 row)
```

The user limit can be displayed using:

```sql
SELECT rolname, (SELECT count(*) FROM pg_stat_activity WHERE usename=rolname) AS used, rolconnlimit FROM pg_roles WHERE rolconnlimit <> -1;
```

```text
   rolname    | used | rolconnlimit
--------------+------|--------------
 my_user      |   12 |           30
(1 row)
```

And it can be changed using `ALTER USER my_user CONNECTION LIMIT 50;`

### Processes

A short info what is running can be seen in `htop` as there is a process per connection running which also shows the database and type of SQL. More information what is running can be displayed using:

    SELECT * FROM pg_stat_activity WHERE state = 'active';

To stop one of this statements you can call:

    SELECT pg_cancel_backend(<pid of the process>)

If the process cannot be killed, try:

    SELECT pg_terminate_backend(<pid of the process>)

### Blocking

Using the following SQL you will get a concrete List of who is blocking who:

```sql
-- run using postgres user
SELECT blocked_locks.pid AS blocked_pid,
       blocked_activity.usename AS blocked_user,
       blocking_locks.pid AS blocking_pid,
       blocking_activity.usename AS blocking_user,
       blocked_activity.query AS blocked_statement,
       blocking_activity.query AS current_statement_in_blocking_process
   FROM  pg_catalog.pg_locks AS blocked_locks
    JOIN pg_catalog.pg_stat_activity AS blocked_activity ON blocked_activity.pid = blocked_locks.pid
    JOIN pg_catalog.pg_locks AS blocking_locks
        ON blocking_locks.locktype = blocked_locks.locktype
        AND blocking_locks.DATABASE IS NOT DISTINCT FROM blocked_locks.DATABASE
        AND blocking_locks.relation IS NOT DISTINCT FROM blocked_locks.relation
        AND blocking_locks.page IS NOT DISTINCT FROM blocked_locks.page
        AND blocking_locks.tuple IS NOT DISTINCT FROM blocked_locks.tuple
        AND blocking_locks.virtualxid IS NOT DISTINCT FROM blocked_locks.virtualxid
        AND blocking_locks.transactionid IS NOT DISTINCT FROM blocked_locks.transactionid
        AND blocking_locks.classid IS NOT DISTINCT FROM blocked_locks.classid
        AND blocking_locks.objid IS NOT DISTINCT FROM blocked_locks.objid
        AND blocking_locks.objsubid IS NOT DISTINCT FROM blocked_locks.objsubid
        AND blocking_locks.pid != blocked_locks.pid
    JOIN pg_catalog.pg_stat_activity AS blocking_activity ON blocking_activity.pid = blocking_locks.pid
   WHERE NOT blocked_locks.GRANTED;
```

### Deadlock

Die folgende Abfrage zeigt Prozesse an, die sich blockieren.

```sql
SELECT DISTINCT blocked_locks.pid     AS blocked_pid,
         blocked_activity.usename  AS blocked_user,
         blocking_locks.pid     AS blocking_pid,
         blocking_activity.usename AS blocking_user,
         substring(blocked_activity.query, 0, 40)    AS blocked_statement,
         substring(blocking_activity.query, 0, 40)   AS current_statement_in_blocking_process
   FROM  pg_catalog.pg_locks         blocked_locks
    JOIN pg_catalog.pg_stat_activity blocked_activity  ON blocked_activity.pid = blocked_locks.pid
    JOIN pg_catalog.pg_locks         blocking_locks
        ON blocking_locks.locktype = blocked_locks.locktype
        AND blocking_locks.DATABASE IS NOT DISTINCT FROM blocked_locks.DATABASE
        AND blocking_locks.relation IS NOT DISTINCT FROM blocked_locks.relation
        AND blocking_locks.page IS NOT DISTINCT FROM blocked_locks.page
        AND blocking_locks.tuple IS NOT DISTINCT FROM blocked_locks.tuple
        AND blocking_locks.virtualxid IS NOT DISTINCT FROM blocked_locks.virtualxid
        AND blocking_locks.transactionid IS NOT DISTINCT FROM blocked_locks.transactionid
        AND blocking_locks.classid IS NOT DISTINCT FROM blocked_locks.classid
        AND blocking_locks.objid IS NOT DISTINCT FROM blocked_locks.objid
        AND blocking_locks.objsubid IS NOT DISTINCT FROM blocked_locks.objsubid
        AND blocking_locks.pid != blocked_locks.pid

    JOIN pg_catalog.pg_stat_activity blocking_activity ON blocking_activity.pid = blocking_locks.pid
   WHERE NOT blocked_locks.GRANTED;
```

### Backup/Restore

To backup and restore or to move a database to another server (maybe with newer postgres version):

```bash
sudo -u postgres pg_dump --create mydb > mydb.dump
sudo -u postgres pg_dumpall --globals-only > globals.dump
```

And to later restore it check the following:

-   create the tablespace directories if not there
-   add locales to your system if missing (`locale -a`, `vi /etc/locale.gen`, `locale-gen`) and restart postgres

Then you may import the data:

```bash
sudo -u postgres psql < globals.dump
sudo -u postgres psql < mydb.dump
```

### Upgrade Postgres

After you have installed multiple versions of postgres on your server you can see them like:

    $ pg_lsclusters
    Ver Cluster Port Status Owner    Data directory               Log file
    9.4 main    5432 online postgres /var/lib/postgresql/9.4/main /var/log/postgresql/postgresql-9.4-main.log
    9.6 main    5433 online postgres /var/lib/postgresql/9.6/main /var/log/postgresql/postgresql-9.6-main.log

To switch from the used 9.4 to the currently unused 9.6 do the following:

    pg_dropcluster 9.6 main --stop
    pg_upgradecluster 9.4 main

This may take some time in which a new 9.6 database on port 5433 will be created and updated. After done it should look like:

    $ pg_lsclusters
    Ver Cluster Port Status Owner    Data directory               Log file
    9.4 main    5433 down   postgres /var/lib/postgresql/9.4/main /var/log/postgresql/postgresql-9.4-main.log
    9.6 main    5432 online postgres /var/lib/postgresql/9.6/main /var/log/postgresql/postgresql-9.6-main.log

Now the old database server can be dropped:

    pg_dropcluster 9.4 main
    apt-get --purge remove postgresql-client-9.4 postgresql-9.4

### Allow access (pg_hba.conf)

To allow somebody to access the server a matching entry in the file `/etc/postgresql/11/main/pg_hba.conf` is needed:

```text
# TYPE  DATABASE        USER            ADDRESS                 METHOD
# "local" is for Unix domain socket connections only
local   all             all                                     peer
# local connections with credentials
host    all             all             127.0.0.1/32            md5
host    all             all             ::1/128                 md5
# Allow replication connections from localhost, by a user with the
# replication privilege.
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            md5
host    replication     all             ::1/128                 md5
# Users and systems which are allowed to use the database:
host    all             all             192.168.1.0/24          md5
```

Your file should look something like the above and you can change or add the entries. After something was changed, you have to reload the configuration in the server to take effect:

From the command line as postgres user:

```bash
/usr/bin/pg_ctl reload
```

Or using SQL:

```sql
SELECT pg_reload_conf();
```

## Rights management

To show which rights are set use:

```sql
SELECT grantee, privilege_type
FROM information_schema.role_table_grants
WHERE table_name='table'
```

Here are some common statement to grant access to a PostgreSQL user or group:

1. Grant CONNECT to the database:

    ```sql
    GRANT CONNECT ON DATABASE database_name TO username;
    ```

2. Grant USAGE on schema:

    ```sql
    GRANT USAGE ON SCHEMA schema_name TO username;
    ```

3. Grant on all tables for DML statements: SELECT, INSERT, UPDATE, DELETE:

    ```sql
    GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA schema_name TO username;
    ```

4. Grant all privileges on all tables in the schema:

    ```sql
    GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA schema_name TO username;
    ```

5. Grant all privileges on all sequences in the schema:

    ```sql
    GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA schema_name TO username;
    ```

6. Grant all privileges on the database:

    ```sql
    GRANT ALL PRIVILEGES ON DATABASE database_name TO username;
    ```

7. Grant permission to create database:

    ```sql
    ALTER USER username CREATEDB;
    ```

8. Make a user superuser:

    ```sql
    ALTER USER myuser WITH SUPERUSER;
    ```

9. Remove superuser status:

    ```sql
    ALTER USER username WITH NOSUPERUSER;
    ```

Those statements above only affect the current existing tables. To apply to newly created tables, you need to use alter default. For example:

```sql
ALTER DEFAULT PRIVILEGES
FOR USER username
IN SCHEMA schema_name
GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO username;
```

{!docs/abbreviations.txt!}
