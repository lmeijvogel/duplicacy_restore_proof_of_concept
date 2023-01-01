# Restore procedures for backed up data

## Summary

Checks whether an InfluxDB backup can be restored.

We create a docker container with a running InfluxDB instance, and
restore a backup into that. We can then query it through the web interface to
see whether it works.

## Procedure

To restore, you basically need to run

```
$ # Clean any existing data, otherwise, setup mode in docker-compose.yml will not work
$ docker-compose down; rm -rf config/* data/* data-v/*
$ docker-compose up
```

This initializes InfluxDB with the configuration specified by the environment variables
in docker-compose.yml

Create a file `$HOME/.influxdb2/configs` and add an entry for `localhost` there:

```
[localhost]
  url = "http://localhost:8086"
  token = "the-admin-token"
  org = "home"
  active = true
```

With this configuration, you should be able to connect to the running instance.

To test, you can run `influx user list --active-config localhost` and you should get
back a list of users (only `the-admin`, if setup worked correctly).

To restore the buckets while retaining the test username/password data:
* Delete existing buckets -- Overwriting buckets is not supported by the influx command line tool:
  * Find buckets: `influx bucket ls --active-config localhost`
  * For each bucket:
      `influx bucket delete --name $BUCKET_NAME --org home --active-config localhost`
      ($ORG being the org name as configured in production. For me: 'home')
  * Restore the data
>     `influx restore --active-config localhost /data/user_data/influxdb_data`

To restore all the data, including usernames/passwords, tokens:
* Find the backup directory (in this example /data/user_data/influxdb_data. This directory contains a lot of timestamped .tar.gz files)
* Run `influx restore --full --active-config localhost /data/user_data/influxdb_data`

(the `--active-config` value references the section name in the `.influxdb2/configs` file.

This replaces *all* local configuration with production data, so it will also remove configured
user/password data, as well as the token. Replace with the production login data.

# Logging in

Go to http://localhost:8086 and log in with the credentials in the docker-compose.yml file, or,
if a full restore was done, with the production credentials (still on localhost).
