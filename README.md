# CONTAINER

This repository contains the template to generate the containers

------------------

## CONFIGURATION 



sudo chown $(whoami):$(whoami) /var/run/docker.sock


# Create database
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:13

# Create container
docker run -v ~/docker/container_solesgps/addons:/mnt/extra-addons -p 8016:8069 -p 8116:8116 --name odoo_16 --link db:db -t odoo:16.0

# Install modules
docker exec -ti odoo_16 bash -c "/usr/bin/odoo -i module -p 8116  -d odoo_16 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"

docker exec -ti odoo_16 bash -c "/usr/bin/odoo -i instance_solesgps -u gpsmap -p 8116  -d odoo_16 --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"