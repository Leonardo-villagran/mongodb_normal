### Docker Compose para desplegar un servidor de MongoDB

Este archivo Docker Compose facilita la configuración de un entorno de desarrollo de MongoDB en tu máquina local. Puedes utilizarlo para desplegar una instancia de MongoDB y gestionar tus bases de datos de manera eficiente.

![alt text](public/images/docker_mongodb.jpg)

## Índice

* [Requisitos previos](#requisitos-previos)
* [Instrucciones de uso](#instrucciones-de-uso)
* [Conexión desde Python](#conexión-desde-python)
* [Notas](#notas)

## Requisitos previos

Antes de utilizar este Docker Compose, asegúrate de tener instalado lo siguiente en tu sistema:

* Linux:

   - [Docker](https://www.docker.com/get-started)
   - [Docker Compose](https://docs.docker.com/compose/install/)

* Windows:

   - [Docker Desktop](https://www.docker.com/products/docker-desktop/)

## Instrucciones de uso

### 1. Clona este repositorio en tu máquina local si aún no lo has hecho.

```
git clone https://github.com/Leonardo-villagran/mongodb_normal
```
---
### 2. Crea un archivo `.env` en la misma ubicación que el archivo `docker-compose.yml` con la siguiente configuración:

   ```plaintext
   MONGO_INITDB_ROOT_USERNAME=admin
   MONGO_INITDB_ROOT_PASSWORD=admin
   MONGO_INITDB_DATABASE=admin
   MONGO_PORT=27017
   ```

   - Asegúrate de configurar los valores según tus necesidades. Si estás en modo desarrollo no es necesario cambiar nada, pero si estás en modo producción cambia los valores para tu seguridad.
---
### 3. Asegúrate de que Docker esté en funcionamiento en tu sistema.

   * Importante:

      - Recuerda que para ejecutar Docker en tu sistema, la virtualización de la CPU debe estar habilitada en la BIOS.
      - Para usuarios de Windows, asegúrate de habilitar la Plataforma de Máquina Virtual y el Subsistema de Windows para Linux.
      ![Windows](public/images/windows_docker_requirements.jpg)
---
### 4. Abre una terminal y navega hasta la ubicación del archivo `docker-compose.yml`.
---
### 5. Ejecuta el siguiente comando para crear y ejecutar el contenedor de MongoDB:

   ```bash
   docker-compose up --build -d
   ```

   - El flag `-d` significa "en segundo plano". Esto lanzará el contenedor de MongoDB en segundo plano.
---
### 6. Conectar a la base de datos

Una vez que el contenedor se haya iniciado correctamente, puedes conectarte a la instancia de MongoDB utilizando la configuración que has proporcionado en el archivo `.env`.

   * Opciones de administración:
      - Puedes optar por utilizar una herramienta de administración de bases de datos como [MongoDB Compass](https://www.mongodb.com/products/compass) o conectarte directamente desde una aplicación compatible con MongoDB. A continuación, te proporciono un ejemplo de cómo conectar a la base de datos desde MongoDB Compass:

      ![Compass](public/images/compass.jpg)

      - Para conectar a través de la terminal puedes hacerlo de la siguiente forma (según el tipo de sistema que estés ocupando) debes descargar mongosh y agregarlo a las variables de entorno si es necesario, después escribir en la terminal:

         - Git Bash:
         ```
         ./mongosh --host localhost --port 27017 -u admin -p admin --authenticationDatabase admin
         ```

         - CMD: 
         ```
         mongosh --host localhost --port 27017 -u admin -p admin --authenticationDatabase admin
         ```

         - Desde la terminal para usar directamente desde docker:
         ```
         docker exec -it mongodb mongosh -u admin -p admin --authenticationDatabase admin
         ```

         **Importante:** Estoy trabajando en Windows, es por esa razón que uso mongosh en la terminal, por lo que en Linux no debería ser necesario y se podría usar directamente el comando `mongo`.
---

## Conexión desde Python

```python
#pip install pymongo
import pymongo

# Establece la URL de conexión con las credenciales
url = "mongodb://admin:admin@localhost:27017/admin"
conexion = pymongo.MongoClient(url)

# Accede a la base de datos deseada
base_de_datos = conexion["<base de datos>"]

# Accede a la colección deseada dentro de la base de datos
coleccion = base_de_datos["<colección>"]

# Realiza operaciones en la colección
documentos = coleccion.find()

for documento in documentos:
    print(documento)

# Cierra la conexión
conexion.close()
```

## Notas

- El archivo `docker-compose.yml` define un servicio llamado `mongodb` que utiliza la imagen oficial de MongoDB. Asegúrate de que la versión de la imagen coincida con tus requisitos.

- Se ha configurado un volumen para persistir los datos de MongoDB en `./data`. Puedes modificar esta ubicación según tus necesidades.

- El archivo `.env` se utiliza para almacenar variables de entorno que se pasan al contenedor de MongoDB. Asegúrate de configurar las variables de entorno según tus requisitos.

- Puedes cambiar el puerto externo (variable `MONGO_PORT`) en el archivo `.env` si es necesario.