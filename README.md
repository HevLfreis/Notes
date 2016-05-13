# Server configration

***

## Enviroments

1. MySQL
    ```
    sudo apt-get install mysql-server mysql-client libmysqlclient-dev
    
    service mysql start
    sudo netstat -tap | grep mysql
    mysql -uroot -p
    ```
2. Apache
    ```
    sudo apt-get install apache2
    
    # python mod-wsgi
    sudo apt-get install libapache2-mod-wsgi
    sudo vim /etc/apache2/sites-available/site.conf
    ```
    ```apacheconf
    ## Apache django
    <VirtualHost *:80>
        ServerName www.domain.com
        ServerAlias domain.com
        ServerAdmin hevlhayt@foxmail.com
    
        Alias /static/ /home/user/Project/static/
    
        <Directory /home/user/Project/static>
            Require all granted
        </Directory>
    
        WSGIScriptAlias / /home/user/Project/APP/wsgi.py
    
        <Directory /home/user/Project/APP>
        <Files wsgi.py>
            Require all granted
        </Files>
        </Directory>
    </VirtualHost>
    ```
    ```
    sudo a2ensite site
    service apache2 reload
    cat /var/log/apache2/error.log
    ```
3. python
    ```
    sudo apt-get install python-pip
    sudo apt-get install python-dev
    pip install django
    pip install MySQL-python
    pip install sqlalchemy
    ```
    
4. django
    ```
    # sync django
    python manage.py makemigrations
    python manage.py migrate
    ```
    ``` python
    # wsgi.py
    import os
    from os.path import dirname, abspath
    
    PROJECT_DIR = dirname(dirname(abspath(__file__)))
    
    import sys
    sys.path.insert(0, PROJECT_DIR)
    
    os.environ["DJANGO_SETTINGS_MODULE"] = "APP.settings"
    
    from django.core.wsgi import get_wsgi_application
    application = get_wsgi_application()
    ```
    
    ```
    # user upload path
    chown -R www-data:www-data /upload
    sudo chmod -R g+w /upload
    ```
    
> If you have any problem, please contact hevlhayt@foxmail.com (ﾉﾟ▽ﾟ)ﾉ