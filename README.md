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

### Clone technomile's Heroku Buildpack:
```bash
git clone https://github.com/technomile/Heroku-WordPress.git wordpress
```
### Install local stack:
```bash
sudo apt install git apache2 mysql-server php libapache2-mod-php php-mcrypt php-mysql php-cli php-curl php-gd php-mbstring php-xml php-xmlrpc
```
### Configure git:
exclude local only files from tracking:
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
### Configure apache:
Enable .htaccess Overrides:
```bash
sudo nano /etc/apache2/apache2.conf
```
set AllowOveride within the Directory block:
```
...
<Directory /var/www/html/>
	AllowOverride All
</Directory>
...
```
Enable rewrite module:
```bash
sudo a2enmod rewrite
```
restart Apache to implement changes:
```bash
service apache2 restart
```
### Configure mySQL database:
log into mySQL with admin priv:
```bash
mysql -u root -p
```
create wordpress db:
```SQL
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```
create a wordpress user
```SQL
GRANT ALL ON wordpress.* TO 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
```
update the db with the changes:
```SQL
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```
exit mySQL
```SQL
EXIT;
```
### Configure the filesystem
make a symlink in /var/www/html:
```bash
sudo ln -s $PROJECT_DIR /var/www/html/$PROJECT_DIR
```
set owner:group of local web files:
```bash
sudo chown -r $YOUR_USER_NAME:www-data /var/www/html
```
give group write access to wp-content dir:
```bash
sudo chmod g+w /var/www/html/wp-content
```
Do the same for these directories:
```bash
sudo chmod -R g+w /var/www/html/wp-content/themes
sudo chmod -R g+w /var/www/html/wp-content/plugins
```
### Configure local wp-config.php
resotre wp-config-sample to wp-config
```bash
mv wp-config.php wp-config.php.bak
cp wp-config-sample.phop wp-config.php
```
get secure values from the WordPress API (copy the output):
```bash
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```
edit wp-config.php:
```
...

define('AUTH_KEY',         'VALUES COPIED FROM THE COMMAND LINE');
define('SECURE_AUTH_KEY',  'VALUES COPIED FROM THE COMMAND LINE');
define('LOGGED_IN_KEY',    'VALUES COPIED FROM THE COMMAND LINE');
define('NONCE_KEY',        'VALUES COPIED FROM THE COMMAND LINE');
define('AUTH_SALT',        'VALUES COPIED FROM THE COMMAND LINE');
define('SECURE_AUTH_SALT', 'VALUES COPIED FROM THE COMMAND LINE');
define('LOGGED_IN_SALT',   'VALUES COPIED FROM THE COMMAND LINE');
define('NONCE_SALT',       'VALUES COPIED FROM THE COMMAND LINE');

...
...

define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpressuser');

/** MySQL database password */
define('DB_PASSWORD', 'password');

...
```
At the end of wp-config.php:
```
...
define('FS_METHOD','direct');
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
