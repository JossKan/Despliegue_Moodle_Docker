# Despliegue Moodle Docker

Actividad 4 de la 2 unidad DOCKER

## 🧩 Paso 1: Instalar y Configurar Docker Compose

Asegúrate de tener instalado **Docker Desktop** en tu **Maquina**.  
> ⚙️ *Docker Desktop incluye automáticamente Docker Compose, por lo que probablemente ya esté listo para usar.*

Puedes verificar la instalación abriendo la **Terminal** y ejecutando el siguiente comando:

```bash
docker compose version

🧱 Paso 2: Crear el Archivo `docker-compose.yml

Este archivo define todos los **servicios necesarios para ejecutar Moodle**, incluyendo:
- 🗄️ La base de datos (**PostgreSQL**, recomendada para Moodle).
- 🌐 El propio servidor de **Moodle**.

Primero, crea un directorio para tu proyecto y navega hacia él:

```bash
mkdir moodle-lms
cd moodle-lms

Luego, crea un archivo llamado docker-compose.yml y ábrelo con tu editor de texto preferido (por ejemplo, nano o vim):

```bash
nano docker-compose.yml

En este archivo agregarás la configuración de los contenedores de Moodle y PostgreSQL en los siguientes pasos.

Pega el siguiente contenido. Usaremos la imagen oficial de PostgreSQL para la base de datos y la imagen oficial de Bitnami para Moodle, ya que es una de las más confiables y actualizadas.

YAML

version: '3.7'

services:
  # ----------------------------------------
  # 1. Base de Datos (PostgreSQL)
  # ----------------------------------------
  moodle-db:
    image: bitnami/postgresql:latest
    container_name: moodle_db
    restart: unless-stopped
    environment:
      # Estas variables son esenciales para la conexión de Moodle
      - POSTGRESQL_USERNAME=bn_moodle
      - POSTGRESQL_PASSWORD=tu_password_segura # ¡CAMBIA ESTO!
      - POSTGRESQL_DATABASE=bitnami_moodle
    volumes:
      # Persistencia de datos: almacena los datos de la DB en el host
      - ./data/postgresql:/bitnami/postgresql

  # ----------------------------------------
  # 2. Servidor Moodle (PHP/Apache)
  # ----------------------------------------
  moodle:
    image: bitnami/moodle:latest
    container_name: moodle_app
    restart: unless-stopped
    ports:
      # Mapeo de puertos: Host:Contenedor
      - "8080:8080"
      - "8443:8443"
    environment:
      # Configuración de Moodle y conexión a la DB
      - MOODLE_DATABASE_HOST=moodle-db
      - MOODLE_DATABASE_PORT_NUMBER=5432
      - MOODLE_DATABASE_USER=bn_moodle
      - MOODLE_DATABASE_PASSWORD=tu_password_segura # ¡DEBE COINCIDIR!
      - MOODLE_DATABASE_NAME=bitnami_moodle
      # Opcional: Configuración del sitio
      - MOODLE_HOST=localhost
      - MOODLE_PASSWORD=admin_password # ¡CAMBIA ESTO!
    volumes:
      # Persistencia de datos: almacena la configuración y archivos de Moodle
      - ./data/moodle:/bitnami/moodle
⚠️ IMPORTANTE: Cambia las líneas que dicen tu_password_segura y admin_password por contraseñas robustas.

🚀 Paso 3: Desplegar Moodle

Una vez guardado el archivo `docker-compose.yml`, despliega toda la aplicación con un solo comando:

```bash
$ sudo docker compose up -d

Explicación de los parámetros:

up: Crea y arranca todos los servicios definidos en el archivo.

-d: Ejecuta los contenedores en modo detached (segundo plano).

Docker Compose descargará las imágenes necesarias y creará los contenedores, conectándolos automáticamente a través de la red interna de Docker.

🌐 Paso 4: Acceder a Moodle
Una vez que los contenedores estén activos (esto puede tardar unos minutos la primera vez que Moodle inicializa la base de datos), puedes acceder a tu sistema LMS.

Verifica el estado de los contenedores:

$ docker compose ps
Asegúrate de que ambos contenedores (moodle_db y moodle_app) tengan el estado running ✅.

Luego, abre tu navegador y visita:

http://localhost:8080