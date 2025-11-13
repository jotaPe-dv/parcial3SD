LAB 3-2
Análisis de datos con S3, AWS Glue y Amazon Athena

Curso ST0263 – Tópicos Especiales en Telemática
Universidad EAFIT

1. Preparación de los datos en Amazon S3

Bucket utilizado:

s3://jpdatalake2/datasets/


Estructura:

datasets/
 ├── airlines.csv
 ├── clientes.csv
 ├── sample_data.csv
 ├── all-news/
 ├── covid19/
 ├── flights/
 ├── gutenberg-small/
 ├── gutenberg/
 ├── onu/
 ├── onu2/
 ├── otros/
 ├── retail_logs/
 └── spark/


Los datos están organizados y accesibles para Glue y Athena.

2. AWS Glue Data Catalog
2.1. Base de datos

Creada en AWS Glue:

jpdatalake_db

2.2. Crawler

Parámetros:

Data source: S3

Ruta: s3://jpdatalake2/datasets/

IAM Role: AWSGlueServiceRole

Target: Glue Data Catalog

Database: jpdatalake_db

Después de ejecutar el crawler, se generan tablas como:

export
hdi
gutenberg_small

3. Amazon Athena
3.1. Configuración

Base de datos seleccionada:

jpdatalake_db


Ubicación de resultados:

s3://jpdatalake2/athena-results/

4. Consultas SQL en Athena
4.1. Visualización inicial
SELECT * FROM hdi LIMIT 20;

4.2. Filtro por indicador GNI
SELECT country, gni
FROM hdi
WHERE gni > 2000;

4.3. Join entre tablas del Data Catalog
SELECT h.country, h.gni, e.export
FROM hdi h
JOIN export e
    ON h.country = e.country
WHERE h.gni > 2000;

5. Procesamiento de texto: análisis de palabras (WordCount)

Tabla externa creada para textos tipo línea:

CREATE EXTERNAL TABLE gutenberg_texts (
    line string
)
LOCATION 's3://jpdatalake2/datasets/gutenberg-small/';


Consulta WordCount:

WITH words AS (
    SELECT word
    FROM gutenberg_texts
    CROSS JOIN UNNEST(split(line, ' ')) AS t(word)
)
SELECT word, COUNT(*) AS freq
FROM words
GROUP BY word
ORDER BY freq DESC
LIMIT 20;

6. Resultados

Se generaron metadatos en Glue Data Catalog.

Athena ejecutó consultas SQL sobre archivos almacenados en S3.

Se realizaron consultas de exploración, filtros, joins y procesamiento de texto (WordCount).

Se utilizó un flujo serverless sin necesidad de infraestructura Hadoop.