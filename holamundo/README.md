# 🐋 React + Vite + Docker + Docker hub

## Imagenes

### Detener contenedores
```sh
# Ver contenedores ejecutandose
docker ps
# detener ejecución del contendor
docker stop [primeros 3 caracteres del id]
```

### Eliminar imagen
```sh
# Ver imagenes creadas
docker images
# Eliminar contenedores detenidos
docker container prune
# Lugo de eliminar los contenedores se pueden eliminar las imagenes que no se estan ejecutando
docker image prune
# Eliminar imagenes que estan usando
docker image rm [id imagen] [id otra imgen] ...
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
# Iniciar sesion
docker login
# Publicar imagen
docker push [nombre imagen]:[etiqueta]
```

### Guardar y cargar imagenes
```sh
# Guardar imagen
docker image save -o [nombre del archivo a crear] [nombre imagen]:[etiqueta]
# Cargar imagen
docker image load -i [nombre archivo]
```

