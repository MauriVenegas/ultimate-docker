# 🐋 Despliegue de aplicaciones

## herramientas de despliegues

- Docker swarm (poco usado)
- Kubernetes (k8s)
- Despliegue simple (contratando un servidor)
- Despliegue de app (solo permite una imagen y servidor por contenedor)

## Opciones de despliegue

- AWS (Amazon)
- Azure (Microsoft)
- Google Cloud
- Digital Ocean (el mas simple)

## Llaves SSH

- **Llave publica**: para encriptar el mensaje a enviar (se comparte con el servidor y gitHub)
- **Llave privada**: para desencriptar (leer) el mensaje recibido (no se comparte con nadie)

Generar llaves publica/privada

```sh
ssh-keygen -t ed25519 -C "mvenegas@ninjaexcel.com"
# Mostrara el siguiente mensaje 'Generating public/private ed25519 key pair'
# En los siguientes pasos se puede presionar 'enter' para generar valor por defecto
```

En windows esta llave se guarda por defecto en la carpeta **C:\Users\Mauricio Venegas\.ssh**, se debe abrir esta carpeta y copiar el contenido (llave) del archivo **id_ed25519.pub**.

Esta se pega en **Add new SSH Key** de GitHub para conectarse con Github sin usar contraseña.

## Digital Ocean

### Crear Droplet

Droplet es una maquina virtual basada en Linux.
No continue porque Digital Ocean me pedía ingresar una tarjeta de crédito.

## Optimizando la imagen para producción

Dockerfile.prod

```sh
# Paso 1
# as build-stage: para referenciarla
FROM node:20.5.0-alpine3.18 as build-stage
ARG VITE_API_URL
ENV VITE_API_URL=$VITE_API_URL
WORKDIR /app/
COPY package*.json .
RUN npm install
COPY . .
RUN ["npm", "run", "build"]

# Paso 2
# crear una imagen a partir de otra imagen
FROM nginx:1.25.1-alphine3.17-slim
COPY --from=build-stage /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

docker-compose.prod.yml

```sh
services:
  app:
    build:
      context: ./frontend
      dockerfile: Dockerfile.prod
      args:
        VITE_API_URL: http://170.64.171.185:3000
    ports:
      - 80:80
    # Volumes: actualiza los cambios echos en local en el contenedor
    volumes:
      # - [ubicación local]: [ubicacion contenedor]
      - ./frontend/src:/app/src
    environment:
      VITE_API_URL: http://id-produccion:3000
  api:
    build: ./backend
    ports:
      - 3000:3000
    environment:
      DB_URL: mongo://db/gamify
    volumes:
      # - [ubicación local]: [ubicacion contenedor]
      - ./backend/app:/app/app
    # depends_on: para cuando un contenedor depende de otro contenedor, ejecuta primero api y despues db
    depends_on:
      - db
  db:
    image: mongo:5.0.19-focal
    ports:
      - 27017:27017
    volumes:
      - gamify:/data/db

volumes:
  gamify:
```
