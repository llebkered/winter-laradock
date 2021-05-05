# Install WinterCMS using Laradock

Notes and files for YouTube tutorial
 
https://youtu.be/upee5wu2pMY


## Installing WinterCMS

Winter can be easily installed using the Wizard Installer or you can use command line. Command line is the recommended way if you are a developer

Link to a simpler wizard install
https://wintercms.com/docs/setup/installation#wizard-installation

---

## Command Line Installation

We'll use composer to install because it will make life easier later of when we need to update the system.

Start off by letting Composer do its thing.

`composer create-project wintercms/winter winter-laradock`

This may take a few minutes depeding on the speed of your computer.

**Important**: Once this task has finished, open the file **config/cms.php** and enable the _disableCoreUpdates_ setting. This will disable core updates from being delivered by the October gateway.

`'disableCoreUpdates' => true,`



## Laradock

You can create your own Docker files to match the deployment server but I find Laradock an easy way to get started.

First step is to download Laradock. Open a terminal an open your project.

`git clone https://github.com/Laradock/laradock.git`

This will copy the Laradock files into a folder called _laradock_

`cd laradock`

Next create an _.env_ file by copying the _env-example_ file

`cp env-example .env`

Next up. Make some minor edits to the _.env_ file. I will call my database _acme_ Somewhere around line 334 in the .env file, you should see the following block of options:

```
### MYSQL #################################################

MYSQL_VERSION=latest
MYSQL_DATABASE=default
MYSQL_USER=default
MYSQL_PASSWORD=secret
MYSQL_PORT=3306
MYSQL_ROOT_PASSWORD=root
MYSQL_ENTRYPOINT_INITDB=./mysql/docker-entrypoint-initdb.d
```

Change `MYSQL_DATABASE=default` to `MYSQL_DATABASE=acme`

Let's start up the docker containers. I'll use Apache and MySQL for this. You can use whatever works best for you.

`docker-compose up -d apache2 mysql phpmyadmin workspace`

This will bring up the

- Apache web server,
- a MySQL database,
- an instance of PHPmyAdmin to administer the database,
- and Workspace to run PHP commands.

You should see the WinterCMS demonstration page at http://localhost. PHPmyAdmin can be found at http://localhost:8081

[http://laradock.io/](http://laradock.io/)



## Finish Winter Installation

If we want to take things further, then we need to install WinterCMS. This will give us access to a backend control panel.

Users of Laravel will be familiar with artisan commands. Winter has some commands for installation, updates and scaffolding.

Let's install.

`php artisan Winter:install`

This will present a series of options.

**Database Type:** 0 (for MySQL)

**MySQL Host:** _mysql_ (This the the MySQL container name. WAMP, XAMP etc will probably be localhost or 127.0.0.1)

**MySQL Port**: (This should default to port 3306)

**Database Name**: _acme_ (We set this in the Laradock .env)

**MySql Login**: _root_ (We set this in the Laradock .env)

**MySql Password**: _root_ (We set this in the Laradock .env)

**First Name**: Your First Name

**Second Name**: Your Second Name

**Email Address**: Your email address

**Admin Login**: username to login to backend with

**Admin Password**: password to log into backend with

**Is this information correct**: _Y_

Some October installations generate a random password. If this happens, be sure to copy it and store in safe place.

You also may wish to check _config/app.php_ and _config/cms.php_ to change any additional configuration options.

