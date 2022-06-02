# Debezium oracle postgres
## Descargar y Descomprimir el servidor oracle dockerizado

* https://drive.google.com/file/d/1I57rzHABegBqtWyuI24OsPABvET3oWxZ/view?usp=sharing

```
sudo tar xf oracle.xz
```

## Descomprimir instantclient en la carpeta debezium-jdbc-es
```
tar xf instantclient_12_2.xz
```
## Arrancar Entorno Debezium y dbs
```
export DEBEZIUM_VERSION=1.3
docker-compose up -d
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

## Verificar la base de datos postgres \dt+ no tendra tablas
```
docker-compose  exec postgres bash -c 'psql -U $POSTGRES_USER $POSTGRES_DB'
```
## Registrar el Destino postgres
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @jdbc-sink.json
```
## Registrar el origen oracle
```
curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @source.json
```
