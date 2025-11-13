LAB 3-1 — Gestión de Archivos en HDFS y S3 para Big Data

Curso: ST0263 – Tópicos Especiales en Telemática
Universidad EAFIT – 2025-1
Estudiante Juan Pablo Rua cartagena
Fecha: 7/11/2025

1. Crear y conectar al clúster EMR

Comando SSH usado:

ssh -i "C:\Users\Juan Rua\.ssh\lb.pem" hadoop@13.217.11.135


CAPTURA #1

2. Clonar el repositorio del curso (en el master)
git clone https://github.com/st0263eafit/st0263-252.git


CAPTURA #2

3. Ver estructura inicial de HDFS

Comandos usados:

hdfs dfs -ls /
hdfs dfs -ls /user
hdfs dfs -ls /user/hadoop


CAPTURA #3

4. Crear directorio en HDFS para datasets
hdfs dfs -mkdir /user/hadoop/datasets
hdfs dfs -mkdir /user/hadoop/datasets/gutenberg-small

CAPTURA #4

5. Copiar datasets desde el servidor (local del cluster) → HDFS
hdfs dfs -put ~/st0263-252/bigdata/datasets/gutenberg-small/*.txt \
           /user/hadoop/datasets/gutenberg-small/



CAPTURA #5

6. Comandos básicos de HDFS

Ejecutar:

hdfs dfs -du /user/hadoop/datasets
hdfs dfs -cat /user/hadoop/datasets/gutenberg-small/<shakira.txt>
hdfs dfs -chmod 755 /user/hadoop/datasets/gutenberg-small


CAPTURA #6:


7. Copiar archivos desde HDFS → LOCAL

Crear carpeta:

mkdir ~/mis_datasets


Copiar:

hdfs dfs -get /user/hadoop/datasets/gutenberg-small/*.txt ~/mis_datasets/

CAPTURA #7

8. Copiar dataset LOCAL → S3
aws s3 cp ~/st0263-252/bigdata/datasets/ s3://<tu_bucket>/datasets/ --recursive


CAPTURA #8:

#9, #10, #11. CAPTURAS de HUE (upload, s3, hadoop)