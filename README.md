# docker_curso

## Comandos comunes

```docker pull ubuntu```

### Para contenedores que estan corriendo 

```docker images | head``` para ver las imagenes que estan corriendo en la maquina. Cada imagen tiene un ID y Tag.

```docker ps``` muestra todos los contenedores que estan corriendo.

```docker ps -a```para ver el historial de los contenedores.

```docker start fsa09f7a79bd```

```docker logs fsa09f7a79bd```

```docker logs -f```

```docker exec``` ejecuta un comando dentro de un contenedor que ya está corriendo. 

```docker exec -it fsa09f7a79bd sh``` para simular una terminal interactiva. 

```docker run```ejecuta un contenedor a partir de una imagen. 

```docker stop fsa09f7a79bd``` para detener al contenedor. 

```docker run -d nginx```para correr una imagen en background. 

## Escribiendo un dockerfile

```vim Dockerfile```

Se recomiendo empezar usando una imagen que tenga todo lo que necesito. 

```
FROM node:12.22.1-alphine3.11

WORKDIR /app
COPY . .
RUN yarn install --production

CMD["node", "/app/src/index.js"]

``` 

Para construir el contenedor ```docker build -t getting_started .```

```docker run -dp 3000:3000 getting-started```

Para mantener los cambios de la app:

```docker run -d -v /Users/erickCuevas/ejemplo-docker/app/etc:/etc/todos -p 3000:3000 getting-started```

Al hacer esto se genera un archivo en `etc/todo.db` que funciona como base de datos. Es es util para generar apps que nos permitan guardar la informacion de los usuarios. 

```nvim src/static/js/app.js```

```docker run -d -v /Users/erickCuevas/ejemplo-docker/app/etc:ect/todos -p 3000:3000 -v /Users/erickCuevas/ejemplo-docker/app/src:/app/src getting started```

Al hacer esto vamos sobreescribiendo nuestra app dependiendo de los cambios que busquemos hacer. Una vez que terminemos de hacer cambios se vuelve a construir la imagen.

```docker build -t getting-started:v2```

La imagen se puede compartir en dockerHub de manera gratuita siempre y cuando sea libre lo que se comparte. 

## Para subir la imagen a DockerHub

```docker login```

```docker images | head```

```docker tag ID_de_la_imagen erick1394/getting-started:v2```

```docker images```

```docker push erick1394/getting-started:v2```

A partir de ahora toda persona puede hacer ```docker pull``` de la imagen

## Corriendo varios containers a la vez

Se recomienda usar un container como base de datos y otro como app, y hacer que ambos interactuen. 

```cd multicontainer```

```docker network create todo-app```

```docker run -d 
--network todo-app --network-alias mysql 
-v todo-mysql-data:/var/lib/mysql 
-e MYSQL_ROOT_PASSWORD=secret 
-e MYSQL_DATABASE=todos 
mysql:5.7
```

`-v /var/lib/mysql` es donde se encuentran la base de datos de mysql. 

Es buena practica ir a ver la documentacion de cada imagen para configurar correctamente las variables de entorno con `-e`.

```docker exec -it ID_image mysql -p```

Y ahora se pueden usar codigos de mysql como por ejemplo: `show databases` 

```docker run -it --network todo-app nicolaka/netshoot```

```dig mysql```

```docker run -dp 3000:3000 
--network todo-app 
-e MYSQL_HOST=mysql 
-e MYSQL_USER=root 
-e MYSQL_PASSWORD=secret 
-e MYSQL_DB=todos 
getting-started:v2
```

```docker ps```

```docker stop ID_container```

```docker run -dp 3000:3000 --network todo-app -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=todos getting-started:v2```




