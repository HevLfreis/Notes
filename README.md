# Server Notes

***

## Linux
1. ### shell
	```
	ps -aux
	netstat -tap
	kill -9 $(ps -e | grep xxx | awk '{print $1}')
	sudo apt-get --purge autoremove xxx
	tar cvzf file.tar.gz /dir
	tar -xvf file.tar.gz -C /dir
	visudo
	
	# screen
	sudo apt-get install screen
	screen -S name
	screen -ls
	screen -r id/name
	screen -X -S name quit
	```
	
2. ### git
	```
	sudo apt-get install git
	
	git commit -a -m 'update sth'  
	git reset --soft <sha>
	git push --force
	git rebase -i <sha>  
	
	git rm -r --cached .
	git add .
	git commit -m ".gitignore update"
	```
	
***

## Database
1. ### mysql
    ```
	# install
    sudo apt-get install mysql-server mysql-client libmysqlclient-dev
    
    service mysql start
    sudo netstat -tap | grep mysql
    mysql -u root -p
	
	# backup and restore
	mysqldump -uroot -p database > dump.sql
	mysql -uroot -p database < dump.sql
	```
	```
	# conf
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
	
2. ### postgresql
	```
	sudo apt-get install libpq-dev postgresql postgresql-contrib
	
	# enter psql and setup passwd
	sudo su - postgres
	psql
	\password postgres
	
	psql -U postgres -h localhost
	
	# backup and restore
	pg_dump dbname > outfile
	psql dbname < infile
	
	# connector for python
	sudo pip install psycopg2
	```
	
3. ### mongodb
	```
	sudo apt-get install mongodb
	mkdir -p /data/db
	sudo chown -R mongodb:mongodb /data
	mongo
	
	# backup and restore
	mongodump --db dbname --out dbdump
	mongorestore --collection col --db dbname dbdump
	```
	
***

## Server
1. ### apache
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
		
		Alias /robots.txt /home/user/projects/DjangoApp/static/robots.txt
		Alias /favicon.ico /home/user/projects/DjangoApp/static/favicon.ico
        Alias /static/ /home/user/projects/DjangoApp/static/
    
        <Directory /home/user/projects/DjangoApp/static>
            Require all granted
        </Directory>
		
		WSGIDaemonProcess djangoapp python-path=/home/user/projects/DjangoApp:
			/home/user/projects/DjangoApp/venv/lib/python2.7/site-packages 
			processes=2 threads=15 display-name=%{GROUP}
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
	```
	```apacheconf
	# apache flask
	<VirtualHost *:80>
		ServerName flaskapp.domain.com

		WSGIDaemonProcess flaskapp python-path=/home/user/projects/FlaskApp:
			/home/user/projects/FlaskApp/venv/lib/python2.7/site-packages 
			processes=2 threads=15 display-name=%{GROUP}
		WSGIProcessGroup flaskapp
		WSGIScriptAlias / /home/user/projects/FlaskApp/app.wsgi
		WSGIApplicationGroup %{GLOBAL}
		
		<Directory /home/user/projects/FlaskApp>
			Require all granted
		</Directory>
	</VirtualHost>
    ```
	```
	# apache with ssl
	a2enmod ssl / a2dismod ssl
	service apache2 restart
	
	netstat -tap  # https listening
	```
	```apacheconf
	# add to site conf, with port 80
	# you can do http to https rewrite in apache or simply in django
	# with front-end nginx, you can use the nginx to do it
	RewriteEngine On
	RewriteCond %{HTTPS} off
	RewriteRule (.*) https://%{SERVER_NAME}/%$1 [R,L]
	
	<VirtualHost *:443>
        ServerName webapp.domain.com
        # ServerAlias domain.com
        ServerAdmin hevlhayt@foxmail.com
    
        Alias /static/ /home/user/projects/WebApp/static/
    
        <Directory /home/user/projects/WebApp/static>
            Require all granted
        </Directory>
				
		# no daemon settings
		WSGIProcessGroup webapp
        WSGIScriptAlias / /home/user/projects/WebApp/WebApp/wsgi.py
		WSGIApplicationGroup %{GLOBAL}
    
        <Directory /home/user/projects/WebApp/WebApp>
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
    ```
    sudo a2ensite site / a2dissite site
    service apache2 start / reload / restart / stop
    cat /var/log/apache2/error.log
    ```
	
2. ### nginx
	```
	sudo apt-get install nginx
	service nginx start / reload / restart / stop
	cat /var/log/nginx/error.log
	ln -s /etc/nginx/sites-available/site /etc/nginx/sites-enabled/site
	```
	
3. ### nginx as a front-end balancer with apache on back-end
	```
	# front-end proxy nginx to serve static
	# back-end apache to serve dynamic
	# https://www.linode.com/docs/uptime/loadbalancing/use-nginx-as-a-front-end-proxy-and-software-load-balancer/
	
	# serve apache on port 8000 (close 80 and 443)
	# comment out all static on apache, as we will serve js or css through nginx
	Listen 8000 # only
	<VirtualHost *:8000> # on every vh
	
	sudo apt-get install libapache2-mod-rpaf # for log
	```
	```
	# /etc/nginx/proxy_params
	proxy_set_header Host $host;
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

	client_max_body_size 100M;
	client_body_buffer_size 1m;
	proxy_intercept_errors on;
	proxy_buffering on;
	proxy_buffer_size 128k;
	proxy_buffers 256 16k;
	proxy_busy_buffers_size 256k;
	proxy_temp_file_write_size 256k;
	proxy_max_temp_file_size 0;
	proxy_read_timeout 300;
	```
	```
	# nginx proxy for sites under apache
	server {
		listen 80;
		server_name domain.com;

		location / {
			proxy_pass http://localhost:8000;
			include /etc/nginx/proxy_params;
		}

		location /static/ {
			root /home/hevlfreis/projects/WebApp;
			access_log off;
			error_log off;
		}
	}
	```
	```
	# nginx with ssl
	# first decrypt the private key 
	openssl rsa -in server.key -out /etc/nginx/ssl/server.key
	
	# vh
	server {
	   listen         80;
	   server_name    domain.com;
	   return         301 https://$server_name$request_uri;
	}

	server {
		listen 443 ssl;
		server_name domain.com;

		ssl on;
		ssl_certificate /etc/nginx/ssl/1_domain_bundle.crt;
		ssl_certificate_key /etc/nginx/ssl/server.key;

		location / {
			proxy_pass http://localhost:8000;
			include /etc/nginx/proxy_params;
		}

		location /static/ {
			root /home/hevlfreis/projects/WebApp;
			access_log off;
			error_log off;
		}
	}
	```
	
4. ### nodejs with nginx
	```
	server {
		listen 80;
		server_name domain.com;

		location / {
			proxy_pass http://localhost:3000
			
			# avoid http400 while using sockit.io
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
			proxy_set_header Host $host;
			
			include /etc/nginx/proxy_params;
		}

		location ~ ^/(images/|img/|javascript/|js/|css/|stylesheets/|flash/|media/|static/|robots.txt|humans.txt|favicon.ico){
			root /home/hevlfreis/projects/Chat2x/public;
			access_log off;
			error_log off;
		}

	}
	```
	
***

## Python
1. ### environments
    ```
	# in ubuntu, you should add dist-package as PYTHONPATH to .profile
	export PYTHONPATH=/usr/lib/python2.7/dist-packages:/usr/local/lib/python2.7/dist-packages
	
    sudo apt-get install python-pip
    sudo apt-get install python-dev
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
	# virtualenv
	sudo pip install virtualenv
	cd project
	virtualenv venv
	
	# active virtual env
	source bin/activate
	pip something in this venv
	deactivate
	```
	```
	# python for mysql
	sudo pip install MySQL-python
	or ?
	sudo pip install mysqlclient  
	
	# python for mongodb
	sudo pip install pymongo
	
	# orm for python
    sudo pip install sqlalchemy
	```
	```
	# hdf5 file system
	sudo apt-get install libhdf5-dev
	
	# install glib
	sudo apt-get install libglib2.0-dev
	sudo apt-get install libffi-dev
	
	# python markdown
	sudo pip install misaka
    ```
    
2. ### django
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
	
	# if you have set up the venv for this django, 
	# just make sure that you have set the venv path correctly in apache's conf. 
	# django will automatically use the project's venv python path.
    ```
    
    ```
    # user uploading path
    sudo chown -R www-data:www-data /upload
    sudo chmod -R g+w /upload
    ```
	```python
	# django with ssl
	SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
	SECURE_SSL_REDIRECT = True # or using server to do the https redirect
	SESSION_COOKIE_SECURE = True
	```
	
