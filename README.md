# Laboratorio Hadoop en Docker

Despliegue de un clúster Apache Hadoop 3 como practica evaluativa. Se trabaja con almacenamiento distribuido con HDFS y ejecución de trabajos MapReduce mediante YARN.

## Ficha de arquitectura

| Elemento | Despliegue |
| --- | --- |
| Imagen | `apache/hadoop:3` |
| HDFS | 1 NameNode y 3 DataNodes |
| YARN | 1 ResourceManager y 1 NodeManager |
| Red | Red bridge privada `hadoop-net` |
| Replicación HDFS | 2 réplicas por bloque |
| Tamaño de bloque | 64 MiB |
| Recursos del NodeManager | 2 vCPU y 2048 MiB de memoria |
| Interfaces web | NameNode: [http://localhost:9870](http://localhost:9870); ResourceManager: [http://localhost:8088](http://localhost:8088) |

Los servicios resuelven sus nombres dentro de la red de Compose. HDFS anuncia `hdfs://namenode:8020` y YARN utiliza `resourcemanager` como host del ResourceManager. La configuración detallada está en [`docker/config`](docker/config).

## Requisitos

- Docker instalado y en ejecución.
- Docker Compose v2 (comando `docker compose`).

## Arranque

Desde la carpeta docker:

```bash
docker compose up -d
```

Comprueba el estado de los contenedores:

```bash
docker compose ps
```

Consulta las interfaces web en [NameNode](http://localhost:9870) y [ResourceManager](http://localhost:8088).

Para detener y retirar los contenedores:

```bash
docker compose down
```

El Compose no declara volúmenes persistentes; al retirar los contenedores con `down`, los datos guardados dentro de ellos no se conservan.

## Contenido del repositorio

- `docker/docker-compose.yml`: definición de servicios y red.
- `docker/config`: propiedades de Hadoop inyectadas en los contenedores.
- `informe/`: memoria del laboratorio.
- `evidencias/`: capturas de la interfaz y pruebas del clúster.

## Autoría

**Alejandro Pérez Ferri** (AlexPF26)
