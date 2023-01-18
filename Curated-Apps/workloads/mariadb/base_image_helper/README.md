This directory contains steps and artifacts to create a docker image and create encrypted data directory.

# Prerequisites

- Data directory Encryption is done using `gramine-sgx-pf-crypt` tool which is part of
  [Gramine installation](https://gramine.readthedocs.io/en/latest/quickstart.html#install-gramine).

- Download MariaDB 10.8.6 on ubuntu 20.04:

  - `curl -LsS https://r.mariadb.com/downloads/mariadb_repo_setup | sudo bash -s
  -- --mariadb-server-version="mariadb-10.8.6" --os-type=ubuntu --os-version=focal`
  - `sudo apt install mariadb-server`

- Run `mysql_secure_installation`:

  - sudo mysql_secure_installation

    - Enter current password for root (enter for none): press enter
    - Switch to unix_socket authentication [Y/n]: n
    - Change the root password? y
    - Remove anonymous users? y
    - Disallow root login remotely? y
    - Remove test database and access to it? y
    - Reload privilege tables now? y

- Connect to the server and allow root to connect to any IP:

  - `sudo mysql`
  - `CREATE USER 'root'@'%' IDENTIFIED BY '$root-password'; GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;` (Use the root password changed in previous step)

- Stop MariaDB server:

  - `systemctl stop mysqld`

- Mariadb is installed by mysql user so change owner of below folders to current user:

  - `sudo chown -R $USER /var/lib/mysql`
  - `sudo chown -R $USER /run/mysqld`

# Base docker image creation

- Execute `bash ./helper.sh` command to encrypt the data directory and create base image.

Please refer to `Curated-Apps/workloads/mariadb/README.md` to curate the image created in above
steps with GSC.
