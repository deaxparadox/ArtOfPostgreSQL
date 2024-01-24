# Load PostgreSQL Sample Database

For loading sample database, you need to have:

- A PostgreSQL database server installed on your system.
- A PostgreSQL sample data called dvdrental.

### Load the sample database using psql tool

First, launch the psql tool:

```bash
$ psql
```

Second login to the PosgreSQL server if needed.

Third, enter the following `CREATE DATABASE` statement to create a new *dvdrental** database:

```sql
postgres=# CREATE DATABASE dvdrental;
CREATE DATABASE
```

PostgreSQl will create a new database named `dvdrental`.

Finally, enter the `exit` command to `quit` psql:

```sql
postgres=# exit
```

Then, navigate the folder of the PostgreSQL installation folder, by defualt `/var/lib/postgresql` in debian:

```bash
cd /var/lib/postgreql/
```

After that, use the **pg_restore** tool to load data into the **dvdrental** database:

```bash
pg_restore -U postgres -d dvdrental /path/to/dvdrental.tar
```

In this command:

- The `-U postgres` specifies the `postgres` user to login to the PostgreSQL.
- The `-d dvdrental` specifies the target database to load.

Finally, enter the password for the **postgres** use and press enter.

It takes about seconds to load data stored in the `dvdrental.tar` file into the `dvdrental` database.

