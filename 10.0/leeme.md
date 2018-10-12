entrar dentro de la carpeta y ejecutar el siguiente comando:

docker build -t  viernes/odooxmarts:10.0.9 .

docker save <image> | bzip2 | \
     ssh user@host 'bunzip2 | docker load'




nota 
crear postgre 
docker run --name db  -e POSTGRES_PASSWORD=syspass -d -p 5432:5432 postgres:9.6-alpine

docker run --link db --name odoo10 -e POSTGRES_USER=odoo -e POSTGRES_HOST=localhost  -e CREATEUSER=no -d   -p 8066:8069  viernes/odooxmarts:10.0.9


