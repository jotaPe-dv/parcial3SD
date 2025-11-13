LAB 3 – Hive: Tablas, Consultas, Joins y Wordcount

Curso: ST0263 – Tópicos Especiales en Telemática
Tema: Hive, Tablas Externas, Joins, Wordcount
Plataforma: EMR AWS + HDFS + S3 + Hive + Hue

1. Acceso a HUE

Acceder al nodo master del cluster EMR:

http://<public-dns>:8888
usuario: hadoop
password: ******

2. Archivos utilizados

Ubicaciones en HDFS:

/user/hadoop/datasets/onu/hdi/hdi-data.csv
/user/hadoop/datasets/onu/export/export-data.csv
/user/hadoop/datasets/gutenberg-small/*.txt

3. Creación de Base de Datos
CREATE DATABASE juanpdb;
USE juanpdb;

4. Tablas HDI (manejada y externa)
4.1 Tabla manejada
CREATE TABLE hdi (
 id INT,
 country STRING,
 hdi FLOAT,
 lifeex INT,
 mysch INT,
 eysch INT,
 gni INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE;


Carga de datos:

LOAD DATA INPATH '/user/hadoop/datasets/onu/hdi/hdi-data.csv'
INTO TABLE hdi;

4.2 Tabla externa HDI
CREATE EXTERNAL TABLE hdi_ext (
 id INT,
 country STRING,
 hdi FLOAT,
 lifeex INT,
 mysch INT,
 eysch INT,
 gni INT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/hadoop/datasets/onu/hdi/';

5. Consultas sobre la tabla HDI
SELECT * FROM hdi LIMIT 10;
SELECT country, gni FROM hdi WHERE gni > 2000;
SELECT country, lifeex FROM hdi WHERE lifeex > 80;

6. Creación de tabla EXPO (externa)
CREATE EXTERNAL TABLE expo (
 country STRING,
 expct FLOAT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/user/hadoop/datasets/onu/export/';

7. JOIN entre HDI y EXPO
SELECT  h.country, h.gni, e.expct
FROM hdi h
JOIN expo e
ON (h.country = e.country)
WHERE h.gni > 2000;

8. Wordcount en Hive
8.1 Tabla externa con archivos de Gutenberg
CREATE EXTERNAL TABLE docs (
 line STRING
)
STORED AS TEXTFILE
LOCATION '/user/hadoop/datasets/gutenberg-small/';


Verificación:

SELECT * FROM docs LIMIT 5;

8.2 Wordcount ordenado por palabra
SELECT word, count(1) AS count
FROM (
  SELECT explode(split(line,' ')) AS word
  FROM docs
) w
GROUP BY word
ORDER BY word DESC
LIMIT 10;

8.3 Wordcount ordenado por frecuencia
SELECT word, count(1) AS count
FROM (
  SELECT explode(split(line,' ')) AS word
  FROM docs
) w
GROUP BY word
ORDER BY count DESC
LIMIT 10;

9. Reto: Guardar resultado en una tabla
9.1 Crear tabla
CREATE TABLE wordcount (
 word STRING,
 count INT
)
STORED AS TEXTFILE;

9.2 Insertar resultados
INSERT INTO wordcount
SELECT word, count(1) AS count
FROM (
  SELECT explode(split(line,' ')) AS word
  FROM docs
) w
GROUP BY word;

9.3 Verificación
SELECT * FROM wordcount LIMIT 20;

10. Conclusiones

Se construyeron tablas manejadas y externas en Hive sobre HDFS y S3; se realizaron consultas simples, filtradas y joins; se procesaron archivos de texto mediante funciones de explosión y tokenización, generando un wordcount completo y almacenado. Esto permite validar el uso de Hive como herramienta de análisis SQL sobre datos distribuidos en Hadoop.