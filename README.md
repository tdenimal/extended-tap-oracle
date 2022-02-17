# extended-tap-oracle

[![License: MIT](https://img.shields.io/badge/License-GPLv3-yellow.svg)](https://opensource.org/licenses/GPL-3.0)

[Singer](https://www.singer.io/) tap that extracts data from a [Oracle](https://www.oracle.com/database/) database and produces JSON-formatted data following the [Singer spec](https://github.com/singer-io/getting-started/blob/master/docs/SPEC.md).

## How to use it

Run and configuration of this [Singer Tap](https://singer.io) depends of the desired replication mode (INCREMENTAL or STREAMING)

## Log based replication

Tap-Oracle Log-based replication requires some configuration changes in Oracle database:

* Enable `ARCHIVELOG` mode

* Set retention period a reasonable and long enough period, ie. 1 day, 3 days, etc.

* Enable Supplemental logging

### Setting up Log-based replication on a self hosted Oracle Database: 

To verify the current archiving mode, if the result is `ARCHIVELOG`, archiving is enabled:
```
  SQL> SELECT LOG_MODE FROM V$DATABASE
```

To enable `ARCHIVELOG` mode (if not enabled yet):
```
  SQL> SHUTDOWN IMMEDIATE
  SQL> STARTUP MOUNT
  SQL> ALTER DATABASE ARCHIVELOG
  SQL> ALTER DATABASE OPEN
```

To set retention period, use RMAN:
```
  RMAN> CONFIGURE RETENTION POLICY TO RECOVERY WINDOW OF 1 DAYS;
```

To enable supplemental logging:
```
  SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS
```


### Install and Run

First, make sure Python 3 is installed on your system or follow these
installation instructions for [Mac](http://docs.python-guide.org/en/latest/starting/install3/osx/) or
[Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-16-04).


It's recommended to use a virtualenv:

```bash
  python3 -m venv venv
  pip install extended-tap-oracle
```

or

```bash
  python3 -m venv venv
  . venv/bin/activate
  pip install --upgrade pip
  pip install .
```

### Configuration

Running the the tap requires a `config.json` file. Example with the minimal settings:

```json
  {
    "host": "foo.com",
    "port": 1521,
    "user": "my_user",
    "password": "password",
    "sid": "ORCL",
    "filter_schemas": "MY_USER" # optional
  }
```

