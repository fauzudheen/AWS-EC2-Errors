# Setting up Django with Postgres, Nginx, and Gunicorn on Ubuntu 22.04

This guide includes commands for handling potential errors during the process of setting up a Django project with Postgres, Nginx, and Gunicorn on Ubuntu 22.04 and setting up SSL certificates with Certbot.

## Blog
https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu

## Potential Errors and Solutions

### Error: Gunicorn Can't Connect to ('0.0.0.0', 80)

If you encounter the following error:

[2019-06-22 11:47:13 +0000] [20317] [ERROR] Retrying in 1 second.

[2019-06-22 11:47:14 +0000] [20317] [ERROR] Can't connect to ('0.0.0.0', 80)


**Solution:**

```
pkill gunicorn
```

### Error: Forbidden (403) - CSRF Verification Failed

If you face a 403 Forbidden error with CSRF verification failed, check your CSRF_TRUSTED_ORIGINS settings. Ensure that the correct origin is allowed.

```python
CSRF_TRUSTED_ORIGINS = ['http://3.25.67.70']
```

### Error: 413 Request Entity Too Large

If you encounter a 413 Request Entity Too Large error, adjust the client_max_body_size in nginx.conf.

```bash
sudo nano /etc/nginx/nginx.conf
```

```nginx
http {
    # Other configurations...
    client_max_body_size 20M; 
}
```

### Database Connection Command

To connect to the database using psql:

```bash
psql -U [user] -d [dbname] -h localhost -W
```

### Certbot SSL Installation

To install Certbot for Apache or NGINX:

```bash
pip install certbot certbot-nginx
```

Create a symlink to ensure Certbot runs:

```bash
sudo ln -s env/bin/certbot /usr/bin/certbot
```

Create SSL certs for a specified domain:

```bash
sudo certbot --nginx -d solemate.fauzudheen.online
```

### Letsencrypt SSL Certificate Renewal

To renew SSL certificates with Certbot:

```bash
sudo env/bin/certbot renew
```

If renewal fails with an internal server error, try:

```bash
sudo env/bin/certbot renew --dry-run
```

Then retry the previous command.


