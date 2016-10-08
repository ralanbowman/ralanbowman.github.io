---
title: Sphinx Install Guide
sidebar: second_sidebar
toc: false
permalink: sphinx_install.html
---

At a previous employer I was asked to find and evaluate several solutions to replace our existing knowledge base platform, which was basically an afterthought tacked on to the ticket system we used for support requests. 

One of the solutions I looked at was [Sphinx](http://www.sphinx-doc.org/en/stable/index.html "Sphinx Documentation Generator"), a documentation generator written in Python. I found it very poorly documented and difficult to install, and so moved on to other potential solutions. However, I really wanted to take a closer look at Sphinx, so after working through the instructions found from various sources online, I was able to get a working install. Several people on a technical writer mailing list I’m on asked about the instructions, so I created an install guide they could use themselves to get started. 

**Note** - this was written using a less formal tone than a conventional user guide or install guide, because I was writing for people of a similar skill level and who wanted a quick overview, not a detailed reference guide. 

---

# Installing Sphinx On a CentOS 6 Virtual Server

### Before you begin

These install notes assume the following:

* You have access to a Linux server (either a physical server or a virtual server) and can work from the command line as the root user
* You are able to navigate the Linux file system, install software from the command line, edit files with a text editor, and run shell commands
* There is a non-root user associated with the domain where you will be serving your documentation from, and you are able to work as that user (domains are associated with a non-root user for security reasons)

These notes are specific to CentOS 6 and the Apache web server. If you have a different distro and web server, you will need to adapt these notes to your situation.

This is my current hardware setup:

* OS - CentOS release 6.6 (Final)
* RAM - 512 MB
* HDD - 8 GB disk / 1 GB swap

Web server setup:

* Apache web server
* Domain of *docs.example.com* in DNS with the A record pointing to the public IP of the server
* DocumentRoot for the website set to *$DEFAULT_WEBSERVER_PATH/docs.example.com/build/html*


[Installing Sphinx](#installing-sphinx)  
[Sphinx Startup and Configuration](#sphinx-startup-and-configuration)  
[My Current Workflow](#my-current-workflow)  

---

## Installing Sphinx

Sphinx requires Python 2.6 up to Python 3.3. Most Linux distros should have Python installed as part of the base OS.

You will need to install several applications before installing Sphinx:

* CentOS Development tools
* Python Setuptools (this installs the Python *easy_install* application)
* Docutils (installed with *easy_install*)
* Jinja2 (installed with *easy_install*)
* Pygments (installed with *easy_install*)
* pip (used to install Sphinx)

Once these applications are installed, you can then install Sphinx.

### CentOS Development tools

The CentOS Development tools are a set of applications used to install, manage, and develop software.  They are usually necessary for installing software that did not come with the base OS install.

The Development tools are installed as the root user with *yum*:

```sh
yum groupinstall "Development tools"
```

### Python Setuptools

The Python Setuptools are a group of Python tools used to install Python modules and libraries. In this case, the Python Setuptools are used to install the *easy_install* package, which will then be used to install several other applications needed by Sphinx.

The Python Setuptools are downloaded as the root user with *wget*, and then processed through Python:

```sh
wget https://bootstrap.pypa.io/ez_setup.py -O - | python
```

### Docutils

The Docutils are used by Sphinx to process and format text.  

The Docutils are installed as the root user using *easy_install*, which was installed as part of the Python Setuptools:

```sh
easy_install docutils
```

### Jinja2

Jinja2 is used as part of the Sphinx template engine.  

Jinja2 is installed as the root user using *easy_install*:

```sh
easy_install Jinja2
```

### Pygments

Pygments is a syntax highlighting library for Python, used by Sphinx to show colored syntax in source code blocks.  

Pygments is installed as the root user using *easy_install*:

```sh
easy_install pygments
```

### pip

*pip* is another tool used to download and install Python packages and modules. *pip* has to be downloaded first, and then installed.

Download *pip* as the root user with the *wget* command:

```sh
wget https://bootstrap.pypa.io/get-pip.py
```

Install *pip* as the root user with Python:

```sh
python get-pip.py
```


### Installing Sphinx

Installing Sphinx is fairly straight forward. Assuming you have installed all the software listed above, do the following as the root user:

```sh
pip install Sphinx
```

This will scroll some output on your screen, and eventually drop you back to the command prompt. Assuming there are no errors, you can now start Sphinx and begin the setup process.

---

## Sphinx Startup and Configuration

From this point forward you will need to work as the non-root user who owns the domain and web server files. Generally you will switch from the root user to the non-root user with the **`su - non_root_user`** command:

```sh
su - non_root_user
```

The **`-`** switch tells the system that you’re switching users (**`su`**) and that you’re using the `$PATH` and local
directories for that user.

Change directories to the location where you want to serve your documentation from. In this case, I changed to the default DocumentRoot for the domain, */var/www/non_root_user/data/www/docs.example.com* - your location will certainly be different. 

After the install was complete, I changed the DocumentRoot for the website in the Apache configuration to point to the directory where Sphinx will put the docs, but that is not required.

### Sphinx Quickstart

Sphinx comes with a utility to get your configuration in place, called *sphinx-quickstart*. Run this as the non-root user:

```sh
sphinx-quickstart
```

The *sphinx-quickstart* utility will run through the configuration options. Generally, you can accept the defaults, but here are the options I set manually:

* Separated source and build directories - say Yes here (default is No)
* Autodoc - say Yes here (default is no)
* Create make scripts - I forget the exact wording, but you are asked to create a script and a Windows
bat file that let you generate your documentation from the source directory into the build directory.
Say Yes here, it will make your life easier.

Once the *sphinx-quickstart* script completes, you will have two new directories: *source* and *build*. All the files you create are in *source*, and all the files being served by Sphinx are in *build*.

At this point you can work from the [official Sphinx docs](http://www.sphinx-doc.org/en/stable/intro.html "Sphinx Intro Docs") to continue your setup and start creating docs and working with Sphinx.

---

## My Current Workflow

My current workflow for Sphinx looks like this:

1. Edit the files in the *source* directory - these are *.rst* files (reStructuredText) files.
2. From the Sphinx install directory, run the **make html** command - this runs an html processor on the *.rst* files in *source*, and creates the output in the *build* directory.  For example:

```sh
-bash-4.1$ pwd
/var/www/docs_user/data/www/docs.eapps-example.com
-bash-4.1$ vim source/SphinxInstall.rst
-bash-4.1$ make html
```

