# 🐋 App Node + React + Docker

## Archivo de configuración de docker con yml
Este archivo es para ejecutar el frontend (app), el backend los cuales tienen su Dockerfile y la base de datos
```sh
# Construir y ejecutar archivo de configuración docker-compose.yml
docker compose up
# Construir sin cache: para cuando los contenedores o imágenes se portan extraño
docker compose build --no-cache
# Detiene y elimina los contenedores y redes creadas por docker compose
docker compose down
# Eliminar imágenes que no se están usando
docker image prune
```

## Logs
```sh
# construir y ejecutar sin los logs
docker compose up --build -d
docker compose up -d
# Ver logs
docker compose logs
# Ver contenedores que se están ejecutando con compose
docker compose ps
```

## Redes
```sh
# Ver la redes creadas
docker network ls
```
- **Red bridge y driver bridge**: red para contenedores dentro de la misma red para que se pueden comunicar entre si
- **Driver host**: utiliza la red del anfitrión (del pc donde esta docker)
- **null**: No se comunica con nadie
- **overley**: 2 o mas maquinas con docker se pueden comunicar (Kubernetes)
-* Red default*: para conectar la app, api y db del docker-compose.yml

