entrar dentro de la carpeta y ejecutar el siguiente comando:

docker build -t viernes/odooxmarts:12.0.4 .

docker tag viernes/odooxmarts:12.0.1 viernes/odooxmarts:latest

docker save <image> | bzip2 | \
     ssh user@host 'bunzip2 | docker load'




nota 

CREA UN CONTENEDOR CON LA BASE DE DATOS POSTGRES
docker run --name db -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -d -p 5432:5432 postgres:9.6-alpine

LOGIN A DOCKER HUB 
docker login 

DESCARGAMOS LA IMAGEN 
docker pull odooxmarts:latest


CREAMOS EL CONTENEDOR ENETERPRICE

docker run --link db --name odoo12 -e POSTGRES_USER=odoo -e POSTGRES_HOST=localhost  -e CREATEUSER=no -d   -p 8066:8069 viernes/odooxmarts:12.0.4

CRRAMOS CONTENDOR CE 

docker run --link db --name odoo12 -e POSTGRES_USER=odoo -e POSTGRES_HOST=localhost  -e CREATEUSER=no -e ODOO_RC=/etc/odoo/odooce.conf -d   -p 8066:8069 viernes/odooxmarts:12.0.4

