# psql


local postgres logs (macOS)
```
/opt/homebrew/var/log/postgres.log
```

Connect
```shell script
psql -U replicator -d postgres://192.168.56.40:5435/ab

psql -U postres -d abdev
```

List databases
```shell script
psql -U postgres -c "\l"
```

Connect to database
```shell script
\c abdev 
```

List tables
```shell script
\dt
```

Show current user
```sql
SELECT current_user;
```

Table schema
```shell script
\d+ mytable
```

Procedure code
```shell
\df+ insert_log
```

Format rows as columns
```shell script
\x
```

Help example
```shell script
\h CREATE USER
```

Open the last query in vim, save and it execs
```shell script
\e
```

Horizontal scrolling `-S`
```shell script
PAGER="less -S" psql -c "TABLE pg_settings"
```

Show `postgres.conf`
```shell script
psql -c "TABLE pg_file_settings"
```




View privileges
```sql
SELECT *
FROM information_schema.table_privileges
WHERE table_name = 'wlogs';
```

## Upgrading Postgres
WIP

-option 1: for using `pg_upgrade` both pg12 and pg13 have to have binaries
installed. Maybe using nested jails? for installing the new version? NO
Maybe cloning the jail before installing pg13 and having two jails?

- option 2: `pg_dump`



# WIP Postgres Failover
WAL Stream, Async, one standby node (no read-only replicas).

## Promoting a Standby
Make sure not to let the PrimaryDB restart, otherwise we would have split brain.
In the standby:
```shell script
su - postgres
/usr/local/bin/pg_ctl promote -D /var/db/postgres/data12/
```

Test
```shell script
psql -U postgres -c "select pg_is_in_recovery();"
```

## $PGDATA/standby.signal
That file is what tells pg to start as a standby.


# Learning Notes
Webminar by Martin Marques (2nd Quadrant) Achieving High Availability with PostgreSQL
- Synchronous replication needs at least two stand-by
  nodes, because if there's no stand-by the PrimaryDB won't commit.
- **Hot** standbys are not a good option for read-only queries.
    1. that load reduces the chance of keeping up with the PrimaryDB.
    2. may lock the PrimaryDB ~30 seconds if they're conflicts (like reading from a table
       that's being altered).
- Health-checks need to consider that false-positives are possible.
    - Connection/link failure vs. actual PrimaryDB failure.
- Primary + 2 Standbys
    - e.g. use one standby as hot, and the other for reporting loads.
- A standby can be set to apply the stream delayed (e.g. 6hours), so it has the stream but un-applied. Why this feature?
- Backups can be taken from the standby or the PrimaryDB.
- The Split-Brain issue means more than one PrimaryDB.
- When there multiple standbys identify the best candidate (further ahead) for promotion
- Failover tool `databases/postgresql-repmgr`
    - Manual or auto failover
    - daemon to monitor the primary
    - detects failed standbys
    - the testing for failed primary can be configured
    - won't promote if there's no quorum (e.g. the standby can't connect to other standbys)
    - reconfigures any remaining standbys to follow the new primary
    - `pg_ping` multiple times
    - `select 1`
- Reconfiguring apps:
    - `pgbouncer` on each of the app servers, otherwise `pgbouncer` is a single-point of failure.
        - pausable during configuration changes.
        - when there're too many app servers, pgbouncer becomes a bit of a nuisance.
    - Virtual IP, `repmgr` has to be in charge of "up"ing it (e.g. on reboot the server should not do it to avoid network collisions)
    - other fencing tools, chronosync, pacemaker, networking, etc.


# pg docs notes

https://www.postgresql.org/docs/current/creating-cluster.html

However, while the directory contents are secure, the default client authentication setup
allows any local user to connect to the database and even become the database superuser.
If you do not trust other local users, we recommend you use one of initdb's -W, --pwprompt
or --pwfile options to assign a password to the database superuser. Also, specify -A
md5 or -A password so that the default trust authentication mode is not used; or modify
the generated pg_hba.conf file after running initdb, but before you start the server
for the first time. (Other reasonable approaches include using peer authentication or
file system permissions to restrict connections. See Chapter 20 for more information.)

### FreeBSD tuning
https://www.postgresql.org/docs/current/kernel-resources.html

These are the FreeBSD default values:
```shell script
kern.ipc.shmall=131072    # Maximum number of pages available for shared memory
kern.ipc.shmmax=536870912 # Maximum shared memory segment size
kern.ipc.semmni=50        # Number of semaphore identifiers
kern.ipc.semmns=340       # Maximum number of semaphores in the system

# When 1, prevents SysV shared memory from being paged out to swap.
kern.ipc.shm_use_phys=0   # Enable/Disable locking of shared memory pages in core
```

The PostgreSQL server uses one process per connection, so you should provide
for at least as many processes as allowed connections, in addition to
what you need for the rest of your system. This is usually not a problem
but if you run several servers on one machine things might get tight.

https://www.postgresql.org/docs/current/preventing-server-spoofing.html
To prevent spoofing with SSL, the server must be configured to accept only hostssl
connections (Section 20.1) and have SSL key and certificate files (Section
18.9). The TCP client must connect using sslmode=verify-ca or verify-full
and have the appropriate root certificate file installed (Section 33.18.1).
