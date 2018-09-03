# Orquestación en Kubernetes para desarrolladores Java
Material para el workshop de orquestación de aplicaciones en Kubernetes del evento XantarJ - HackDayHispano - A Coruña - 29 de Septiembre de 2018.

![Java & Kubernetes](https://i.imgur.com/EOgEt5M.png)

## Abstract del workshop

Java 10 viene con muchas características necesarias para ejecutar aplicaciones JVM en Docker. El objetivo de este taller es desplegar una aplicación Java usando Docker & Kubernetes con los ajustes y límites de memoria y CPU adecuados.

En esta sesión, construiremos una imagen de Docker con una aplicación Java basada en el framework Netty. Esta aplicación tendrá un tamaño muy pequeño siguiendo las mejores prácticas de Docker y se aprovechará de Java Platform Module System (JPMS) presentado en la versión JDK 9. Desplegaremos la aplicación en Kubernetes y la escalaremos para demostrar cuán poderosa es hoy en día la JVM en conjunto con Docker y Kubernetes.

Luego expondremos las métricas de aplicaciones y JVM, que serán consumidas por Prometheus, un sistema que registra datos de series temporales (telemetría) para monitorear y alertar, y usaremos Grafana para consultar y generar métricas desde los pods de la aplicación.

Durante todo el workshop, descubriremos los errores más comunes al trabajar con Docker y la JVM y cómo evitarlos.

## Contenido
El contenido de este repositorio se encuentra dividido en diferentes carpetas que continenen `Dockerfiles` Java 11 para realizar las Builds de docker de las aplicaciones de ejem,plo, así como ficheros `yml` con la configuración del despliegue de aplicaciones en Kubernetes.

### jdk-11-docker
Dockerfiles que utilizaremos para realizar las builds de la imagenes de Docker de las aplicaciones Java 11 que deployaremos en el cluster.

### nginx-example
Ficheros de configuración de Kubernetes para el deployment básico de `nginx`, durante el workshop los utilizaremos para explicar el deployment básico de aplicaciones en Kubernetes y sobre cómo dar conectividad a una aplicación en Kubernetes.

### jersey-netty
Ficheros de configuracicón de Kubernetes para el deployment de nuestra aplicación Java 11 de ejemplo. Así como el código fuente de la aplicación.
