Lab 3-4 — Procesamiento de Datos con Apache Spark

Este laboratorio combina los contenidos del Lab 3 y Lab 4 en un solo flujo práctico utilizando Apache Spark, tanto en AWS EMR como en Google Colab.

1. Objetivos del Lab 3-4

Comprender el funcionamiento de Spark en modo distribuido y local

Leer datos desde HDFS, S3 y Google Drive

Consultar información usando DataFrames y Spark SQL

Realizar operaciones de transformación y filtrado

Manejar RDDs para procesamiento de texto

Guardar resultados en formato Parquet

Ejecutar el flujo completo en EMR y Colab

2. Contenidos del Lab 3-4
2.1 Lectura de datos (CSV) con Spark

Se cargaron datasets como:

airlines.csv

hdi-data.csv

gutenberg-small (archivos .txt)

Ejemplo de lectura:

df = spark.read.csv(".../datasets/airlines.csv", header=True, inferSchema=True)
df.show(5)

2.2 Transformaciones y consultas

Se aplicaron operaciones como:

select()

filter()

groupBy()

count()

orderBy()

Ejemplo:

df_clean = df.filter(df.rating > 0)
df_clean.show(5)

2.3 Spark SQL

Se creó una vista temporal para consultas SQL:

df.createOrReplaceTempView("air")
spark.sql("SELECT airline, rating FROM air WHERE rating > 5").show()

2.4 Procesamiento con RDDs

Se realizó un conteo de palabras:

rdd = sc.textFile("hdfs:///user/hadoop/datasets/gutenberg-small/*")
wc = (rdd.flatMap(lambda line: line.split(" "))
          .map(lambda word: (word, 1))
          .reduceByKey(lambda a,b: a+b)
          .sortBy(lambda x: x[1], ascending=False))

2.5 Escritura en formato Parquet
df.write.mode("overwrite").parquet(
    "/content/drive/MyDrive/datasets/output/airlines_parquet"
)


Se generaron correctamente los archivos Parquet y el archivo _SUCCESS.

3. Ejecución del Laboratorio

El laboratorio se ejecutó en dos entornos:

a) AWS EMR

Lectura desde HDFS

Ejecución de consultas con DataFrames y SQL

Procesamiento de RDDs

b) Google Colab

Configuración manual de Spark

Lectura de datasets almacenados en Google Drive

Escritura final en formato Parquet

4. Resultados Principales

Lectura correcta de los datasets airlines.csv, hdi-data.csv y gutenberg-small

Limpieza y filtrado de datos

Análisis descriptivo básico

Conteo de palabras usando RDDs

Generación de archivos Parquet en Google Drive

Verificación de la ejecución exitosa mediante capturas de pantalla

5. Conclusión

El Lab 3-4 permitió integrar diversas operaciones fundamentales de Apache Spark, reforzando el uso de DataFrames, SQL y RDDs, así como la lectura y escritura de datos en distintos formatos y entornos de ejecución.