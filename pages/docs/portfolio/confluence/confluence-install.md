---
title: Confluence Install Guide
sidebar: second_sidebar 
toc: false
permalink: confluence-install.html
---

This install guide was written for someone with mid-level Linux system administration skills who is familiar with installing software from the command line. This covers a basic install of Confluence on a single server, and does not cover clustering, database replication and failover, or any type of high availability. 

---

# Confluence Install Guide

### Overview

This user guide will explain how to install Atlassian Confluence Enterprise Wiki software on your Virtual Server.

This guide assumes that you are able to connect to the command line of the server using an SSH client, work as the *root* user, and are able to
navigate the Linux file system, install and configure software, and edit files using a text editor.

**NOTE** - this install guide uses the [Vim](http://www.vim.org/ "Vim") text editor for all file configuration examples. If you are unfamiliar
with Vim, the end of this document has a [quick tutorial on using Vim](#addendum---a-very-short-vim-tutorial) that should cover the basics of using the editor.

### Prerequisites

Before the installation process can begin, you will need the following:

*   A Virtual Server built with the latest version of CentOS Linux - choose a minimal server install if possible
*   1.5 GB of RAM at a minimum
*   25 GB of disk space at a minimum
*   1 public IP address at a minimum

You will also need:

*   A **My Atlassian** account - <https://my.atlassian.com/>  
*   A valid Confluence license for the correct number of users (purchased from your My Atlassian account)

[Server Preparation](#server-preparation)  
     [Additional Software Installation](#additional-software-installation)  
     [Set the MySQL root password](#set-the-mysql-root-password)

[Installing Confluence](#installing-confluence)  
     [Download the Confluence installer from Atlassian](#download-the-confluence-installer-from-atlassian)  
     [Set the installer as an executable](#set-the-installer-as-an-executable)  
     [Run the installer - follow the prompts](#run-the-installer---follow-the-prompts)  
     [Set up the MySQL database for Confluence](#set-up-the-mysql-database-for-confluence)  
     [Move to the browser-based installer (this is where you install the license)](#move-to-the-browser-based-installer-this-is-where-you-install-the-license)  

[Addendum: A Very Short Vim Tutorial](#addendum---a-very-short-vim-tutorial)

---

## Server Preparation

Confluence is a Java application that uses the Tomcat application server to serve dynamic content. The Confluence install binary contains a JVM
(Java) and Tomcat, so **DO NOT** install those before installing Confluence. Having Java and Tomcat installed before installing
Confluence will cause port conflicts and various other problems that can be difficult to troubleshoot.

### Additional Software Installation

Once the server has been provisioned and built, you will need to install some additional software before you can install Confluence. Software is
installed from the command line as the *root* user, using the *yum* package manager. You will need to install the following:

*   Sendmail (a mail server, used to send messages from Confluence)
*   *wget* (used to download Confluence)
*   CentOS Development tools group (general tools)
*   MySQL database server

You will need to connect to the server using the Secure SHell (SSH), which encrypts the connection between your local computer and the
server. Once you are connected via SSH you will be at the command line of server.

The commands used are:

*   **`yum update -y`** - this updates the *yum* application so that it is looking at the latest version of the repositories
*   **`yum install -y sendmail sendmail-cf sendmail-doc`** - *sendmail* is the mail server Confluence will use to send announcements
*   **`yum install -y wget`** - *wget* is the application that will be used to download Confluence
*   **`yum groupinstall -y "Development tools"`** - the Dev tools are a suite of applications needed to install and configure software
*   **`yum install -y mysql-server`** - MySQL is the database that will be used by Confluence

At the command prompt, you will type in each command one at a time and hit Enter (or Return). Let each command complete - some of them will
take several minutes to finish. You will taken back to the command prompt once the command completes.

### Set the MySQL root password

Once all the additional software is installed, you will need to set a MySQL *root* password. **NOTE** - this is not the same as the *root*
password for the system super-user! This is a password for the MySQL database *root* user.

This is done before installing Confluence so that the database server is secure during the time it takes to install Confluence and the time it
takes to actually set up the database itself.

Change directories to the */root* directory, and create a file called ***.my.cnf***. Note the dot (.) at the beginning of the file name.
Create the file using the **`touch`** command, and then edit it using a text editor (vim in this example).

```shell
[root@example ~]# cd /root
[root@example ~]# touch .my.cnf
[root@example ~]# vim .my.cnf
```    

Add this info to the `.my.cnf` file, and save and exit.

```shell
[client]
password=use_a_strong_passwd
```

Choose a strong password, preferably using a password generator. Several are available online, such as the [Strong Password
Generator](https://strongpasswordgenerator.com/ "Strong Password Generator").  If the MySQL *root* password is easy to guess, a hacker could compromise
your database and access all the information in it.

This ends the server preparation. The next step is to install Confluence.

---

## Installing Confluence

The first part of the Confluence install is done from the command line of the server, as the *root* user. Once that is complete, you will move
to a browser to finish the last part of the configuration. After that you can begin using Confluence.

The steps to install Confluence are:

1.  Download the Confluence installer from Atlassian
2.  Set the installer as an executable
3.  Run the installer - follow the prompts
4.  Set up the MySQL database for Confluence
5.  Move to the browser-based installer (this is where you install the license)
6.  Begin using Confluence

### Download the Confluence installer from Atlassian

The Confluence installers are here: [Confluence installers](https://www.atlassian.com/software/confluence/download "Confluence Installers").
The page will default to the OS of the computer you use, so make sure to select **Linux 64 Bit** from the drop down menu. This will default to
the most current version of the installer.

However, instead of downloading the software from this page, right-click on the **Download** button, and select the option that allows you to
*Copy Link Address* (the wording may vary depending on your browser).  You do not need to save or open the link, just copy the link address.

Connect to the server using SSH, and as the *root* user change directories to the */tmp* directory (note the leading slash). Then use
the **`wget`** command to download the installer. You should be able to type in **`wget`**, a space, and then paste the link address into the
command prompt.

```shell
[root@example ~]# cd /tmp
[root@example ~]# wget https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-X.X-x64.bin
```
This will download the Confluence installer to your server. Depending on your connection speed, network traffic, etc., this may take a few
minutes. When the download is complete you will be returned to the command prompt.

### Set the installer as an executable

By default, the Confluence installer is not executable, meaning you cannot run the installer program. This is by design, and done for
security reasons. You will need to set the installer to executable with the **`chmod +x atlassian-confluence-X.X-x64.bin`** command. Remember to
substitute `X.X` with the version number of the installer you downloaded.

```shell
[root@example ~]# chmod +x atlassian-confluence-X.X-x64.bin
```

### Run the installer - follow the prompts

Once the installer has been set to executable, you can run the installer program, and follow the prompts to install Confluence. Generally
speaking you can just accept the defaults for each choice.

Start the install process with the **`./atlassian-confluence-X.X-x64.bin`** command. The command is:
`dot slash atlassian-confluence-X.X-x64.bin` (all together, no spaces).  After you have entered that command, press ENTER (or RETURN).

This is an example of what you will see during the install process.  What's inside the angle brackets is what you'll type at the prompt
whenever the system asks you for an action. For example, \< o, Enter \> means to press the letter o and then press Enter.

```shell
[root@example ~]# ./atlassian-confluence-X.X-x64.bin
Unpacking JRE ...
Starting Installer ...


This will install Confluence X.X on your computer.
OK [o, Enter], Cancel [c]
< o, Enter >

Choose the appropriate installation or upgrade option.
Please choose one of the following:
Express Install (uses default settings) [1], Custom Install (recommended for advanced users) [2, Enter], Upgrade an existing Confluence installation [3]
< 2, Enter >

Where should Confluence X.X be installed?
[/opt/atlassian/confluence]
< Enter >

Default location for Confluence data
[/var/atlassian/application-data/confluence]
< Enter >

Configure which ports Confluence will use.
Confluence requires two TCP ports that are not being used by any other applications on this machine. The HTTP port is where you will access
Confluence through your browser. The Control port is used to Startup and Shutdown Confluence.
Use default ports (HTTP: 8090, Control: 8000) - Recommended [1, Enter], Set custom value for HTTP and Control ports [2]
< 1, Enter >

Confluence can be run in the background.
You may choose to run Confluence as a service, which means it will start automatically whenever the computer restarts.
Install Confluence as Service?
Yes [y, Enter], No [n]
< y, Enter >

Extracting files ...



Please wait a few moments while Confluence starts up.
Launching Confluence ...
Installation of Confluence X.X is complete
Your installation of Confluence X.X is now ready and can be accessed via your browser.
Confluence X.X can be accessed at http://localhost:8090
Finishing installation ...
[root@example tmp]#
```

After the install completes, you will need to move on to the MySQL database configuration before finishing the rest of the Confluence setup
from the browser.

### Set up the MySQL database for Confluence

In this section you will start the MySQL database server, secure it, and create the database that Confluence will use. You will also add some
settings to the file that controls some of the variables that MySQL uses.

You will need the MySQL *root* password that you set in the [Set the MySQL root password](#set-the-mysql-root-password) section, and you will
also need to create a database password for the Confluence database. You will want that password to be as secure as the MySQL *root* password so
you will again want to use a [Strong Password Generator](https://strongpasswordgenerator.com/ "Strong Password Generator")
for this.

Start the MySQL database server with the **`service mysqld start`** command. Note the **d** at the end - **mysqld**.

```shell
[root@example ~]# service mysqld start
```

If this is the first time that the MySQL database server has run, there will be a lot of output generated on the screen. Wait until you are
returned to the command prompt, and then run the following command to secure the MySQL server: **`/usr/bin/mysql_secure_installation`**

```shell
[root@example ~]# /usr/bin/mysql_secure_installation
```

You will be asked for the current MySQL *root* password - enter it when prompted. If you're asked to set the MySQL *root* password again, select
**n** for no. As part of the process to secure the database, you will see the following prompts:

*   **Remove anonymous users?** - select **y**
*   **Disallow root login remotely?** - select **y**
*   **Remove test database and access to it?** - select **y**
*   **Reload privilege tables now?** - select **y**

This will drop you back to the command prompt once the tables are reloaded.

The next step is to create the actual database that Confluence will use.  This involves creating the database, the database user, and the database
user password. As always, make sure to use a secure password for the database user. All of this is done from the command prompt of the MySQL
server.

The commands you will need to use are as follows. An example of how this will look is also below. By convention, MySQL commands are always shown
in UPPERCASE. However, you do not actually need to use uppercase letters, the commands are not case-sensitive. Also, all MySQL commands
end with a semi-colon (;).

*   **`mysql -u root -p`** - this logs you in to the MySQL database server as the MySQL *root* user. You will be prompted for the MySQL
    *root* password.
*   **`CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin;`** - this creates the actual database, called *confluence*
*   **`GRANT ALL PRIVILEGES ON confluence.* TO 'confluence_db_user'@'localhost' IDENTIFIED BY 'secure_password';`** - this creates the database user and sets the password for that user. Choose a database user name that you will remember and as always use a secure password.


```shell
[root@example ~]# mysql -u root -p
Enter password:

Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 5.0.95 Source distribution

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin;
mysql> GRANT ALL PRIVILEGES ON confluence.* TO 'confluence_db_user'@'localhost' IDENTIFIED BY 'secure_password';
mysql> quit;
[root@example ~]#
```

After you have created the database and database user / password, you will need to edit the file that controls some settings for MySQL. The
file is located at */etc/my.cnf*. Note the dot between my and cnf.

```shell
[root@example ~]# cd /etc
[root@example ~]# vim my.cnf
```

The *my.cnf* file is divided into sections. Add the following to the `[mysqld]` section:

```shell
character-set-server=utf8
collation-server=utf8_bin
default-storage-engine=INNODB
max_allowed_packet=32M
```

Once you have made these changes, save and exit the file, and restart the MySQL database server with the ***`service mysqld restart`*** command.

```shell
[root@example ~]# service mysqld restart
```

This ends the command line install part of Confluence. The rest of the install process takes place in the browser.

### Move to the browser-based installer (this is where you install the license)

The last part of the Confluence install takes place in the browser. Open a browser, and go to ***http://IP\_ADDRESS:8090***, where IP\_ADDRESS is
the IP address of your server. If the browser will not open the page, the connection is usually being blocked by some kind of firewall application.

From the first page that opens, you will need to copy the **Server ID**, which is just above the box where you enter the License Key. Take the
Server ID, and go to your **My Atlassian** account. There will be a place there to generate the License Key by entering the Server ID.

The final steps to finalize the installation are as follows:

1.  Choose a **Production Installation** - this allows you to use the MySQL database
2.  When prompted, choose MySQL in the **Choose Database** screen
3.  You will need to choose the **JDBC** option to connect the database to Confluence
4.  You will need to enter the Confluence database user and password when prompted

This takes a few minutes to run. If there is a problem, it will error out almost immediately.

This should complete the Confluence installation.

---

## Addendum - A Very Short Vim Tutorial

Vim is a very complex and powerful text editor. However, you only need to use a very small set of commands in order to edit the files
referenced in this install guide. Basically, you need to know how to:

1.  Open a file for editing
2.  Move to a location in the file
3.  Add text to the file
4.  Save and exit the file

### 1. Open a file for editing

To open a file for editing, use the **`vim name_of_file`** command from the command prompt. For example, to open the **`.my.cnf`** file:

```
vim .my.cnf
```

### 2. Move to a location in the file

If the file you're editing has text in it already, you can navigate to the location where you need to enter your text using the arrow keys.
However, Vim is a modal editor, and you have to be in command mode to move around. So before you use the arrow keys, hit the **`Esc`**
(Escape) key twice. The `Esc` key will always take you out of insert mode and back to command mode. After hitting the `Esc` key twice, use
the up/down/left/right arrow keys as needed to move you to your desired location.

### 3. Add text to the file

Once the cursor is where you want it to be, you need to enter insert mode to add your text. Press the lower-case **i** key. This will allow
you to type in the text you need to add. Using the Return or Enter key will create a new line if needed.

**TIP** - if you make a mistake, hit the `Esc` key twice, and then use the lower-case **x** key to erase what you've typed, and then use the
**i** key again to get back to insert mode.

### 4. Save and exit the file

Once you've added your text or made your changes to the file you can save and exit. Hit the `Esc` key twice to get back to command mode, and
then type in **`:wq`**. That is a `colon w q` all together with no spaces. This will save and exit the file, and get you back to the
command prompt.

  
