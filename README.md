
# Docker

## Comandos básicos

- `systemctl status docker` -> Verifica el estado del servicio Docker.
- `docker run hello-world` -> Ejecuta un contenedor de prueba de Docker.

- `docker images` -> Lista las imágenes locales.
- `docker ps` -> Lista los contenedores en ejecución.
- `docker ps -a` -> Lista todos los contenedores.

## Ejecutar un contenedor

1. Ejecutar un contenedor de Nginx:

    ```bash
    docker run --name web01 -d -p 9080:80 nginx
    ```

    Esto inicia un contenedor llamado `web01` y expone el puerto 80 del contenedor en el puerto 9080 de tu máquina.

2. Listar los contenedores en ejecución:

    ```bash
    docker ps
    ```

3. Inspeccionar el contenedor `web01`:

    ```bash
    docker inspect web01
    ```

4. Acceder al contenedor desde el navegador:

    ```bash
    curl http://172.17.0.2:80
    ```

    O bien, ir al navegador e ingresar `IP:HostPort`.

## Construir una imagen

1. Crear un directorio para las imágenes:

    ```bash
    mkdir images
    cd images/
    ```

2. Crear un Dockerfile:

    ```bash
    vim Dockerfile
    ```

3. Pegar el siguiente contenido en el Dockerfile:

    ```dockerfile
    FROM ubuntu:latest AS BUILD_IMAGE
    RUN apt update && apt install wget unzip -y
    RUN wget https://www.tooplate.com/zip-templates/2128_tween_agency.zip
    RUN unzip 2128_tween_agency.zip && cd 2128_tween_agency && tar -czf tween.tgz * && mv tween.tgz /root/tween.tgz

    FROM ubuntu:latest
    LABEL "project"="Marketing"
    ENV DEBIAN_FRONTEND=noninteractive

    RUN apt update && apt install apache2 git wget -y
    COPY --from=BUILD_IMAGE /root/tween.tgz /var/www/html/
    RUN cd /var/www/html/ && tar xzf tween.tgz
    CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
    VOLUME /var/log/apache2
    WORKDIR /var/www/html/
    EXPOSE 80
    ```

4. Construir la imagen:

    ```bash
    docker build -t tesimg .
    ```

5. Listar las imágenes:

    ```bash
    docker images
    ```

6. Ejecutar un contenedor desde la imagen construida:

    ```bash
    docker run -P -d tesimg
    ```

7. Verificar los contenedores en ejecución:

    ```bash
    docker ps
    ```

8. Acceder al contenedor desde el navegador utilizando la IP y el puerto asignado.

## Limpieza

1. Detener los contenedores:

    ```bash
    docker stop web01 heuristic_hugle
    ```

2. Listar todos los contenedores:

    ```bash
    docker ps -a
    ```

3. Eliminar contenedores:

    ```bash
    docker rm heuristic_hugle web01 competent_gates elastic_ramanujan relaxed_sammet
    ```

4. Limpiar imágenes no usadas:

    ```bash
    docker images
    docker rmi a54ee9c44b3b 6130c26b5558 057d51c0049c 825d55fb6340 12766a6745ee feb5d9fea6a5
    ```