3. ### flask
	```
	sudo pip install Flask
	```
	``` python
	# app.wsgi
	
	# you can ignore the two lines below when you make sure that 
	# you have set the venv path correctly in apache's conf.
	# activate_this = '/home/user/projects/FlaskApp/venv/bin/activate_this.py'
	# execfile(activate_this, dict(__file__=activate_this))

	from yourapp import app as application

	import sys
	sys.path.insert(0, '/home/user/projects/FlaskApp')
	```
	
***

## JS
1. ### nodejs
	```
	sudo apt-get install nodejs
	sudo apt-get install npm
	
	ln -s /usr/bin/nodejs /usr/bin/node
	
	# cli tool for deployment
	sudo npm install forever -g --save
	
	# mongodb odm for nodejs
	npm install mongoose
	```
	
2. ### npm
	```
	npm install (-g) <package>
	```
	```
	# npm on win
	# canvas
	https://github.com/Automattic/node-canvas/wiki/Installation---Windows
	
	# node-gyp
	https://github.com/nodejs/node-gyp
	```
	
***

## Machine Learning Framework
1. ### torch and torch-rnn
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
	
	python scripts/preprocess.py 
		--input_txt my_data.txt --output_h5 my_data.h5 --output_json my_data.json
	
	th train.lua -input_h5 my_data.h5 -input_json my_data.json -gpu -1
	th sample.lua -checkpoint cv/checkpoint_10000.t7 -length 2000 -gpu -1
	```
	
2. ### tensorflow
	```
	# Ubuntu/Linux 64-bit, CPU only, Python 2.7
	(tensorflow)$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.10.0rc0-cp27-none-linux_x86_64.whl

	# Ubuntu/Linux 64-bit, GPU enabled, Python 2.7
	# Requires CUDA toolkit 7.5 and CuDNN v4. For other versions, see "Install from sources" below.
	(tensorflow)$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.10.0rc0-cp27-none-linux_x86_64.whl
	
	sudo pip install --upgrade $TF_BINARY_URL
	
	# tensorflow is based on a new version of python-six
	# but ubuntu 14.04 default python provides an old version of python-six
	# and we can't upgrade it through apt-get
	# solution is rm it by hand and then pip install a new one
	```
	
3. ### mxnet
	```
	sudo apt-get install build-essential libatlas-base-dev libopencv-dev
	
	git clone --recursive https://github.com/dmlc/mxnet
	cd mxnet; make -j$(nproc)
	
	# quick test
	python example/image-classification/train_mnist.py
	```
	
***

## Other
### https
```
https://startssl.com/  > 1_root_bundle.crt, 2_your_domain.crt (for apache) 
						 1_your_domain_bundle.crt (for nginx)

# on server 
openssl req -newkey rsa:2048 -keyout server.key -out server.csr
```


> If you have any problem, please contact hevlhayt@foxmail.com (ﾉﾟ▽ﾟ)ﾉ