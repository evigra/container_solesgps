# CONTAINER

This repository contains the template to generate the containers

------------------

## CONFIGURATION 



sudo chown $(whoami):$(whoami) /var/run/docker.sock


# Create database
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:13

# Create container
docker run -v ~/docker/container/addons:/mnt/extra-addons -p 8014:8069 -p 8114:8114 --name odoo_14 --link db:db -t odoo:14.0

# Install modules
docker exec -ti odoo_14 bash -c "/usr/bin/odoo -i module -p 8114  -d odoo_14 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"
