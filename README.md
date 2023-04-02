# CONTAINER

This repository contains the template to generate the containers

------------------

## CONFIGURATION 

sudo chown $(whoami):$(whoami) /var/run/docker.sock

# Create container
docker run -v ~/docker/container/addons:/mnt/extra-addons -p 8013:8069 -p 8113:8113 --name odoo_13 --link db:db -t odoo:13.0 

# Install module gpsmap
docker exec -ti odoo_13 bash -c "/usr/bin/odoo -p 8113 -i gpsmap -d odoo_13 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"
