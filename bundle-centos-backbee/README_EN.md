# 5 Minutes Stacks, episode 54: BackBee - English version

## Episode 54 : BackBee

BackBee CMS is an open source Content Management System (CMS) with one major advantage. BackBee CMS uses On-page Editing Technology (OPE Technology) which means you can easily create and manage websites as they appear with no prior technical knowledge.
                                      

On-page Editing Technology  is the natural evolution of your usual Content Management System functionalities. On-page Editing Technology is a tool that allows you to enter, edit and manage your website directly as it appears to your users: The back office and front office are merged.

Today, Cloudwatt provides the necessary toolset to start your BackBee instance in a few minutes and to become its master.

The deployement base is an CentOS 7 instance. The Apache and MariaDB servers are deployed on a single instance.

## Preparations

### The versions

* Centos 7 
* Apache 2.4
* BackBee 1.2
* MariaDB 5.5
* PHP 5.4

## Install One-Click

Go to the Apps page on the Cloudwatt website, choose the apps, press DEPLOY and follow the simple steps... 2 minutes later, a green button appears... ACCESS: you have your owncloud server!

You will not be able to finish the installation of this one, you will find more information on [this link](# install)

## Install Console

Using the console, you can deploy a BackBee server:

1.	Go the Cloudwatt Github in the applications/bundle-centos-backbee repository
2.	Click on the file nammed bundle-centos-backbee.heat.yml
3.	Click on RAW, a web page appear with the script details
4.	Save as its content on your PC. You can use the default name proposed by your browser (just remove the .txt)
5.  Go to the « [Stacks](https://console.cloudwatt.com/project/stacks/) » section of the console
6.	Click on « Launch stack », then click on « Template file » and select the file you've just saved on your PC, then click on « NEXT »
7.	Named your stack in the « Stack name » field
8.	Enter your keypair in the « keypair_name » field
9.	Choose the instance size using the « flavor_name » popup menu and click on « LAUNCH »

The stack will be automatically created (you can see its progress by clicking on its name). When all its modules will become "green", the creation will be completed. Then you can go on the "Instances" menu to discover the flotting IP value that has been automatically generated. Now, just run this IP adress in your browser and enjoy !

It is (already) FINISH !

## Install cli

If you like only the command line, you can go directly to the "CLI launch" version by clicking [this link](# cli)


## For further
<a name="install" />

### Config database
![config bdd](https://raw.githubusercontent.com/flemzord/applications/master/bundle-centos-backbee/images/installation.png?raw=true)

### Homepage
![Homepage](https://raw.githubusercontent.com/flemzord/applications/master/bundle-centos-backbee/images/homepage.png?raw=true)

### Homepage + BackOffice
![Homepage & Backoffice](https://raw.githubusercontent.com/flemzord/applications/master/bundle-centos-backbee/images/homepage_back.png?raw=true)

### Configuration of the database

The configuration of the database is quite simple.

You must enter the following information:

- Server: 127.0.0.1 or localhost
- Port: 3306
- Database name: backbee
- Database username: backbee

You will find the password of your user MariaDB at this address: `http: // IP / password.txt`.

And here you can continue to configure BackBee.

## So watt ?

The goal of this tutorial is to accelarate your start. At this point you are the master of the stack.
You have a SSH access point on your virtual machine thru the flotting IP and your private keypair (default user name `cloud`).

The interesting entry access points are:

- `/home/www` : Backbee installation repository
- [Website](http://developers.backbee.com)

<a name="cli" />

## Install cli

### The prerequisites to deploy this stack

* an internet acces
* a Linux shell
* a [Cloudwatt account](https://www.cloudwatt.com/cockpit/#/create-contact), with an [existing keypair](https://console.cloudwatt.com/project/access_and_security/?tab=access_security_tabs__keypairs_tab)
* the tools [OpenStack CLI](http://docs.openstack.org/cli-reference/content/install_clients.html)
* a local clone of the git repository [Cloudwatt applications](https://github.com/cloudwatt/applications)

### Size of the instance

Per default, the script is proposing a deployement on an instance type "Small" (s1.cw.small-1).  Instances are charged by the minute and capped at their monthly price (you can find more details on the [Tarifs page](https://www.cloudwatt.com/fr/produits/tarifs.html) on the Cloudwatt website). Obviously, you can adjust the stack parameters, particularly its defaut size.

### By the way...

If you do not like command lines, you can go directly to the "run it thru the console" section by clicking [here](#console) 

## What will you find in the repository

Once you have cloned the github, you will find in the  `bundle-centos-backbee/` repository:

* `bundle-centos-backbee.heat.yml` : HEAT orchestration template. It will be use to deploy the necessary infrastructure.
* `stack-start.sh` : Stack launching script. This is a small script that will save you some copy-paste.
* `stack-get-url.sh` : Flotting IP recovery script.

## Start-up

### Initialize the environment

Have your Cloudwatt credentials in hand and click [HERE](https://console.cloudwatt.com/project/access_and_security/api_access/openrc/). 
If you are not logged in yet, you will go thru the authentication screen then the script download will start. Thanks to it, you will be able to initiate the shell acccesses towards the Cloudwatt APIs.

Source the downloaded file in your shell. Your password will be requested. 

~~~ bash
$ source COMPUTE-[...]-openrc.sh
Please enter your OpenStack Password:

~~~ 

Once this done, the Openstack command line tools can interact with your Cloudwatt user account.

### Adjust the parameters

With the `bundle-centos-backbee.heat.yml` file, you will find at the top a section named `parameters`. The sole mandatory parameter to adjust is the one called `keypair_name`. Its `default` value must contain a valid keypair with regards to your Cloudwatt user account. This is within this same file that you can adjust the instance size by playing with the `flavor` parameter.

~~~ yaml
heat_template_version: 2013-05-23


description: All-in-one BackBee stack


parameters:
  keypair_name:
    default: amaury-ext-compute         <-- Indicate here your keypair
    description: Keypair to inject in instances
    type: string

  flavor_name:
      default: s1.cw.small-1              <-- Indicate here the flavor size
      description: Flavor to use for the deployed instance
      type: string
      constraints:
        - allowed_values:
            - s1.cw.small-1
            - n1.cw.standard-1
            - n1.cw.standard-2
            - n1.cw.standard-4
            - n1.cw.standard-8
            - n1.cw.standard-12
            - n1.cw.standard-16
            
[...]
~~~ 

### Start up the stack

In a shell, run the script `stack-start.sh` with the name you want to give it as parameter:

~~~ bash
$ ./stack-start.sh LE_BIDULE
+--------------------------------------+------------+--------------------+----------------------+
| id                                   | stack_name | stack_status       | creation_time        |
+--------------------------------------+------------+--------------------+----------------------+
| ed4ac18a-4415-467e-928c-1bef193e4f38 | LE_BIDULE  | CREATE_IN_PROGRESS | 2015-04-21T08:29:45Z |
+--------------------------------------+------------+--------------------+----------------------+
~~~ 

Last, wait 5 minutes until the deployement been completed.

At each new deployement of the stack, a mySQL password is generated,  available during installation at http: // IP / password.txt`.
Once the CMS has installed it will be removed.

### Enjoy

Once all of this done, you can run the `stack-get-url.sh` script. 

~~~ bash
./stack-get-url.sh THE_THING
THE_THING 82.40.34.249
~~~ 

It will gather the assigned flotting IP of your stack. You can then paste this IP in your favorite browser and start to configure your BackBee instance.

## In the background

The  `start-stack.sh` script is taking care of running the API necessary requests to: 

* start an Centos 7 based instance
* do an update of the system packages
* install Apache, PHP, MariaDB and BackBee
* configure MariaDB with a BackBee dedicated user and database, with a generated password
* show a flotting IP on the internet

-----
Have fun. Hack in peace.
