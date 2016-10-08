---
title: Confluence Install Guide
sidebar: second_sidebar 
toc: false
permalink: confluence_install.html
---

This install guide was written for a fellow member of a mailing list for technical writers. They wanted to install and evaluate [Confluence](https://www.atlassian.com/software/confluence "Confluence") for their user-facing documentation, but were having problems following the [official documentation](https://confluence.atlassian.com/conf57/installing-confluence-on-linux-701435875.html "Confluence Install docs") and posted on the mailing list asking for help. I had installed Confluence numerous times when my then employer was also evaluating it, so I was able to provide an install guide based on my notes. **Note** - this install guide is based on Confluence 5.7, which was the most current version at the time. 

I specifically wrote this for someone with only low to moderate skill with Linux and system administration, but tried to keep it high level enough to not get bogged down in the "why" and just concentrate on the "how." 

---

# Installing Confluence

This document explains how to install Atlassian Confluence Enterprise Wiki software on a Linux server. This is based on my current install notes (for Confluence 5.7).

### Prerequisites

Before the installation process can begin, you will need the following hardware:

* Linux server running Red Hat Enterprise Linux (RHEL) or a RHEL clone such as CentOS Linux
* 1.5 GB of RAM at a minimum
* 25 GB of disk space at a minimum
* 1 public IP address (Internet access is assumed)

The server can be either a standalone physical server or a virtual server. My current Confluence instance runs on a virtual server.

You will also need:

* root access to the server over SSH
* A My Atlassian account - https://my.atlassian.com/
* A valid Confluence license for the correct number of users (purchased from your My Atlassian account)

This install guide assumes that you are able to connect to the command line of the server using an SSH client, work as the root user, and are able to navigate the Linux file system, install and configure software, and edit files using a text editor. I realize that you may not have all these skills, so I will try and explain as much as I can without getting bogged down into too much detail. I can answer any questions you may have about the specifics of certain commands if needed.

**NOTE** - in this install guide I show examples of editing files using the [Vim](http://www.vim.org/ "Vim") text editor. Vim is a highly configurable text editor often used by programmers and system administrators. At the end of this document is a [quick tutorial on using Vim](#addendum---a-very-short-vim-tutorial) that should cover most of what you need to do. If Vim is too complex (and it is a very complex piece of software), see if your server has `nano` or `pico` installed, which are more user-friendly. 

You should be able to hand this install guide to someone with moderate to advanced Linux skills and they should be able to follow along with no issues.

[Server Preparation](#server-preparation)  
&nbsp; &nbsp; [Additional Software Installation](#additional-software-installation)  
&nbsp; &nbsp; [Set the MySQL root password](#set-the-mysql-root-password)  

[Installing Confluence](#installing-confluence-1)  
&nbsp; &nbsp; [Download the Confluence installer from Atlassian](#download-the-confluence-installer-from-atlassian)  
&nbsp; &nbsp; [Set the installer as an executable](#set-the-installer-as-an-executable)  
&nbsp; &nbsp; [Run the installer - follow the prompts](#run-the-installer---follow-the-prompts)  
&nbsp; &nbsp; [Set up the MySQL database for Confluence](#set-up-the-mysql-database-for-confluence)  
&nbsp; &nbsp; [Move to the browser-based installer (this is where you install the license)](#move-to-the-browser-based-installer-this-is-where-you-install-the-license)  
&nbsp; &nbsp; [Enjoy using Confluence](#enjoy-using-confluence)  

[Addendum: A Very Short Vim Tutorial](#addendum---a-very-short-vim-tutorial)  

---

## Server Preparation

Confluence is a Java application that uses the Tomcat application server to serve dynamic content. The Confluence install binary contains a JVM (Java) and Tomcat, so do not install those before installing Confluence. Having Java and Tomcat installed before installing Confluence will cause port conflicts and various other problems that can be difficult to troubleshoot.

Here are the details for my current server:

* Server OS: CentOS Linux 6.5 (Final) x86_64 (CentOS is a RHEL clone)
* RAM: 1536 MB (1.5 GB)
* HDD: 25 GB

When CentOS is installed, you are usually prompted to install certain packages or groups of packages. Try to pick a minimal install - you don't need any games or desktop GUIs or office suites. You can install the Apache HTTP server, but I would not install MySQL at this part of the process.

Installing CentOS is far beyond the scope of this doc, but detailed instructions can be found here - [RHEL Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/ "RHEL Install Guide"). Due to licensing issues, CentOS is no longer able to provide this document themselves, it has to come from the upstream source.

At my work we have a CentOS template available that is fairly bare-bones - no Apache, no databases, no PHP - and that is what I always start with.

### Additional Software Installation

Once the server has been provisioned and built, you will need to install some additional software before you can install Confluence. Software is installed from the command line as the root user, using the yum package manager. You will need to install the following:

* Sendmail (a mail server)
* wget (used to download Confluence)
* CentOS Development tools group (general tools)
* MySQL database server

You will need to connect to the server using the Secure SHell (SSH), which encrypts the connection between your local computer and the server. Once you are connected via SSH you will be at the command line of server, which is similar to a DOS prompt but much more powerful.

The commands used are:

* **`yum update -y`** - this updates the yum application so that it is looking at the latest version of the repositories
* **`yum install -y sendmail sendmail-cf sendmail-doc`** - sendmail is the mail server Confluence will use to send announcements
* **`yum install -y wget`** - wget is the application that will be used to download Confluence
* **`yum groupinstall -y "Development tools"`** - the Dev tools are a suite of applications needed to install and configure software
* **`yum install -y mysql-server`** - MySQL is the database that will be used by Confluence

At the command prompt, you will type in each command one at a time and hit Enter (or Return). Let each command complete - some of them will take several minutes to finish. You will taken back to the command prompt once the command completes.

### Set the MySQL root password

Once all the additional software is installed, you will need to set a MySQL root password. **NOTE** - this is not the same as the root password for the system super-user! This is a password for the MySQL database root user.

This is done before installing Confluence so that the database server is secure during the time it takes to install Confluence and the time it takes to actually set up the database itself.

Change directories to the */root* directory, and create a file called ***.my.cnf***. Note the dot (.) at the beginning of the file name. Create the file using the **`touch`** command, and then edit it using a text editor (vim in this example). 

```sh
[root@example ~]# cd /root
[root@example ~]# touch .my.cnf
[root@example ~]# vim .my.cnf
```

Add this info to the .my.cnf file, and save and exit.

```sh
[client]
password=secure_password
```

Choose a strong password, preferably using a password generator. Several are available online, such as the [Strong Password Generator](https://strongpasswordgenerator.com/ "Strong Password Generator"). If the MySQL root password is easy to guess, a hacker could compromise your database and access all the information in it.

This ends the server preparation. The next step is to install Confluence.

---

## Installing Confluence

The first part of the Confluence install is done from the command line of the server, as the root user. Once that is complete, you will move to a browser to finish the last part of the configuration. After that you can begin using Confluence.

The steps to install Confluence are:

1. Download the Confluence installer from Atlassian
2. Set the installer as an executable
3. Run the installer - follow the prompts
4. Set up the MySQL database for Confluence
5. Move to the browser-based installer (this is where you install the license)
6. Enjoy using Confluence

###  Download the Confluence installer from Atlassian

The Confluence installers are here: [Confluence installers](https://www.atlassian.com/software/confluence/download "Confluence Installers"). The page will default to the OS you're currently using, so make sure to select the Linux tab. What you're going to want to do is get the link to the installer so that you can download it directly to the server where you're installing Confluence. You could download the installer to your local computer and then upload it to the server, but that requires an unnecessary extra step.

I use the most current version of the Linux 64 bit installer, which is currently Confluence 5.7 Server - Linux Installer (64 bit). The version number will change, but unless you know for a fact that you're using a 32 bit system, get the 64 bit installer.

Right-click on the Trial download button, and select the option that lets you *Copy Link Address* (the wording my vary depending on your browser). You don't need to save the link address, or open, you just want to copy the link address.

Connect to the server using SSH, and as the root user change directories to the */tmp* directory (note the leading slash). Then use the **`wget`** command to download the installer. You should be able to type in **`wget`**, a space, and then paste the link address into the command prompt.

```sh
[root@example ~]# cd /tmp
[root@example ~]# wget https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-5.7-x64.bin
```

This example uses the URL of the current (2015-02-20) Confluence installer, the URL you use may be slightly different.

This will download the Confluence installer to your server. Depending on your connection speed, network traffic, etc, this may take a few minutes. You'll know that the download is complete when you're returned to the command prompt.

### Set the installer as an executable

By default, the Confluence installer isn't executable, meaning you cannot run the installer program. This is by design, and done for security reasons. You will need to set the installer to executable with the **`chmod +x atlassian-confluence-5.7-x64.bin`** command.

**NOTE** - the intricacies of UNIX/Linux file permissions are too esoteric to get into here. This is a quick overview: [Unix - File Permissions](https://www.tutorialspoint.com/unix/unix-file-permission.htm "Unix File Permissions"). If you're really interested I'll be glad to go into more detail, but the "how" is only relevant here, not the "why".

```sh
[root@example ~]# chmod +x atlassian-confluence-5.7-x64.bin
```

### Run the installer - follow the prompts

Once the installer has been set to executable, you can run the installer program, and follow the prompts to install Confluence. Generally speaking you can just accept the defaults for each choice. I've installed Confluence a dozen times or so, and I've never strayed from the defaults.

Start the install process with the **`./atlassian-confluence-5.7-x64.bin`** command. The command is: `dot slash atlassian-confluence-5.7-x64.bin` (all together, no spaces). After you have entered that command, press ENTER (or RETURN).

This is an example of what you will see during the install process. What's inside the angle brackets and is what you'll type at the prompt whenever the system asks you for an action. For example, < o, Enter > means to press the letter o and then press Enter.

```sh
[root@example ~]# ./atlassian-confluence-5.7-x64.bin
Unpacking JRE ...
Starting Installer ...

This will install Confluence 5.7 on your computer.
OK [o, Enter], Cancel [c]
< o, Enter >

Choose the appropriate installation or upgrade option.
Please choose one of the following:
Express Install (uses default settings) [1], Custom Install (recommended for advanced users) [2, Enter], Upgrade an existing Confluence installation [3]
< 2, Enter >

Where should Confluence 5.7 be installed?
[/opt/atlassian/confluence]
< Enter >

Default location for Confluence data
[/var/atlassian/application-data/confluence]
< Enter >

Configure which ports Confluence will use.
Confluence requires two TCP ports that are not being used by any other
applications on this machine. The HTTP port is where you will access
Confluence through your browser. The Control port is used to Startup and
Shutdown Confluence.
Use default ports (HTTP: 8090, Control: 8000) - Recommended [1, Enter], Set custom value for HTTP and Control ports [2]
< 1, Enter >

Confluence can be run in the background.
You may choose to run Confluence as a service, which means it will start
automatically whenever the computer restarts.
Install Confluence as Service?
Yes [y, Enter], No [n]
< y, Enter >
Extracting files ...



Please wait a few moments while Confluence starts up.
Launching Confluence ...
Installation of Confluence 5.7 is complete
Your installation of Confluence 5.7 is now ready and can be accessed via your browser.
Confluence 5.7 can be accessed at http://localhost:8090
Finishing installation ...
[root@example tmp]#
```

After the install completes, you will need to move on to the MySQL database configuration before finishing the rest of the Confluence setup from the browser.

### Set up the MySQL database for Confluence

In this section you will start the MySQL database server, secure it, and create the database that Confluence will use. You will also add some settings to the file that controls some of the variables that MySQL uses.

You will need the MySQL root password that you set in the [Set the MySQL root password](#HREF TO ANCHOR) section, and you will also need to create a database password for the Confluence database. You will want that password to be as secure as the MySQL root password so you will again want to use a [Strong Password Generator](https://strongpasswordgenerator.com/ "Strong Password Generator") for this.

Start the MySQL database server with the **`service mysqld start`** command. Note the **d** at the end - **mysqld**.

```sh
[root@example ~]# service mysqld start
```

If this is the first time that the MySQL database server has run, there will be a lot of output generated on the screen. Wait until you are returned to the command prompt, and then run the following command to secure the MySQL server: **`/usr/bin/mysql_secure_installation`**

```sh
[root@example ~]# /usr/bin/mysql_secure_installation
```

You will be asked for the current MySQL root password - enter it when prompted. If you're asked to set the MySQL root password again, select **n** for no. As part of the process to secure the database, you will see the following prompts:

* **Remove anonymous users?** - select **y**
* **Disallow root login remotely?** - select **y**
* **Remove test database and access to it?** - select **y**
* **Reload privilege tables now?** - select **y**

This will drop you back to the command prompt once the tables are reloaded.

The next step is to create the actual database that Confluence will use. This involves creating the database, the database user, and the database user password. As always, make sure to use a secure password for the database user. All of this is done from the command prompt of the MySQL server.

The commands you will need to use are as follows. An example of how this will look is also below. By convention, MySQL commands are always shown in UPPERCASE. However, you do not actually need to use uppercase letters, the commands are not case-sensitive. Also, all MySQL commands end with a semi-colon.

* **`mysql -u root -p`** - this logs you in to the MySQL database server as the MySQL root user. You will be prompted for the MySQL root password.
* **`CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin;`** - this creates the actual database, called confluence
* **`GRANT ALL PRIVILEGES ON confluence.* TO 'confluence_db_user'@'localhost' IDENTIFIED BY 'secure_password';`** - this creates the database user and sets the password for that user. I tend to use something similar to confluence_db_user or sometimes conf_db_user.

```sh
[root@example ~]# mysql -u root -p
Enter password:

Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 5.0.95 Source distribution

Copyright (c) 2000, 2011, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE confluence CHARACTER SET utf8 COLLATE utf8_bin;
mysql> GRANT ALL PRIVILEGES ON confluence.* TO 'confluence_db_user'@'localhost' IDENTIFIED BY 'secure_password';
mysql> quit;
[root@example ~]#
```

After you have created the database and database user / password, you will need to edit the file that controls some settings for MySQL. The file is located at */etc/my.cnf*. Note the dot between my and cnf.

```sh
[root@example ~]# cd /etc
[root@example ~]# vim my.cnf
```

The *my.cnf* file is divided into sections. Add the following to the `[mysqld]` section:

```
character-set-server=utf8
collation-server=utf8_bin
default-storage-engine=INNODB
max_allowed_packet=32M
```

Once you have made these changes, save and exit the file, and restart the MySQL database server with the service mysqld restart command.

```sh
[root@example ~]# service mysqld restart
```
This ends the command line install part of Confluence. The rest of the install process takes place in the browser.

### Move to the browser-based installer (this is where you install the license)

(My notes here aren't as detailed, because I've installed Confluence quite a few times and this part is pretty self-explanatory to me by now)

The last part of the Confluence install takes place in the browser. Open a browser, and go to ***http://IP_ADDRESS:8090***, where IP_ADDRESS is the IP address of your server. If the browser won't open the page, my experience is that the connection is being blocked by some kind of firewall application.

From the first page that opens, you will need to copy the Server ID, which is just above the box where you enter the License Key. Take the Server ID, and go to your **My Atlassian** account. There will be a place there to generate the License Key by entering the Server ID.

Here are the rest of the steps needed to finalize the installation:

1. Choose a **Production Installation** - this allows you to use the MySQL database
2. When prompted, choose MySQL in the **Choose Database** screen
3. You will need to choose the **JDBC** option to connect the database to Confluence
4. You will need to enter the Confluence database user and password when prompted

This takes a few minutes to run. If there is a problem, it will error out almost immediately.

This should complete the Confluence installation.

### Enjoy using Confluence

From this point onward you should be able to use Confluence.

If you need more in-depth information on how to use Confluence, there is one book I've heard good things about: [Atlassian Confluence 5 Essentials](https://www.packtpub.com/web-development/atlassian-confluence-5-essentials "Confluence Essentials").

The Atlassian Confluence docs are a bit hit and miss, and a bit disorganized (in my opinion). But they are the official source, and you should be able to find some answers there - [Confluence Documentation Home](https://confluence.atlassian.com/doc/confluence-documentation-home-135922.html "Confluence Documentation Home"). Make sure that the doc you're looking at matches the version of Confluence you have installed.

---

## Addendum - A Very Short Vim Tutorial 

Vim is the successor to the **vi** editor, created in 1976 by Bill Joy (co-founder of Sun Microsystems). Vi comes from a world where all text input and editing was done from a green-screen serial terminal, and the concept of a mouse and a GUI had not yet been invented. Vim (Vi IMproved) was created in 1991 by Bram Moolenaar for the Amiga system, and has become more or less the "official" replacement to Vi. 

Vim (and Vi) are modal editors, meaning that you enter commands in command mode, and enter text in insert mode. This can be quite confusing at first, and even after using Vim for almost 20 years I sometimes get tripped up by which mode I'm in. More than you ever wanted to know about Vim can be found here - [Vim User Manual](http://vimdoc.sourceforge.net/htmldoc/usr_toc.html "Vim User Manual"). 

However, you only need to use a very small set of commands in order to edit the files referenced in this install guide. Basically, you need to know how to:

1. Open a file for editing
2. Move to a location in the file
3. Add text to the file
4. Save and exit the file 

### 1. Open a file for editing 

To open a file for editing, use the **`vim name_of_file`** command from the command prompt. For example, to open the **`.my.cnf`** file: 

```sh
vim .my.cnf
```

### 2. Move to a location in the file

If the file you're editing has text in it already, you can navigate to the location where you need to enter your text using the arrow keys. However, remember that Vim is a modal editor, and you have to be in command mode to move around. So before you use the arrow keys, hit the **`Esc`** (Escape) key twice. The Escape key will always take you out of insert mode and back to command mode. After hitting the Escape key twice, hit the up/down/left/right arrow keys as needed to move you to your desired location. 

### 3. Add text to the file

Once the cursor is where you want it to be, you need to enter insert mode to add your text. Press the lower-case **i** key. This will allow you to type in the text you need to add. Using the Return or Enter key will create a new line if needed.  

**TIP** - if you make a mistake, hit the Escape key twice, and then use the lower-case **x** key to erase what you've typed, and then use the **i** key again to get back to insert mode. 

### 4. Save and exit the file 

Once you've added your text or made your changes to the file you can save and exit. Hit the Escape key twice to get back to command mode, and then type in **`:wq`**. That's a `colon w q` all together with no spaces. This will save and exit the file, and get you back to the command prompt. 

<br />
If you're interested in learning how to use Vim, there is a built-in tutorial called **`vimtutor`** that you can access from the command line: 

```sh
[root@example ~]# vimtutor
```
If you work with Linux or UNIX systems on a regular basis, knowing Vim can be a huge benefit. Even just knowing some of the basics will make your life much easier. 





