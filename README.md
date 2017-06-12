# Wordpress on Heroku install and configuration using:

[![WordPress](http://technomile.github.io/img/cms_buildpack_github.png)](http://www.technomile.com)
## Heroku Buildpack for WordPress

# Installation notes
## local installation
Clone technomile's Heroku Buildpack:
'''bash
git clone https://github.com/technomile/Heroku-WordPress.git wordpress
'''

## remote installation
Create a heroku project and add necessary addons:
'''bash
cd wordpress
heroku create
heroku addons:add clear:dbignite
heroku addons:add newrelic:wayne
heroku addons:add sendgrid:starter
'''
Push the project to heroku:
'''bash
git push heroku master
'''


Check out more [documentation](http://technomile.github.io/wordpress/) to learn more about Heroku & WordPress and to set up your own instance.
