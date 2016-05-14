# Server configration

***

## Enviroments

1. MySQL
    ```
    sudo apt-get install mysql-server mysql-client libmysqlclient-dev
    
    service mysql start
    sudo netstat -tap | grep mysql
    mysql -uroot -p
	
	sudo vim /etc/mysql/my.cnf
	
	# switch off case sensitive for importing from win
	[mysqld]
	lower_case_table_names = 1
    ```
2. Apache
    ```
    sudo apt-get install apache2
    
    # python mod-wsgi
    sudo apt-get install libapache2-mod-wsgi
    sudo vim /etc/apache2/sites-available/site.conf
    ```
    ```apacheconf
    # apache django
    <VirtualHost *:80>
        ServerName app.domain.com
        # ServerAlias domain.com
        ServerAdmin hevlhayt@foxmail.com
    
        Alias /static/ /home/user/project/static/
    
        <Directory /home/user/project/static>
            Require all granted
        </Directory>
		
		WSGIDaemonProcess app python-path=/home/user/projects/APP:/home/user/projects/APP/venv/lib/python2.7/site-packages processes=2 threads=15 display-name=%{GLOBAL}
		WSGIProcessGroup app
        WSGIScriptAlias / /home/user/projects/APP/wsgi.py
    
        <Directory /home/user/projects/APP>
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
    # user uploading path
    chown -R www-data:www-data /upload
    sudo chmod -R g+w /upload
    ```
    
> If you have any problem, please contact hevlhayt@foxmail.com (ﾉﾟ▽ﾟ)ﾉ