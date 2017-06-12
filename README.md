# Wordpress on Heroku install and configuration using:

[![WordPress](http://technomile.github.io/img/cms_buildpack_github.png)](http://www.technomile.com)
## Heroku Buildpack for WordPress

# Installation notes
## local installation
Here are the geberal steps we are going to take
1. clone the buildpack somewhere in home folder
2. install the local Stack and Dependencies 
	* (we're using LAMP on ubuntu here)
3. configure the local environment
	* git
	* apache
	* database
	* file system
	* wp-config
4. Upgrade and Configure core, plugins, themes...
5. Done. Move on. Remote Installation. 

Clone technomile's Heroku Buildpack:
```bash
git clone https://github.com/technomile/Heroku-WordPress.git wordpress
```
Install local stack:
```bash
sudo apt install git apache2 mysql-server php libapache2-mod-php php-mcrypt php-mysql php-cli php-curl php-gd php-mbstring php-xml php-xmlrpc
```
Configure git:
```bash
nano $GIT_DIR/info/exclude
```
add this line to the file:
```
wp-config*
```
then make sure git excludes any current changes:
```bash
git update-index --assume-unchanged [<file>...]
```
## remote installation
Create a heroku project and add necessary addons:
```bash
cd wordpress
heroku create
heroku addons:add clear:dbignite
heroku addons:add newrelic:wayne
heroku addons:add sendgrid:starter
```
Push the project to heroku:
```bash
git push heroku master
```


Check out more [documentation](http://technomile.github.io/wordpress/) to learn more about Heroku & WordPress and to set up your own instance.
