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

### nginx
Ficheros de configuración de Kubernetes para el deployment básico de `nginx`, durante el workshop los utilizaremos para explicar el deployment básico de aplicaciones en Kubernetes y sobre cómo dar conectividad a una aplicación en Kubernetes.

### wildfly
Ficheros de configuracicón de Kubernetes para el deployment de nuestra aplicación Java 11 de ejemplo. Así como el código fuente de la aplicación.

### jersey-netty
Ficheros de configuracicón de Kubernetes para el deployment de nuestra aplicación Java 11 de ejemplo. Así como el código fuente de la aplicación.


## Workshop

El workshop comenzará con una charla de introducción sobre casos de uso de Kubernetes y una introducción a los conceptos de Kubernetes, para despues continuar con los siguientes pasos prácticos.

### Arranque de la máquina virtual

1.- Crear una nueva máquina virtuan en VirtualBox

![Crear máquina virtual](https://i.imgur.com/WeQ0GPQ.png)

2.- Establecer 4096Mb de RAM (si es posible)

![Setear la configuración de memoria](https://i.imgur.com/Zn97vJO.png)

3.- Añadir como volumen la imagen del workshop. 

![Añadir el disco del workshop](https://i.imgur.com/ryNukeR.png)

4.- Añadir una interfaz de #Host Network# (para poder conectar por SSH)

> IMPORTANTE - Activar la opción de DHCP

![Host Network](https://i.imgur.com/YvAlAHH.png)
![Host Network - interfaz](https://i.imgur.com/0QH9QIa.png)

5.- Modificar la configuración de Red de Segundo adaptador para establecer nuestro Host Network

![Adaptador 1](https://i.imgur.com/oUjeiy0.png)
![Adaptador 2](https://i.imgur.com/D5zQRa8.png)

6.- Arrancar la máquina virtual, hacer login y ifconfig para obtener la IP del adaptador

Usuario: kubemotion
Password: kubemotion

![ifconfig](https://i.imgur.com/c2kwVIj.png)


7.- Hacer ssh desde tu ordenador

ssh kubemotion@<ip_obtenida_ifconfig>

#### Introducción a Docker 

```bash
# Iniciar nginx con docker 
docker run --name docker-nginx -p 8002:80 nginx     

# Iniciar nginx con docker daemonizado
docker run --name docker-nginx-daemon -p 8002:80 -d nginx     
curl localhost:8002

# Obtener logs de un contenedor en marcha
docker logs -f docker-nginx-daemon 

# Matar gracefully un contenedor
docker stop f3

# Obtener un listado de los contenedores que han sido apagados
docker ps -a | grep docker-nginx-daemon

# Listar las imagenes de docker
docker images

# Eliminar una imagen de docker
docker rmi <url> 
```

#### Introducción a Kubernetes 

```bash
# Listar los nodos de un cluster
kubectl get nodes

# Describir un nodo del cluster
kubectl describe node localhost.localdomain

# Listar los pods corriendo en un cluster
kubectl get po --all-namespaces
```

#### Deployment de ejemplo (nginx) en Kubernetes

```bash
# Abrir una nueva sesión de ssh con un watch para ver los pods running
watch kubectl get po

# Deployar nginx en el cluster
kubectl create -f nginx/deployment.yml

# Describir las características de un pod running
kubectl describe po <nombre_pod>

# Deployar un servicio de kubernetes para dar conectividad al pod
kubectl create -f nginx/service.yml

# Describir el servicio para el puerto en el nodo
kubectl describe svc nginx

# Verificar la conectividad de nginx
curl 127.0.0.1:<nodeport>

# Escalar un deployment
kubectl scale deployments/nginx --replicas=5

# Eliminar un deployment
kubectl delete deployment/nginx
kubectl delete -f nginx/deployment.yml

# Eliminar un servicio
kubectl delete service/nginx
kubectl delete -f nginx/service.yml
```

#### Deployment de una aplicación Java en Kubernetes

```bash
docker run -p 8080:8080 -p 9990:9990 -it jboss/wildfly /opt/jboss/wildfly/bin/standalone.sh 

# Deployar wildfly en el cluster
kubectl create -f wildfly/deployment.yml

# Describir las características de un pod running
kubectl describe po <nombre_pod>

# Deployar un servicio de kubernetes para dar conectividad al pod
kubectl create -f wildfly/service.yml

# Describir el servicio para el puerto en el nodo
kubectl describe svc wildfly

# Verificar la conectividad de wildfly
curl 127.0.0.1:<nodeport>

# Escalar un deployment
kubectl scale deployments/wildfly --replicas=5

# Eliminar un deployment
kubectl delete deployment/wildfly
kubectl delete -f wildfly/deployment.yml

# Eliminar un servicio
kubectl delete service/wildfly
kubectl delete -f wildfly/service.yml
```

## Orquestación de aplicaciones

Terminaremos el workshop con una charla sobre la diferencia de los pasos que hemos realizado y lo necesario para la orquestación de aplicaciones en producción.
