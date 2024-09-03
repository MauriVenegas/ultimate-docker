# 🐋 React + Vite + Docker + Docker hub

## Imágenes

### Crear imagen
```sh
# construir imagen
docker build -t [nombre app]:[nombre etiqueta] .
```

### Detener contenedores
```sh
# Ver contenedores ejecutándose
docker ps
# detener ejecución del contenedor
docker stop [primeros 3 caracteres del id]
```

### Eliminar imagen
```sh
# Ver imágenes creadas
docker images
# Eliminar contenedores detenidos
docker container prune
# Lugo de eliminar los contenedores se pueden eliminar las imágenes que no se están ejecutando
docker image prune
# Eliminar imágenes que están usando
docker image rm [id imagen] [id otra imagen] ...
```

### Etiquetas (Tags)
```sh
# Asignar etiqueta a una imagen
docker build -t [nombre imagen]:[nombre etiqueta] .
# Eliminar imagen con etiqueta
docker image rm [id imagen]:[nombre etiqueta]
# Cambiar etiqueta de una imagen
docker image tag [imagen]:[etiqueta] [imagen]:[nueva etiqueta]
```

### Publicar imagen
```sh
# cambiar nombre imagen
docker image tag [imagen]:[etiqueta] [nuevo nombre imagen]:[etiqueta]
# Iniciar sesión
docker login
# Publicar imagen
docker push [nombre imagen]:[etiqueta]
```

### Guardar y cargar imágenes
```sh
# Guardar imagen
docker image save -o [nombre del archivo a crear] [nombre imagen]:[etiqueta]
# Cargar imagen
docker image load -i [nombre archivo]
```
## Contenedores

### Iniciar y detener contenedor
```sh
# Crear contenedor desde una imagen
docker create --name [nombre contenedor] [nombre imagen]:[etiqueta]
# Ver los contenedores corriendo
docker ps
# Ver los contenedores detenidos
docker ps -a
# Iniciar contenedor
docker start [nombre contenedor o id]
# Crear contenedor desde una imagen y lo ejecuta (docker pull + create + start)
docker run -d --name [nombre contenedor] [nombre imagen]:[etiqueta]
# Detener contenedor
docker stop [id contenedor]
# Eliminar contenedores detenidos
docker container prune
```

### Logs
```sh
# Ver logs
docker logs [id o nombre contenedor]
# Para que siga observando al contenedor
docker logs -f [id o nombre contenedor]
```

### Puertos
```sh
# Mapear puertos
# Ejemplo => docker run  -d -p 80:5173 --name holarun 16c
docker run -d -p [puerto anfitrion]:[puerto contenedor] --name [nombre contenedor] [id imagen]
# Para ver la aplicación ingresamos la URL
# Ejemplo => http://localhost/80:5173
http://localhost/[puerto anfitrion]:[puerto contenedor]
```

### Eliminar contenedores
```sh
# primero detener contenedor
docker stop [id contenedor]
# Eliminar contenedor
docker container rm [id contenedor]
# Si el contenedor se esta ejecutando se puede forzar le eliminación
docker container rm -f [id contenedor]
```

### Ejecutar comandos (en el contenedor)
```sh
# Ejecutar comando dentro del contenedor de forma interactiva (-it) para entrar a la consola
# Ejemplo => docker exec -it holamundo sh
docker exec -it [nombre contenedor] [shell a usar]
# Solo ejecutar un comando al especifico (sin modo interactivo)
# Ejemplo => docker exec holamundo ls
docker exec [nombre contenedor] [comando a usar]
```

### Volúmenes
Se utilizan para guardar datos del contenedor
```sh
# Crear volumen
docker volume create [nombre volumen]
# Ver información del volumen
docker volume inspect [nombre volumen]
# Crear contenedor con volumen
# 👁️ Ojo: la carpeta 'datos' debe estar creada desde el archivo 'Dockerfile' para que tenga los permisos de usuarios
# Ejemplo => docker run -d -p 3000:3000 -v datos:/app/datos app-react:3
docker run -d -p [puerto anfitrion]:[puerto contenedor] -v [nombre volumen]:[ruta contenedor] [imagen]
# Crear contenedor con nombre y volumen
# Ejemplo => docker run -d -p 3000:3000 --name holavolumen -v datos:/app/datos app-react:4
```
### Compartiendo código
Al crear la imagen, esta se crea del código actual, pero si se modifica el código local este no se modifica en la imagen. Tenido que volver a crear una nueva nueva imagen con el código nuevo.

- En producción siempre hay que crear una nueva imagen.
- Pero en desarrollo podemos usar los volúmenes para actualizar los cambios

```sh
# Vincular directorio con contenedor para actualizar los cambios
# Para que funcione en windows:
  # se debe usar usar el '\' en la ruta de la carpeta local
  # Y aplicar el hot reload  para que tome los cambios => -e CHOKIDAR_USEPOLLING=true
# Ejemplo => docker run -d -e CHOKIDAR_USEPOLLING=true -p 80:5173 --name holamundo -v .\src:/app/src app-react:4
docker run -e CHOKIDAR_USEPOLLING=true -d -p [puerto anfitrion]:[puerto contendor] --name [nombre contenedor] -v .\src:/app/src [imagen]
```

### Copiar archivos de forma bidireccional
```sh
# Copiar desde el contenedor hacia mi directorio local
# Ejemplo => docker cp fad3046b4f4d:/app/contenedor.txt .
docker cp [id completo contenedor]:[ruta del archivo con su extension] [ruta de pagado en directorio local]

# Copiar desde el directorio local al contenedor
# Ejemplo => docker cp anfitrión.txt fad3046b4f4d:/app/
docker cp [ruta del archivo con su extension] [id completo contenedor]:[ruta de pegado en el contenedor]
```