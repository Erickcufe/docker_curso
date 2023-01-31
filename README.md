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

``docker exec``` ejecuta un comando dentro de un contenedor que ya está corriendo. 

``docker exec -it fsa09f7a79bd sh``` para simular una terminal interactiva. 

```docker run```ejecuta un contenedor a partir de una imagen. 

```docker stop fsa09f7a79bd``` para detener al contenedor. 

```docker run -d nginx```para correr una imagen en background. 

