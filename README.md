# CONTAINER

This repository contains the template to generate the containers

------------------

## CONFIGURATION 



sudo chown $(whoami):$(whoami) /var/run/docker.sock


# Create database
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:13

# Create container
docker run -v ~/docker/container_solesgps/addons:/mnt/extra-addons -p 8015:8069 -p 8115:8115 -p 8071:8071 -p 8072:8072 --name odoo_15 --link db:db -t odoo:15.0

# Install modules
docker exec -ti odoo_15 bash -c "/usr/bin/odoo -i module -p 8115  -d odoo_15 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"

docker exec -ti odoo_15 bash -c "/usr/bin/odoo -i instance_solesgps -p 8115  -d odoo_15 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"