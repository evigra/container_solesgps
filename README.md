# CONTAINER

This repository contains the template to generate the containers

------------------

## CONFIGURATION 



sudo chown $(whoami):$(whoami) /var/run/docker.sock


# Create database
docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:13

# Create container
docker run -v ~/docker/container/addons:/mnt/extra-addons -p 80{odoo_version}:8069 -p 81{odoo_version}:81{odoo_version} --name odoo_{odoo_version} --link db:db -t odoo:{odoo_version}.0

# Install modules
docker exec -ti odoo_{odoo_version} bash -c "/usr/bin/odoo -i module -p 81{odoo_version}  -d odoo_{odoo_version} --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"
