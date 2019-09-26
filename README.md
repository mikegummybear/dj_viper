Viper Project
=============

A sample Django project with a good production setup.


## Development setup

To do


## Production setup

1) Clone project to /srv/src/

```
cd /srv/src/
git clone https://github.com/codehappyph/dj_viper.git
```


2) Create virtualenv called `viper` and set it as local python version
  for the project directory

```
cd /srv/src/dj_viper/
pyenv virtualenv 3.6.9 viper
pyenv local viper
```


3) Install requirements

Whenever you are inside the project folder, the virtualenv, `viper`, will
always be activated by default.

```
cd /srv/src/dj_viper/
pip install -r requirements.txt
```

4) Setup the celery systemd service

Copy all systemd service files to the system settings

```
cd /srv/src/dj_viper/
sudo cp -r var/etc/systemd/system/viper-celery* /etc/systemd/system/
systemctl daemon-reload
```

**NOTE:**

When you make changes to the celery service files, you will need to copy
them again to the `/etc/systemd/system` folder and reload the systemctl
daemon.


5) Setup nginx gunicorn

Make sure user is part of www-data group

```
sudo usermod -aG www-data pydev
```

Copy the nginx sites

```
cd /srv/src/dj_viper/
sudo cp var/etc/nginx/sites-available/viper /etc/nginx/sites-available/
sudo ln -s /etc/nginx/sites-available/viper /etc/nginx/sites-enabled/viper
```


6) Start the gunicorn and celery services

```
systemctl start viper-celery.service
systemctl start viper-celerybeat.service

systemctl start viper-gunicorn.sock
systemctl enable viper-gunicorn.sock
```

There are also: stop and restart commands

Whenever you make code changes, you will need to restart the celery services.

References:

Celery Daemonizing

- http://docs.celeryproject.org/en/latest/userguide/daemonizing.html#usage-systemd

Django Gunicorn / NGINX

- https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04#creating-systemd-socket-and-service-files-for-gunicorn
- https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-18-04#configure-nginx-to-proxy-pass-to-gunicorn
