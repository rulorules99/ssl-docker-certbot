[![N|Solid](https://ikuty.com/wp-content/uploads/2016/07/certbot.png)](https://certbot.eff.org/)
[![N|Solid](https://github.frapsoft.com/top/docker-security.jpg)](https://www.docker.com/)

## SSL CONFIG WITH DOCKER AND CERTBOT AND UBUNTU

 [FOLLOWER GUIDE](https://www.humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx)

WARNING!!!

> You have to replace yourdomain.com for
> you domain url example yourdomain.com -> example.com
> from all scripts.



#### Config with out SSL, in the server run the next commands
```sh
$ sudo mkdir -p /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site

$ sudo vim /docker/letsencrypt-docker-nginx/src/letsencrypt/docker-compose.yml # Use content of file of ./letsencrypt/docker-compose.yml
$ sudo vim /docker/letsencrypt-docker-nginx/src/letsencrypt/nginx.conf # Use content of file of ./letsencrypt/nginx.conf
$ sudo vim /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site/index.html # Use content of file of ./letsencrypt/index.html

$ cd /docker/letsencrypt-docker-nginx/src/letsencrypt
$ docker-compose up -d
```
### Test of certbot with --staging flag
```bash
$ sudo docker run -it --rm \
-v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
-v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--register-unsafely-without-email --agree-tos \
--webroot-path=/data/letsencrypt \
--staging \
-d yourdomain.com -d www.yourdomain.com
```
### Test of certbot with aditional information
```bash
$ sudo docker run --rm -it --name certbot \
-v /docker-volumes/etc/letsencrypt:/etc/letsencrypt \
-v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
certbot/certbot \
--staging \
certificates

$ sudo rm -rf /docker-volumes/
```

### Production of certbot certificate SSL
```bash
$ sudo docker run -it --rm \
-v /docker-volumes/etc/letscrypt:/etc/letsencrypt \
-v /docker-volumes/var/lib/letsencrypt:/var/lib/letsencrypt \
-v /docker/letsencrypt-docker-nginx/src/letsencrypt/letsencrypt-site:/data/letsencrypt \
-v "/docker-volumes/var/log/letsencrypt:/var/log/letsencrypt" \
certbot/certbot \
certonly --webroot \
--email youremail@domain.com --agree-tos --no-eff-email \
--webroot-path=/data/letsencrypt \
-d yourdomain.com -d www.yourdomain.com
```

### Stop the example container without SSL
```bash
$ cd /docker/letsencrypt-docker-nginx/src/letsencrypt

$ docker-compose down
```

# SSL CONFIG

### Set up Your Production Site to Run in Nginx Docker Container
```bash
$ sudo mkdir -p /docker/letsencrypt-docker-nginx/src/production/production-site
$ sudo mkdir -p /docker/letsencrypt-docker-nginx/src/production/dh-param

$ sudo vim /docker/letsencrypt-docker-nginx/src/production/docker-compose.yml # Use content of file of ./production/docker-compose.yml
$ sudo vim /docker/letsencrypt-docker-nginx/src/production/production.conf # Use content of file of ./production/production.conf                                                                                                               
$ sudo vim /docker/letsencrypt-docker-nginx/src/production/production-site/index.html # Use content of file of ./production/index.html 

$ sudo openssl dhparam -out /docker/letsencrypt-docker-nginx/src/production/dh-param/dhparam-2048.pem 2048

$ cd /docker/letsencrypt-docker-nginx/src/production
$ docker-compose up -d
```




