# Restore proof-of-concept script for restoring data from Duplicacy

This script is here to ensure that data can actually be restored from b2 storage
with a fresh install.

To make this work, first a create file `./duplicacy-preferences` with the following contents:
```
[
    {
        "name": "default",
        "id": "desktop",
        "repository": "",
        "storage": "b2://<duplicacy bucket - see b2 buckets overview or password manager>",
        "encrypted": true,
        "no_backup": false,
        "no_restore": false,
        "no_save_password": false,
        "nobackup_file": "",
        "keys": {
            "b2_id": "<username per Lastpass>",
            "b2_key": "<password per Lastpass>",
            "password": "<see comment in Lastpass entry>"
        },
        "filters": "",
        "exclude_by_attribute": false
    }
]
```

Where the storage url, `b2_id`, `b2_key` and `password` are filled in from e.g. a password manager.

At this point, you should be able to run `docker-compose build` and `docker-compose up` to list the backups as a proof-of-concept.

Adjust the command as needed to restore the data: Mount volumes etc.
