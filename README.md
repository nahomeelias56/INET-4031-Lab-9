# INET-4031 Lab 9 - Puppet Infrastructure as Code

This repository contains Puppet manifests and supporting files for automating server configuration, including LAMP stack deployment and user-group management on Linux systems.

## Files

### `lamp_stack_server.pp`
A Puppet manifest that provisions a full LAMP stack. It handles the installation and configuration of the following:
* **Apache2** — Installs the Apache web server and ensures the service is running and enabled.
* **PHP and modules** — Installs `php`, `libapache2-mod-php`, `php-cli`, and `php-mysql`. Each package is configured to notify Apache upon installation, triggering a service restart to apply changes.
* **phpinfo.php** — Deploys a PHP info page to `/var/www/html/phpinfo.php` for verifying the PHP installation.

Dependencies are enforced so that PHP packages are only installed after Apache, and services only start after their required packages are installed.

### `phpinfo.php`
A minimal PHP script deployed to the web server's root. When accessed through a browser, it calls `phpinfo()` to display a test PHP page with information.

### `server_users_groups.pp`
A Puppet manifest file that manages user accounts and groups on the server. It creates the following resources:
* **Groups** — Creates `group01` and `group02`.
* **Users** — Creates four user accounts (`user04` through `user07`), each configured with a Bash shell, a hashed password, and a managed home directory. Group memberships are assigned as follows:
   * `user04` — member of `group01`
   * `user05` — member of `group02`
   * `user06` — member of both `group01` and `group02`
   * `user07` — no group membership

### `testing_puppet.pp`
A simple test manifest that creates a file in the `/tmp` directory to verify Puppet is working correctly.

## Requirements
* Puppet installed on the server
* An Ubuntu-based system

## Running the Files
With sudo permissions, use the command

```
sudo puppet apply server_users_groups.pp
```

to create the pre-defined users and their groups. Afterwards, run

```
sudo puppet apply lamp_stack_server.pp
```

to start the installation and configuration of the LAMP stack.
