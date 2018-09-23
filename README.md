# Tugas 2 BDT
## Partisi Basis Data
### 05111540000118 - Muhammad Faris Didin Andiyar

## Outline
* **Deskripsi server**
* **Implementasi Partisi 1 : Sakila DB**
    * Deskripsi Dataset
    * Proses pembuatan partisi
    * Benchmarking
* **Implementasi Partisi 2 : Measures dataset**
    * Deskripsi Dataset
    * Proses pembuatan partisi
    * Benchmarking

## Deskripsi Server yang digunakan
* **Sistem Operasi** : Ubuntu Server 16.04.5
* **Versi Mysql** : Mysql 5.7.23
* **RAM** : 1024 MB
* **CPU** : 1 core

## Implementasi Partisi 1 : Sakila DB
### Deskripsi dataset
* Dataset ini terdiri dari 23 Tabel.
* Masing - masing tabel memiliki jumlah baris data sebagai berikut :
    
    | Nama Tabel | Jumlah Data|
    | ---------- | ---------- |
    | payment                    |      16049 |
    | rental                     |      16044 |
    | film_actor                 |       5462 |
    | inventory                  |       4581 |
    | film                       |       1000 |
    | film_text                  |       1000 |
    | film_category              |       1000 |
    | address                    |        603 |
    | city                       |        600 |
    | customer                   |        599 |
    | actor                      |        200 |
    | country                    |        109 |
    | category                   |         16 |
    | language                   |          6 |
    | store                      |          2 |
    | staff                      |          2 |
    | nicer_but_slower_film_list |       NULL |
    | customer_list              |       NULL |
    | staff_list                 |       NULL |
    | actor_info                 |       NULL |
    | film_list                  |       NULL |
    | sales_by_store             |       NULL |
    | sales_by_film_category     |       NULL |

### Proses pembuatan partisi
* Pemilihan tabel yang akan dipartisi

    Pemilihan tabel yang akan dipatisi ditentukan berdasarkan jumlah data terbanyak dari keseluruhan tabel serta kemungkinan akan bertambahnya data secara signifikan. Dari tabel sakila tabel yang akan dipartisi adalah tabel payment dan tabel rental.

* Daftar tabel yang akan dipartisi adalah:
    * Tabel payment.
    * Tabel rental.

#### Tabel payment
* Jenis partisi yang digunakan adalah : HASH
* Karena menggunakan partisi HASH maka tidask perlu menentukan predikat partisi.
* Tabel akan dibagi kedalam 5 partisi yakni :
    1. p0
    2. p1
    3. p2
    4. p3
    5. p4

#### Implementasi Partisi

Script yang digunakan untuk partisi tabel payment.

```SQL
CREATE TABLE payment (
  payment_id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
  customer_id SMALLINT UNSIGNED NOT NULL,
  staff_id TINYINT UNSIGNED NOT NULL,
  rental_id INT DEFAULT NULL,
  amount DECIMAL(5,2) NOT NULL,
  payment_date DATETIME NOT NULL,
  last_update TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY  (payment_id),
  KEY idx_fk_staff_id (staff_id),
  KEY idx_fk_customer_id (customer_id)
--  CONSTRAINT fk_payment_rental FOREIGN KEY (rental_id) REFERENCES rental (rental_id) ON DELETE SET NULL ON UPDATE CASCADE,
--  CONSTRAINT fk_payment_customer FOREIGN KEY (customer_id) REFERENCES customer (customer_id) ON DELETE RESTRICT ON UPDATE CASCADE,
--  CONSTRAINT fk_payment_staff FOREIGN KEY (staff_id) REFERENCES staff (staff_id) ON DELETE RESTRICT ON UPDATE CASCADE
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE payment
	PARTITION BY HASH (payment_id)
	PARTITIONS 5;
```

Script yang digunakan untuk partisi tabel rental.

```SQL
CREATE TABLE rental (
  rental_id INT NOT NULL AUTO_INCREMENT,
  rental_date DATETIME NOT NULL,
  inventory_id MEDIUMINT UNSIGNED NOT NULL,
  customer_id SMALLINT UNSIGNED NOT NULL,
  return_date DATETIME DEFAULT NULL,
  staff_id TINYINT UNSIGNED NOT NULL,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (rental_id),
  UNIQUE KEY  (rental_id,staff_id,inventory_id,customer_id),	
  KEY idx_fk_inventory_id (inventory_id),
  KEY idx_fk_customer_id (customer_id),
  KEY idx_fk_staff_id (staff_id)
--  CONSTRAINT fk_rental_staff FOREIGN KEY (staff_id) REFERENCES staff (staff_id) ON DELETE RESTRICT ON UPDATE CASCADE,
--  CONSTRAINT fk_rental_inventory FOREIGN KEY (inventory_id) REFERENCES inventory (inventory_id) ON DELETE RESTRICT ON UPDATE CASCADE,
--  CONSTRAINT fk_rental_customer FOREIGN KEY (customer_id) REFERENCES customer (customer_id) ON DELETE RESTRICT ON UPDATE CASCADE
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE rental 
	PARTITION BY HASH(rental_id)
	PARTITIONS 5;
```


## Implementasi Partisi 2 : measures dataset
### Deskripsi Dataset
* dataset terdiri dari dua tabel yakni :
    * measures
    * measures_partitioned (bentuk partisi dari tabel measures)

* sumber dataset
    http://www.vertabelo.com/blog/technical-articles/everything-you-need-to-know-about-mysql-partitions 

### Import Dataset
1. Download dataset dari yang disebutkan diatas.
2. masuk ke command line mysql 
    ```bash
        mysql -u root -p
    ```
3. buat database bernama measures
    ```sql
        CREATE DATABASE measures;
    ```
4. keluar dari command line mysql.
5. import database yang sudah didownload.
    ```SQL
        mysql -u root -p -D measures < {nama file sql}
    ```
    contoh file sql bernama sample_measure.sql
    ```SQL
        mysql -u root -p -D measures < sample_measure.sql
    ```

### BENCHMARKING

#### SELECT
|No. Pengujian|Tabel Tanpa Partisi | Tabel dengan Partisi|
|---|---|---|
|1|||
|2|||
|3|||
|4|||
|5|||
|6|||
|7|||
|8|||
|9|||
|10|||