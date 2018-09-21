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