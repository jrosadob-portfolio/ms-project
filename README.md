## MS_PROJECT
+ Carpeta con el nombre del proyecto, contiene las demas carpetas que son repositorios que forman parte del proyecto.
+ Crear archivo `.gitignore` para excluir:
    ```bash
    **/node_modules/**
    **/dist/**
    # microservicios de backend
    ms-*/
    # infraestructura (When use docker/docker-compose)
    in-*/
    # Database (When use docker/docker-compose)
    db-*/
    # Aplicaciones de Frontend
    app-*/
    # Archivos de configuración: config-develop, config-qa, config-main
    config*/
    # Librerias para Angular
    libraries-ng/
    # Librerias para NestJ
    librerias-nestjs/
    ```
+ Agregando los repositorios del proyecto
    ```bash
    $ git submodule add -f https://github.com/jrosadob-portfolio/auth-ms.git ./ms-auth
    $ git submodule add -f https://github.com/jrosadob-portfolio/gateway.git ./ms-gateway
    $ git submodule add -f https://github.com/jrosadob-portfolio/orders-ms.git ./ms-orders
    $ git submodule add -f https://github.com/jrosadob-portfolio/payments-ms.git ./ms-payments
    $ git submodule add -f https://github.com/jrosadob-portfolio/products-ms.git ./ms-products
    ```
    La opción -f es para poder incluir la carpeta del modulo, debido a que la carpeta está en el `.gitignore`.
+ Alias de Docker compose:
    ```bash
    # Use: docker-compose.develop.yml
    $ dc develop up -d
    $ dc develop down
    $ dc develop --build

    # Use: docker-compose.qa.yml
    $ dc qa up -d
    $ dc qa down
    $ dc qa --build

    # Use: docker-compose.main.yml
    $ dc main up -d
    $ dc main down
    $ dc main --build
    ```
+ Microservices
  + ordesrDb. Postgres:14 Conecction Ok sin tablas
  + ms-auth. Up ok
  + ms-gateway. Up ok. Tested on http://localhost:3000
  + ms-orderes. Problemas con Prisma
  + ms-payments. Up ok. Tested on http://localhost:3003
  + ms-products. Up ok. Use port:3001 Donde se usa
  + nats-server. Up ok. http://localhost:8222

## Notas
+ Las bases de datos deben estar levantadas
+ crear docker-compose.database.yml 


## gcloud
+ Autenticarse dentro del contenedor
  ```bash
  gcloud auth login
  gcloud init
  ```
+ Otros comandos
  ```bash
  gcloud --version
  gcloud 
  ```
+ Configurando docker para usar el registry de GCP
  ```
  $ gcloud auth configure-docker <repositoryUrlGcp>
  $ gcloud auth configure-docker us-east1-docker.pkg.dev
  ```
+ Renombrar imagenes para subirlas al registro
  ```bash
  $ docker tag ms-auth us-east1-docker.pkg.dev/jrosadob-ms-project/docker/ms-auth
  ```
+ Crear la imagen con el nombre del registro
  ```bash
  $ docker build -f dockerfile.prod -t us-east1-docker.pkg.dev/jrosadob-ms-project/docker/ms-auth .
  ```
+ Subir la imagen
  ```bash
  $ docker image push us-east1-docker.pkg.dev/jrosadob-ms-project/docker/ms-auth
  ```
+ gcloud CLI commands
  ``` bash
  # Autenticarse
  $ gcloud auth login

  # Inicializar/configurar gcloud
  $ gcloud init

  # Establecer el proyecto
  $ gcloud config set project jrosadob-ms-project

  # Establecer el repositorio de GCP para que el docker lo identifique
  $ gcloud auth configure-docker us-east1-docker.pkg.dev

  # Establecer una políticas al usuario
  $ gcloud projects add-iam-policy-binding jrosadob-ms-project --member="user:jrosadob.gcp@gmail.com" --role="roles/artifactregistry.admin"
  $ gcloud projects add-iam-policy-binding jrosadob-ms-project --member="user:jrosadob.gcp@gmail.com" --role="roles/artifactregistry.writer"
  ```
+ Nombre del repositorio docker de GCP
  ```bash
  us-east1-docker.pkg.dev/jrosadob-ms-project/docker
  ```
+ Subir imagenes


## MS_AUTH





## Dev

1. Clonar el repositorio
2. Crear un .env basado en el .env.template
3. Ejecutar el comando `git submodule update --init --recursive` para reconstruir los sub-módulos
4. Ejecutar el comando `docker compose up --build`


### Pasos para crear los Git Submodules

1. Crear un nuevo repositorio en GitHub
2. Clonar el repositorio en la máquina local
3. Añadir el submodule, donde `repository_url` es la url del repositorio y `directory_name` es el nombre de la carpeta donde quieres que se guarde el sub-módulo (no debe de existir en el proyecto)
```
git submodule add <repository_url> <directory_name>
```
4. Añadir los cambios al repositorio (git add, git commit, git push)
Ej:
```
git add .
git commit -m "Add submodule"
git push
```
5. Inicializar y actualizar Sub-módulos, cuando alguien clona el repositorio por primera vez, debe de ejecutar el siguiente comando para inicializar y actualizar los sub-módulos
```
git submodule update --init --recursive
```
6. Para actualizar las referencias de los sub-módulos
```
git submodule update --remote
```


## Importante
Si se trabaja en el repositorio que tiene los sub-módulos, **primero actualizar y hacer push** en el sub-módulo y **después** en el repositorio principal. 

Si se hace al revés, se perderán las referencias de los sub-módulos en el repositorio principal y tendremos que resolver conflictos.



# Prod

1. Clonar el repositorio
2. Crear un .env basado en el .env.template
3. Ejecutar el comando
```
docker compose -f docker-compose.prod.yml build
```