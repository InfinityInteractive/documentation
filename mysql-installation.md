# 🛠 MySQL Installation

### Setting Up a MySQL Database for Pterodactyl

MySQL is a fundamental component of the Pterodactyl Panel, but it can be intimidating to set up if you're new to the process. This basic tutorial provides a simple overview to get MySQL up and running with the panel. If you want to explore advanced features, consider referring to more comprehensive tutorials available online.

#### Step 1: Logging In

The first step is to log in to the MySQL command line, where you'll execute essential statements to set up your database. Follow these instructions and provide the password for the Root MySQL account you set during MySQL installation. If you don't recall setting a password, you can often press Enter to proceed without one.

```bash
mysql -u root -p
```

#### Step 2: Creating a User

For security reasons and due to changes in MySQL 5.7, it's recommended to create a dedicated user for the Pterodactyl panel. Start by instructing MySQL to use the `mysql` database, which manages user-related information.

```sql
 USE mysql;
```

Next, create a user named 'pterodactyl' and permit logins only from localhost to enhance security and prevent external connections. You can also use '%' as a wildcard or specify a numeric IP. Ensure you replace `'somePassword'` with a strong, unique password of your choice.

```sql
 CREATE USER 'pterodactyl'@'127.0.0.1' IDENTIFIED BY 'somePassword';
```

#### Step 3: Creating a Database

Now, it's time to create a database specifically for the Pterodactyl panel. In this tutorial, we'll name the database 'panel,' but feel free to choose an alternative name if you prefer.

```sql
 CREATE DATABASE panel;
```

#### Step 4: Assigning Permissions

To allow the 'pterodactyl' user to access the 'panel' database, execute the following command:

```sql
 GRANT ALL PRIVILEGES ON panel.* TO 'pterodactyl'@'127.0.0.1';
```

This command grants the necessary permissions to ensure the Pterodactyl panel can interact seamlessly with the MySQL database.

#### Creating a Database Host for Nodes

_Note: This section focuses on creating a MySQL user with privileges to create and modify users, a crucial requirement for the Panel to generate per-server databases on the specified host._

#### Step 5: Creating a User

If your database resides on a different host than the one hosting your Pterodactyl Panel or Daemon, be sure to utilize the IP address of the machine where the Panel is running. Using '127.0.0.1' for external connections will result in a 'connection refused' error. Customize the username and password to your preferences.

```sql
 CREATE USER 'pterodactyluser'@'127.0.0.1' IDENTIFIED BY 'somepassword';
```

#### Step 6: Assigning Permissions

The following command bestows your newly created user with the ability to create additional users and manage databases. As before, confirm that '127.0.0.1' corresponds to the IP address you used in the previous command.

```sql
 GRANT ALL PRIVILEGES ON *.* TO 'pterodactyluser'@'127.0.0.1' WITH GRANT OPTION;
```

#### Step 7: Allowing External Database Access

If you need to enable external access to your MySQL instance, allowing game servers to connect, follow these steps:

1.  Locate and open your `my.cnf` file, the configuration file for MySQL. The file's location may vary depending on your operating system and MySQL installation method. You can use the command below to locate it:

    ```bash
    find /etc -iname my.cnf
    ```
2.  Open `my.cnf` and add the following text at the end of the file:

    ```bash
    [mysqld]
    bind-address=0.0.0.0
    ```
3. Save the file.
4. Restart the MySQL or MariaDB service to apply these changes. This configuration change overrides the default MySQL settings, which typically accept requests only from localhost. By allowing `bind-address` to be `0.0.0.0`, you enable connections on all interfaces, including external connections. Ensure that your firewall allows traffic on the MySQL port, typically port 3306.

If your database and Pterodactyl Wings are hosted on the same machine and do not require external access, you can alternatively use the 'docker0' interface's IP address rather than '127.0.0.1.' You can find this IP address by running the command `ip addr | grep docker0`, which typically appears as '172.x.x.x.'.

With these steps, you've successfully configured a MySQL database for Pterodactyl, ensuring the necessary user privileges and, if needed, enabling external access for game servers. Please take into account security best practices and tailor the configurations to your specific requirements.
