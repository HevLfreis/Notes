# Server configration

***

## Enviroments
1. ### linux
	```
	ps -aux
	sudo netstat -tap
	kill -9 $(ps -e | grep xxx | awk '{print $1}')
	sudo apt-get --purge autoremove xxx
	
	# screen
	sudo apt-get install screen
	screen -S name
	screen -ls
	screen -r id/name
	screen -X -S name quit
	```
	
2. ### mysql
    ```
    sudo apt-get install mysql-server mysql-client libmysqlclient-dev
    
    service mysql start
    sudo netstat -tap | grep mysql
    mysql -u root -p
	
	mysqldump -uroot -p database > dump.sql
	mysql -uroot -p database < dump.sql
	
	sudo vim /etc/mysql/my.cnf
	
	# switch off case sensitive for importing from win
	[mysqld]
	lower_case_table_names = 1
	
	# set timeout to avoid mysql has gone away in sqlalchemy 
	interactive_timeout = 3600
	wait_timeout = 3600
	
	# flask-sqlalchemy
	# the pool_recycle should be less than timeout 
	app.config['SQLALCHEMY_POOL_RECYCLE'] = 1800
    ```
	
3. ### mongodb
	```
	sudo apt-get install mongodb
	mkdir -p /data/db
	sudo chown -R mongodb:mongodb /data
	mongo
	```
4. ### apache
    ```
    sudo apt-get install apache2
    
    # python mod-wsgi
    sudo apt-get install libapache2-mod-wsgi
    sudo vim /etc/apache2/sites-available/site.conf
    ```
    ```apacheconf
    # apache django
	# mod_wsgi: http://modwsgi.readthedocs.io/en/develop/index.html
    <VirtualHost *:80>
        ServerName djangoapp.domain.com
        # ServerAlias domain.com
        ServerAdmin hevlhayt@foxmail.com
    
        Alias /static/ /home/user/projects/DjangoApp/static/
    
        <Directory /home/user/projects/DjangoApp/static>
            Require all granted
        </Directory>
		
		WSGIDaemonProcess djangoapp python-path=/home/user/projects/DjangoApp:/home/user/projects/DjangoApp/venv/lib/python2.7/site-packages processes=2 threads=15 display-name=%{GROUP}
		WSGIProcessGroup djangoapp
        WSGIScriptAlias / /home/user/projects/DjangoApp/DjangoApp/wsgi.py
		
		# solving the problem of python packages using native c like scipy
		WSGIApplicationGroup %{GLOBAL}
    
        <Directory /home/user/projects/DjangoApp/DjangoApp>
        <Files wsgi.py>
            Require all granted
        </Files>
        </Directory>
    </VirtualHost>
	
	# apache flask
	<VirtualHost *:80>
		ServerName flaskapp.domain.com

		WSGIDaemonProcess flaskapp python-path=/home/user/projects/FlaskApp:/home/user/projects/FlaskApp/venv/lib/python2.7/site-packages processes=2 threads=15 display-name=%{GROUP}
		WSGIProcessGroup flaskapp
		WSGIScriptAlias / /home/user/projects/FlaskApp/app.wsgi
		WSGIApplicationGroup %{GLOBAL}
		
		<Directory /home/user/projects/FlaskApp>
			Require all granted
		</Directory>
	</VirtualHost>

    ```
    ```
    sudo a2ensite site
    service apache2 reload
    cat /var/log/apache2/error.log
    ```
	
5. ### nginx
	```
	waiting...
	```
	
6. ### python
    ```
    sudo apt-get install python-pip
    sudo apt-get install python-dev
	
	# hdf5 file system
	sudo apt-get install libhdf5-dev
	```
	```
	# config pip in aliyun
	cd ~/.pip
	vim pip.conf
	
	# pip.conf
	[global]
	index-url = http://mirrors.aliyun.com/pypi/simple/

	[install]
	trusted-host=mirrors.aliyun.com
	```
	```	
	sudo pip install MySQL-python
	or ?
	sudo pip install mysqlclient  
	
    sudo pip install sqlalchemy
	
	sudo pip install virtualenv
	cd project
	virtualenv venv
	
	# active virtual env
	# in ubuntu, you should add dist-package as PYTHONPATH to .bashrc
	source bin/activate
	pip something in this venv
	deactivate
	
	# install glib
	sudo apt-get install libglib2.0-dev
	sudo apt-get install libffi-dev
	
	# python markdown
	sudo pip install misaka
    ```
    
