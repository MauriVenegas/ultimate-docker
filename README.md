# 🐋Curso Ultimate Docker

## Terminal Linux

### Ejecutando Linux
```sh
# run: busca imagen en local, si no esta la descarga y luego la ejecuta
docker run ubuntu
# Muestra imágenes
docker image ls
# Ver los proceso (contenedores) corriendo
docker ps
# Ver procesos y contenedores detenidos
docker ps -a
# Acceder a la shell de la imagen, -it: interactive
docker run -it ubuntu
# Busca en los comandos ya ingresados
CTRL + R
# Ver el historial de comandos
history
```

### Navegando en el sistema de archivos
```sh
# Donde estoy. PWR: Print Working Directory
pwr
# Lista detallada de directorios
ls -l
# Muestra el contenido si ingresar a el
ls [nombre directorio]
```

### Gestión de archivos
```sh
# Crear directorio
mkdir [nombre directorio]
# Cambiar nombre
mv [nombre actual] [nuevo nombre]
# Crear multiple archivos
touch [nombre archivo] [nombre archivo 2] ...
# Eliminar archivos
rm [nombre archivo] [nombre archivo 2] ...
# eliminar directorios -r: recursivo
rm -r [nombre directorio]
```

#### Búsqueda de texto
```sh
# Buscar texto en un archivo
grep [texto] [archivo]
# También se puede en multiples archivos
grep [texto] [archivo1] [archivo2] ...
# O todo los archivos con la extensión .txt
grep [texto] *.txt
# No senile a mayúsculas y minúsculas
grep -i [texto] *.txt
# Buscar directorios con -r
grep -ir [texto] .
```

### Encadenando comandos
```sh
# Varios comandos en una linea
mkdir holamundo ; cd holamundo ; echo "Listo"
# Si uno tiene éxito que se ejecute el siguiente
mkdir holamundo && cd holamundo && echo "Listo"
# si uno falla se ejecuta el siguiente
mkdir holamundo || echo "Existe"
```

### Variables de entorno
env: environment
```sh
# Ver las variables de entorno
env
# Ver el valor de una variable
echo $PATH
```
### Gestión de usuario
- #: administrador
- $: usuario común
```sh
# Crear usuario
useradd -m [nombre usuario]
# Ver usuarios
cat  /etc/passwd
# Cambiar shell de usuario a bash
usermod -s /bin/bash [nombre usuario]
# Ver contraseñas de usuarios
cat /etc/shadow
# Cambiar de usuario, su: switch user
su [nombre usuario]
# Salir del usuario y cambiar al admin
exit
# Eliminar usuario (solo lo puede hacer el admin)
userdel [nombre usuario]
# Iniciar contenedor con un usuario en especifico
docker start -i [primeros dígitos id]
docker exec -it -u [nombre usuario] [primeros dígitos id] bash
```

### Gestión de grupos
```sh
# Ver los grupos de un usuario (al crear un usuario se le crea un grupo con su nombre de usuario)
groups [nombre usuario]
# Crear grupo
groupadd [nombre grupo]
# Ver grupos
cat /etc/group
# Agregar usuario a un grupo -g: forzar a tener un nuevo grupo principal, -G: agrega el grupo al listado de grupos del usuario
usermod -G [grupo] [usuario]
```

### Permisos
```sh
# Si creamos un archivo y lo intentamos abrir nos dirá que no tenemos permiso
echo "echo hola mundo" > archivo.sh
./archivo.sh
# Ver detalles de un archivo
ls -l archivo.sh
# -rw-r--r-- 1 root root
# -: archivo | d: directorio
# r: read (lectura), w: write(escritura), x: execute (ejecutar)
# rw-:permisos del usuario | r--: permisos del grupo | r--: otros
# root: dueño del archivo | root: grupo del archivo

# Agregar permiso de ejecución para el usuario => u: usuario, +: agregar, x: ejecutar
chmod u+x archivo.sh
# Agregar permiso a un grupo => g: grupo
# Agregar permiso a otros => o: otros

# Agregar permiso de escritura y ejecución al grupo
chmod g+wx archivo.sh
# Quitar permiso de lectura, escritura y ejecución a otros
chmod o-rwx archivo.sh
# También se puede hacer todo junto separado por coma (,)
chmod u-w,g-wr,o+x archivo.sh
# Asignar todos los permisos al usuario
chmod u=rwx archivo.sh
```

