# Despliegue Moodle Docker

Actividad 4 de la 2 unidad DOCKER

## 🧩 Paso 1: Instalar y Configurar Docker Compose

Asegúrate de tener instalado **Docker Desktop** en tu **Maquina**.  
> ⚙️ *Docker Desktop incluye automáticamente Docker Compose, por lo que probablemente ya esté listo para usar.*

Puedes verificar la instalación abriendo la **Terminal** y ejecutando el siguiente comando:

`$ docker compose version`

## 🧱 Paso 2: Crear el Archivo `docker-compose.yml

Este archivo define todos los **servicios necesarios para ejecutar Moodle**, incluyendo:
- 🗄️ La base de datos (**PostgreSQL**, recomendada para Moodle).
- 🌐 El propio servidor de **Moodle**.

Primero, crea un directorio para tu proyecto y navega hacia él:


`$mkdir moodle-lms`
`$cd moodle-lms`

Luego, crea un archivo llamado docker-compose.yml y ábrelo con tu editor de texto preferido (por ejemplo, nano o vim):

`$nano docker-compose.yml`

En este archivo agregarás la configuración de los contenedores de Moodle y PostgreSQL.

Usaremos la imagen oficial de PostgreSQL para la base de datos y la imagen oficial de Bitnami para Moodle, ya que es una de las más confiables y actualizadas.

IMPORTANTE: Cambia las líneas que dicen tu_password_segura y admin_password por contraseñas robustas.

## 🚀 Paso 3: Desplegar Moodle

Una vez guardado el archivo `docker-compose.yml`, despliega toda la aplicación con un solo comando:

`$ sudo docker compose up -d`

Explicación de los parámetros:

up: Crea y arranca todos los servicios definidos en el archivo.

-d: Ejecuta los contenedores en modo detached (segundo plano).

Docker Compose descargará las imágenes necesarias y creará los contenedores, conectándolos automáticamente a través de la red interna de Docker.

## 🌐 Paso 4: Acceder a Moodle
Una vez que los contenedores estén activos (esto puede tardar unos minutos la primera vez que Moodle inicializa la base de datos), puedes acceder a tu sistema LMS.

Verifica el estado de los contenedores:

`$ docker compose ps`
Asegúrate de que ambos contenedores (moodle_db y moodle_app) tengan el estado running ✅.

Luego, abre tu navegador y visita:

http://localhost:8080