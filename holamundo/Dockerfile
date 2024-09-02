# FROM: indica la iamgen base o sistema operativo
# WORKDIR: cambia el directorio de trabajo
# COPY: para agregar archivos
# ADD: para agregar archivos que estan en una url o archivo comprimido
# RUN: para ejecutar comandos
# ENV: veriables de entorno
# EXPOSE: exponer los puertos
# USER: asignar usuarios
# CMD: se ejecuta cuando se inicia el contenedor (este se puede reescribir desde la linea de comandos)
# ENTRYPOINT: se ejecuta cuando se inicia el contenedor

FROM node:20.5.0-alpine3.18
# Contruir imagen => docker build -t [nombre app] .
# Acceder el sistema operativo => docker run -it [nombre app] sh

# Saber usuario actual => whoami
# Crear grupo => addgroup [nombre grupo]
# Crear usuario y asignar a un grupo => adduser -S -G [nombre grupo] [nombre usuario]
RUN addgroup react && adduser -S -G react react
# Cambia al usuario react
USER react

# Asignamos un nuevo espacio de trabajo
WORKDIR /app/

# Copiar y pegar archivos en la imagen

# Cada vez que se modifica un archivo (escribir codigo) de vuelven a cargar todos los archivos y ejecutar 'npm install' demorando mucho el proceso por lo cual lo mejor es copiar los package.json, ejecutar 'npm install' y al final copiar el resto de archivos
# --chown=react: para que el usuario react tenga los permisos
COPY --chown=react package*.json .

# Primer '.' es copiar todo de donde estoy y el segundo '.' pegar lo copiado en el directorio definido por WORKDIR  '/app/'
COPY --chown=react . .

# Excluyendo archivos inecesarios, se crea un archivo '.dockerignore' para agregar archivo como node_modules

# Instalar las dependencias, RUN: se ejecuta mientras se contruye la imagen
RUN npm install

# Cada uno de los comandos que se ejecutan es una capa
# Podemos ejecutr: => 'docker history [nombre app]' para ver el paso de cada ejecucion por ejemplo 'npm install'

# Variables de entorno
ENV API=https://apiv1.miweb.com
# Desde dentro del sistem operativo de la imagen vemos las variables de entorbo con:
# 'printenv' o 'printenv [nombre variable de entirno]'

# Puerto a exponer
EXPOSE 5173

# Ejecutar comandos dentro de la misma shell
# CMD: se usa cuando se ejecuta el contenedor sobre la imagen ya ejecutada
CMD ["npm", "run", "dev"]
