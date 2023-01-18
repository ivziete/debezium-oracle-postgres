# Debezium oracle postgres

En oracle se tiene un esquema llamado debezium que contiene el siguiente script `demobld.sql` del cual la tabla EMP y SALGRADE es la que se sincronizara a postgres,creando la estructura de la tabla y sincronizando los insert, update y delete hechos en esa tabla.

## Descargar y Descomprimir el servidor oracle dockerizado

Clonar este repositorio

```
git clone git@github.com:ivziete/debezium-oracle-postgres.git
cd debezium-oracle-postgres
```

Dentro de esta carpeta descargar el servidor oracle ya configurado

* https://drive.google.com/file/d/1UOyUz3dQT6QdXczv7-HDbZpmohRbXjXd/view?usp=sharing

Descomprimir como usuario root
```
sudo tar xf oracle.xz
```

## Descomprimir instantclient en la carpeta debezium-jdbc-es
```
tar xf instantclient_12_2.xz
```
## Arrancar Entorno Debezium, Oracle y Postgres
```
export DEBEZIUM_VERSION=1.3
docker-compose up -d
```
## Arrancar Entorno apicurio Debezium, Oracle y Postgres
```
export DEBEZIUM_VERSION=1.3
docker-compose -f docker-compose-apicurio.yml up -d
```
## Arrancar Entorno avro Debezium, Oracle y Postgres
```
export DEBEZIUM_VERSION=1.3
docker-compose -f docker-compose-avro.yml up -d
```
## Para ver los logs en otra ventana
```
docker-compose logs -f
```
## Usuario oracle

* Host: localhost:5432
* SID: orcl
* User: debezium 
* Pass: dbz

## Usuario Postgres

* Host: localhost:5432
* DB: inventory
* User: postgresuser
* Pass: postgrespw

## Verificar la base de datos postgres \dt+ no tendra tablas
```
docker-compose  exec postgres bash -c 'psql -U $POSTGRES_USER $POSTGRES_DB'
\dt+
select * from emp 
```
## Registrar el Destino emp postgres
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @jdbc-sink.json
```
## Registrar el Destino salgrade postgres
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @jdbc-sink-salgrade.json
```
## Registrar el origen oracle
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @source.json
```

Verificar nuevamente postgres ya tendra la tabla emp y salgrade, puede usar el esquema `demobld.sql` para referencia.

Pueden ver un demo de como se ve aqui:
* https://www.youtube.com/watch?v=5g6O7GLF35U
