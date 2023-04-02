# CONTAINER

This repository contains the template to generate the containers

------------------

## CONFIGURATION 

sudo chown $(whoami):$(whoami) /var/run/docker.sock

# Create container
docker run -v ~/docker/container/addons:/mnt/extra-addons -p 8014:8069 -p 8114:8114 --name odoo_14 --link db:db -t odoo:14.0 

# Install module gpsmap
docker exec -ti odoo_14 bash -c "/usr/bin/odoo -p 8114 -i gpsmap -d odoo_14 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"
