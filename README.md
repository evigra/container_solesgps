# CONFIGURACION
CONTAINNER  container_PROYECTO
NUEVO       container_NUEVOPROYECTO
DOMAIN      PROYECTO.com
instance    instance_PROYECTO
DB          container_postgres_16

This repository contains the template to generate the containers

------------------
# UBUNTU

sudo adduser evigra
sudo usermod -aG sudo evigra 

sudo apt-get install docker-compose-v2
sudo usermod -aG docker evigra
mkdir /home/evigra/.ssh/
mkdir /home/evigra/docker/
cd /home/evigra/.ssh/
nano /home/evigra/.ssh/e.vizcaino@solesgps.com
chmod 600 /home/evigra/.ssh/e.vizcaino@solesgps.com
chmod 700 /home/evigra/.ssh
sudo apt-get install git

sudo apt-get install nginx

sudo apt-get install certbot
sudo apt install python3-certbot-nginx



# GITHUB CONFIGURATION 
git clone git@github.com:evigra/container_PROYECTO.git container_NUEVOPROYECTO
git remote set-url origin git@github.com:evigra/container_NUEVOPROYECTO.git

git push --set-upstream origin 16.0

eval "$(ssh-agent -s)"; ssh-add ~/.ssh/e.vizcaino@solesgps.com

git clone --recurse-submodules git@github.com:evigra/container_NUEVOPROYECTO.git

git submodule add git@github.com:evigra/instance_PROYECTO.git addons/instance_PROYECTO


# DOCKER CONTAINER CONFIGURATIONS

sudo chown $(whoami):$(whoami) /var/run/docker.sock

docker-compose up
docker-compose down
docker-compose restart

docker exec -it container_postgres_16 psql -U odoo -d postgres

# Install modules
docker exec -it container_PROYECTO bash -c "odoo -d container_PROYECTO -i instance_PROYECTO -p 8006 --db_host=container_postgres_16 --db_port=5432 --db_user=odoo --db_password=odoo  --without-demo=all"



# show container in console

docker exec -it container_PROYECTO /bin/bash


# NGIX

sudo nano /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx

sudo certbot --nginx -d PROYECTO.com -d www,PROYECTO.com
sudo certbot certificates
sudo systemctl status certbot.timer
sudo certbot renew --dry-run    

server {
    listen 80;
    server_name PROYECTO.com www.PROYECTO.com;
    client_max_body_size 10M;

    proxy_read_timeout 720s;
    proxy_connect_timeout 720s;
    proxy_send_timeout 720s;

    location / {
        proxy_pass http://127.0.0.1:8016;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_redirect off;
    }

    location /websocket {
        proxy_pass http://127.0.0.1:8016;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_read_timeout 720s;
    }
}
