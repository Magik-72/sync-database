# Description
Use this package to synchronize your local development MySQL database with a remote one (e.g., production).
It'll connect through SSH to a remote server, make a dump, download it and import it into your local MySQL database.

# Prerequisites
### Local environment
* mysql binary (`mysql --version`)
* GZip (`gzip --version`)

### Remote environment
* mysqldump binary (`mysqldump --version`)
* GZip (`gzip --version`)

# Limitations
* This package only supports MySQL
* It only supports databases hosted on the remote server you provided (local machine > remote machine > dump > local machine)

# Installation
`composer require toyi/sync-database`

### Before Laravel 5.5
After updating composer, add the SyncDatabaseProvider to the providers array in `config/app.php`
`Toyi\SyncDatabase\SyncDatabaseProvider::class,`

# Usage
You should publish the `config/sync-database.php` config file using `php artisan vendor:publish --provider="Toyi\SyncDatabase\SyncDatabaseProvider"`.

The `ssh` part will be used to connect to your remote server.\
The `database` part will be used to dump the remote database.

Once everything is configured, use `php artisan toyi:sync-database`.

Available options : 
* `--tables-no-data` A comma separated list of tables. Only their structure will be dumped (no data)
* `--no-migrations` Don't execute pending migrations at the end.

**The remote database will only be dumped, nothing will ever be written.**

The temporary files are deleted once the dump process is over.\
However, should something fail, they might persist (and take unecessary disk space). If this is the case, you can delete them yourself (they will be located in the /tmp directory of both the local and remote machines).