7. ### django
    ```
	sudo pip install django
	
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
    
    os.environ["DJANGO_SETTINGS_MODULE"] = "DjangoApp.settings"
    
    from django.core.wsgi import get_wsgi_application
    application = get_wsgi_application()
	
	# settings.py for deployment
	DEBUG = False
	ALLOWED_HOSTS = ['*']
	
	# if you have set up the venv for this django, just make sure that you have set the venv path correctly in apache's conf. 
	# The django will automatically use the project's venv python path.
    ```
    
    ```
    # user uploading path
    sudo chown -R www-data:www-data /upload
    sudo chmod -R g+w /upload
    ```
	
8. ### flask
	```
	sudo pip install Flask
	```
	``` python
	# app.wsgi
	
	# you can ignore the two lines below when you make sure that you have set the venv path correctly in apache's conf.
	# activate_this = '/home/user/projects/FlaskApp/venv/bin/activate_this.py'
	# execfile(activate_this, dict(__file__=activate_this))

	from yourapp import app as application

	import sys
	sys.path.insert(0, '/home/user/projects/FlaskApp')
	```
	
9. ### git
	```
	git commit -m 'update sth'  
	git reset --soft <sha>
	git push --force
	git rebase -i <sha>  
	
	git rm -r --cached .
	git add .
	git commit -m ".gitignore update"
	```
	
10. ### https
	```
	https://startssl.com/  > 1_root_bundle.crt, 2_your_domain.crt
	```
	```
	# on server 
	openssl req -newkey rsa:2048 -keyout server.key -out server.csr
	a2enmod ssl
	service apache2 reload
	
	netstat -tnap  // 443 listening
	
	// add to site conf, with port 80
	<VirtualHost *:443>
        ServerName djangoapp.domain.com
        # ServerAlias domain.com
        ServerAdmin hevlhayt@foxmail.com
    
        Alias /static/ /home/user/projects/DjangoApp/static/
    
        <Directory /home/user/projects/DjangoApp/static>
            Require all granted
        </Directory>
				
		# no daemon settings
		WSGIProcessGroup djangoapp
        WSGIScriptAlias / /home/user/projects/DjangoApp/DjangoApp/wsgi.py
		WSGIApplicationGroup %{GLOBAL}
    
        <Directory /home/user/projects/DjangoApp/DjangoApp>
        <Files wsgi.py>
            Require all granted
        </Files>
        </Directory>
		SSLEngine On
		SSLCertificateFile "/etc/apache2/ssl/2_your_domain.crt"
		SSLCertificateKeyFile "/etc/apache2/ssl/server.key"
		SSLCertificateChainFile "/etc/apache2/ssl/1_root_bundle.crt"
    </VirtualHost>
	```
	```python
	# django settings, or using apache to do the https redirect
	SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
	SECURE_SSL_REDIRECT = True
	SESSION_COOKIE_SECURE = True
	```

11. ### torch and torch-rnn
	```
	# python env
	sudo apt-get install software-properties-common
	sudo apt-get install python-software-properties
	sudo apt-get install ipython
	sudo pip install Cython
	
	# pip failed ?
	sudo pip install h5py
	sudo apt-get install python-h5py
	```
	```
	# torch
	sudo apt-get install git
	git clone https://github.com/torch/distro.git torch --recursive
	bash install-deps
	./install.sh
	source ~/.bashrc

	th
	```
	```
	# torch-hdf5
	git clone https://github.com/deepmind/torch-hdf5
	sudo apt-get install totem
	
	cd torch-hdf5
	luarocks make hdf5-0-0.rockspec
	```
	```
	# torch-rnn
	git clone https://github.com/jcjohnson/torch-rnn
	
	python scripts/preprocess.py --input_txt my_data.txt --output_h5 my_data.h5 --output_json my_data.json
	
	th train.lua -input_h5 my_data.h5 -input_json my_data.json -gpu -1
	th sample.lua -checkpoint cv/checkpoint_10000.t7 -length 2000 -gpu -1
	```
	
13. ### tensorflow
	```
	waiting...
	```
	
14. ### mxnet
	```
	waiting...
	```
	

> If you have any problem, please contact hevlhayt@foxmail.com (ﾉﾟ▽ﾟ)ﾉ