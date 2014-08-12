Selenium 2.X init script (Hub+Node)
===================================

Sample debian-style init script for Selenium 2.

This init script assumes you have Selenium JAR in `/usr/share/selenium/`
You must also have the `daemon` package installed and JRE/JDK installed.

Tested on Ubuntu LTS 14.04 64bit with ORACLE JDK 1.7


Daemon configuration is under `/etc/default/selenium-hub|node` and `/etc/selenium/`
This is Selenium 2.x style config, so json files are expected. I've included the default ones.


Instructions:
-------------

Install required packages:

	
	sudo apt-get install daemon default-jre-headless
	

Initialize users and dirs:

	
	sudo groupadd -r selenium-hub
	sudo groupadd -r selenium-node
	
	sudo useradd -r -d /var/lib/selenium-hub  -s /bin/bash -m -g selenium-hub  -G selenium-hub  selenium-hub
	sudo useradd -r -d /var/lib/selenium-node -s /bin/bash -m -g selenium-node -G selenium-node selenium-node
	
	sudo mkdir /var/log/selenium-hub/
	sudo mkdir /var/log/selenium-node/
	sudo mkdir /usr/share/selenium/
	
	sudo chown selenium-hub.selenium-hub /var/log/selenium-hub
	sudo chown selenium-node.selenium-node /var/log/selenium-node
	

Download Selenium 2.X JAR and create a symlink:

	
    sudo wget --no-check-certificate http://selenium-release.storage.googleapis.com/2.42/selenium-server-standalone-2.42.2.jar -O /usr/share/selenium/selenium-server-standalone-2.42.2.jar
	ln -s /usr/share/selenium/selenium-server-standalone-2.42.2.jar /usr/share/selenium/selenium-server-standalone.jar
	

Git clone this repository (optional):

	
		git clone git@github.com:davidedg/selenium2-initscript.git
	or
		git clone https://github.com/davidedg/selenium2-initscript.git
		

Copy default configs and init scripts from repo:

	
	sudo cp -a selenium2-initscript/etc/init.d/* /etc/init.d/
	sudo chmod +x /etc/init.d/selenium-*
	sudo cp -a selenium2-initscript/etc/default/* /etc/default/
	sudo cp -a selenium2-initscript/etc/selenium /etc/
	sudo chown selenium-node.selenium-node /etc/selenium/*
	sudo chown selenium-hub.selenium-hub /etc/selenium/DefaultHub.json
	

Install services on boot:

	
    sudo update-rc.d selenium-hub defaults
    sudo update-rc.d selenium-node defaults
	

Start selenium-hub and selenium-node:

	
    sudo /etc/init.d/selenium-hub start
    sudo /etc/init.d/selenium-node start
	

Check the logs:

	
    sudo tail -f /var/log/selenium-hub/selenium-hub.log
    sudo tail -f /var/log/selenium-node/selenium-node.log
	
