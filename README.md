# `Tugas Big Data - Hadoop`

| Nama Lengkap         | NRP        |
| -------------------- | ---------- |
| Raditya Hardian Santoso | 5027231033 |

# 1. Apache Hadoop (HDFS)

## Tugas:

- Buat direktori movielens di HDFS.
- Upload file u.data ke direktori tersebut.
- Tampilkan 10 baris pertama dari file.
- Hitung ukuran file di HDFS.

# Penyelesaian

1. Dapatkan Direktori Movielens

```bash
wget https://files.grouplens.org/datasets/movielens/ml-100k.zip
```

2. Unzip File yang masi berupa zip

```bash
unzip ml-100k.zip
```

3. Buat Direktori movielens di HDFS

```bash
hdfs dfs -mkdir /movielens
```

```bash
hdfs dfs -put /ml-100k/u.data /movielens/
```

4. Menampilkan 10 baris pertama dari file.

```bash
hdfs dfs -cat /movielens/u.data | head -n 10
```

![hadoop](https://github.com/user-attachments/assets/f3f30849-6118-4713-8890-6463512ef7cf)


5. Menampilkan ukuran file di HDFS.

```bash
hdfs dfs -du -h /movielens/u.data
```

![hadoop 2](https://github.com/user-attachments/assets/3a7b5a8e-fe53-40b7-aac8-71f7c0e9bec3)


# 2. Apache Pig

## Tugas:

- Load file u.data ke Pig.
- Hitung rata-rata rating per item_id (film).
- Ambil hanya film yang memiliki rating rata-rata ≥ 4.0.
- Simpan hasil akhir ke output/film_favorit.

# Penyelesaian

```bash
# load data
raw_data = LOAD '/movielens/u.data' USING PigStorage('\t')
           AS (user_id:int, item_id:int, rating:int, timestamp:long);

grouped = GROUP raw_data BY item_id;
# hitung rating
avg_rating = FOREACH grouped GENERATE group AS item_id,
             AVG(raw_data.rating) AS avg_rating;
# filter hanya rating >= 4.0
fav_movies = FILTER avg_rating BY avg_rating >= 4.0;

# store ke hdfs
STORE fav_movies INTO '/output/film_favorit' USING PigStorage('\t');

```

1. Menampilkan /output/film_favorit :

```bash
hdfs dfs -ls /output/film_favorit
```

2. Menampilkan hanya film yang memilikki rating >= 4.0

```
hdfs dfs -cat /output/film_favorit/part-r-00000
```

# 3. Apache Hive

## Tugas:

- Buat database movielens.
- Buat tabel ratings (user_id INT, item_id INT, rating INT, timestamp BIGINT).
- Load data dari file u.data.
- Hitung rata-rata rating setiap film.
- Ambil 10 film dengan rata-rata rating tertinggi.

# Penyelesaian

1. Buat database movielens.

```bash
CREATE DATABASE IF NOT EXISTS movielens;
USE movielens;
```
2. Buat tabel ratings (user_id INT, item_id INT, rating INT, timestamp BIGINT), dan Load data dari file u.data, serta menghitung rata2 rating setiap film

```bash
CREATE TABLE IF NOT EXISTS ratings (
  user_id INT,
  item_id INT,
  rating INT,
  'timestamp' BIGINT
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
STORED AS TEXTFILE;
```

3. Ambil 10 film dengan rata-rata rating tertinggi.

```bash
LOAD DATA INPATH '/movielens/u.data' INTO TABLE ratings;

SELECT item_id, AVG(rating) AS avg_rating
FROM ratings
GROUP BY item_id
ORDER BY avg_rating DESC
LIMIT 10;
```

