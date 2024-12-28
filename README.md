✅️ DockerGHA3 ( Docker, GitHub Actions, Java Spring Boot y Dokku )

Este proyecto demuestra cómo integrar Docker, GitHub Actions, Java Spring Boot y Dokku para crear un flujo de trabajo de desarrollo y despliegue automatizado. La aplicación es un servicio básico de Spring Boot que se ejecuta dentro de un contenedor Docker, se automatiza con GitHub Actions y se despliega usando Dokku.

🟥 Características

Docker: Empaqueta la aplicación Spring Boot en un contenedor para garantizar que se ejecute de la misma manera en cualquier entorno.

GitHub Actions: Automatiza el proceso de construcción, prueba y despliegue de la aplicación con cada cambio en el repositorio.

Java Spring Boot: Framework backend para el desarrollo de la aplicación web.

Dokku: Plataforma de despliegue similar a Heroku que usa contenedores Docker para gestionar aplicaciones de forma sencilla.

🟧 Estructura del Proyecto
```
DockerGHA3/
│
├── .github/
│   └── workflows/
│       └── main.yml             # Flujo de trabajo de GitHub Actions
├── Dockerfile                   # Dockerfile para contenerizar la app
├── README.md                    # Documentación del proyecto
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └──  DominioApplication.java   # Clase principal de Spring Boot
│   │   └── resources/
│   │       └── application.properties         # Configuración de la app
├── target/                       # Directorio de build (generado por Maven/Gradle)
├── pom.xml                       # Archivo de configuración de Maven (si usas Maven)
└── .gitignore                    # Archivos y directorios que Git debe ignorar
```
Dockerfile: Archivo que define cómo crear la imagen Docker para el proyecto Spring Boot.

main.yml: Archivo de configuración para GitHub Actions que automatiza la construcción, pruebas y despliegue.

src/: Contiene el código fuente de la aplicación Spring Boot.

pom.xml: Archivo de configuración de Maven para las dependencias y construcción del proyecto.

dokku-deploy.sh: Script que automatiza el proceso de despliegue de la aplicación en un servidor remoto usando Dokku.

🟨 Instalación

Requisitos

Docker: Necesario para construir y ejecutar la aplicación en contenedores.

GitHub Actions: Se utiliza para la integración continua y automatización de procesos de despliegue.

Dokku: Necesitas un servidor remoto con Dokku instalado para gestionar el despliegue.

Java: Debes tener instalado Java y Maven para desarrollar la aplicación de backend con Spring Boot.

⬜️ Código

Dockerfile
```
# syntax=docker/dockerfile:1
FROM busybox:latest
COPY --chmod=755 <<EOF /app/run.sh
#!/bin/sh
while true; do
  echo -ne "The time is now $(date +%T)\\r"
  sleep 1
done
EOF

ENTRYPOINT /app/run.sh
```

Este Dockerfile crea una imagen basada en busybox que ejecuta un script en un bucle infinito para mostrar la hora actual en tiempo real.

1. Base de la imagen: Utiliza busybox:latest, una imagen minimalista de Unix.

2. Copia el script: Copia un script llamado run.sh al contenedor, que:

Imprime la hora actual en formato HH:MM:SS cada segundo, sobrescribiendo la línea anterior.

3. Permisos: El script recibe permisos de ejecución (chmod=755).

4. Punto de entrada: Configura el script run.sh como el punto de entrada del contenedor, lo que significa que se ejecutará automáticamente cuando se inicie el contenedor.

Función:

El contenedor muestra la hora actual en tiempo real, actualizándola cada segundo en la misma línea de la terminal.

Main.yml
```
#
name: Create and publish a Docker image

# Configures this workflow to run every time a change is pushed to the branch called `release`.
on:
  push:
    branches: ['main']

# Defines two custom environment variables for the workflow. These are used for the Container registry domain, and a name for the Docker image that this workflow builds.
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

# There is a single job in this workflow. It's configured to run on the latest available version of Ubuntu.
jobs:
  build-and-push-image:
    runs-on: ubuntu-latest
    # Sets the permissions granted to the `GITHUB_TOKEN` for the actions in this job.
    permissions:
      contents: read
      packages: write
      # 
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      # Uses the `docker/login-action` action to log in to the Container registry registry using the account and password that will publish the packages. Once published, the packages are scoped to the account defined here.
      - name: Log in to the Container registry
        uses: docker/login-action@65b78e6e13532edd9afa3aa52ac7964289d1a9c1
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      # This step uses [docker/metadata-action](https://github.com/docker/metadata-action#about) to extract tags and labels that will be applied to the specified image. The `id` "meta" allows the output of this step to be referenced in a subsequent step. The `images` value provides the base name for the tags and labels.
      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
      # This step uses the `docker/build-push-action` action to build the image, based on your repository's `Dockerfile`. If the build succeeds, it pushes the image to GitHub Packages.
      # It uses the `context` parameter to define the build's context as the set of files located in the specified path. For more information, see "[Usage](https://github.com/docker/build-push-action#usage)" in the README of the `docker/build-push-action` repository.
      # It uses the `tags` and `labels` parameters to tag and label the image with the output from the "meta" step.
      - name: Build and push Docker image
        uses: docker/build-push-action@f2a1d5e99d037542a71f64918e516c093c6f3fc4
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```
Este archivo main.yml define un flujo de trabajo de GitHub Actions para crear y publicar una imagen Docker en un registro de contenedores (GitHub Container Registry) cada vez que se hace un push a la rama main.

1. Trigger (Disparador):

Se ejecuta automáticamente cuando hay un push a la rama main.

2. Variables de entorno:

Define dos variables:

REGISTRY: Dominio del registro de contenedores (ghcr.io).

IMAGE_NAME: Nombre de la imagen Docker basada en el repositorio de GitHub.

3. Job (build-and-push-image):

Se ejecuta en un entorno Ubuntu (ubuntu-latest).

4. Pasos del Job:

Checkout repository: Clona el repositorio en el entorno de GitHub Actions.

Log in to the Container registry: Inicia sesión en el registro de contenedores de GitHub (GitHub Container Registry) utilizando las credenciales almacenadas en el GITHUB_TOKEN.

Extract metadata: Utiliza la acción docker/metadata-action para extraer las etiquetas y etiquetas adicionales para la imagen Docker.

Build and push Docker image: Utiliza la acción docker/build-push-action para construir la imagen Docker con el Dockerfile del repositorio y subirla al registro de contenedores de GitHub. Las etiquetas y las etiquetas adicionales se aplican a la imagen.

Propósito:

Automatizar la construcción y publicación de una imagen Docker en GitHub Container Registry cuando se actualiza la rama main, usando el archivo Dockerfile del repositorio.

🟦 Estado del Proyecto

    ☑️ Terminado.

🟪 Licencia  

Este proyecto no tiene licencia asignada. Al no contar con una licencia explícita, se considera que todos los derechos están reservados. Si deseas usar este proyecto, por favor, contáctame.

🟫 Autores

- Pascua2020 (https://github.com/Pascua2020)
- UTN