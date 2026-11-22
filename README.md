# Laboratorio Hadoop en Docker

Despliegue de un clúster Apache Hadoop 3 para practicar almacenamiento distribuido con HDFS y ejecución de trabajos MapReduce mediante YARN. La infraestructura se define con Docker Compose en `docker/docker-compose.yml`.

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

Desde la raíz del repositorio, descarga la imagen si es necesario y arranca los seis servicios en segundo plano:

```bash
docker compose -f docker/docker-compose.yml up -d
```

Comprueba el estado de los contenedores:

```bash
docker compose -f docker/docker-compose.yml ps
```

Consulta las interfaces web en [NameNode](http://localhost:9870) y [ResourceManager](http://localhost:8088). La inicialización puede tardar unos instantes; si no aparecen activos, revisa los registros:

```bash
docker compose -f docker/docker-compose.yml logs -f
```

Para detener y retirar los contenedores:

```bash
docker compose -f docker/docker-compose.yml down
```

El Compose no declara volúmenes persistentes; al retirar los contenedores con `down`, los datos guardados dentro de ellos no se conservan.

## Contenido del repositorio

- `docker/docker-compose.yml`: definición de servicios y red.
- `docker/config`: propiedades de Hadoop inyectadas en los contenedores.
- `informe/`: memoria del laboratorio.
- `evidencias/`: capturas de la interfaz y pruebas del clúster.

## Autoría

**Alejandro Pérez Ferri** (AlexPF26)