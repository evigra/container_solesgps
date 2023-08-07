# CONTAINER

This repository contains the template to generate the containers

------------------

# GITHUB CONFIGURATION 

eval "$(ssh-agent -s)"; ssh-add ~/.ssh/developer



# DOCKER CONTAINER CONFIGURATIONS

sudo chown $(whoami):$(whoami) /var/run/docker.sock


# Create web container 
docker run -v ~/docker/container_solesgps/addons:/mnt/extra-addons -v ~/docker/container_solesgps/config/odoo.conf:/etc/odoo/odoo.conf -p 8069:8069 -p 8005:8005 --name container_solesgps --link db:db -t odoo:15.0

# Create database
docker exec -ti container_solesgps bash -c "/usr/bin/odoo -p 8005  -d developer-instance-solesgps --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"

# Install modules
docker exec -ti container_solesgps bash -c "/usr/bin/odoo -i instance_solesgps -p 8005  -d developer-instance-solesgps --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"

docker exec -ti container_solesgps bash -c "/usr/bin/odoo -u instance_solesgps,gpsmap -p 8005  -d developer-instance-solesgps --db_host db --db_port 5432 --db_user odoo --db_password odoo --without-demo all"
