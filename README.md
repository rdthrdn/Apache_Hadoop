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

# 1. Hadoop

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

# 3. Apache Hive

## Tugas:

- Buat database movielens.
- Buat tabel ratings (user_id INT, item_id INT, rating INT, timestamp BIGINT).
- Load data dari file u.data.
- Hitung rata-rata rating setiap film.
- Ambil 10 film dengan rata-rata rating tertinggi.

