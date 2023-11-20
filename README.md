# Lab8Web
Nama : Anggita Risqi Nur Clarita

NIM : 312210450

Kelas : TI.22.A.4

## DAFTAR ISI <br>
| No | Description | Link |
|-----|------|-----|
|1|Modul Praktikum 8|[Click Here](https://drive.google.com/file/d/1s-uOT39YPG008yviVRTGLBYXR6wh9onp/view)|
|2|Instruksi Praktikum|[Click Here](#instruksi-praktikum)|
|3|Langkah-langkah Praktikum|[Click Here](#langkah-langkah-praktikum)|
|4|Database Studi Kasus Data Barang|[Click Here](#membuat-database-studi-kasus-data-barang)|
|5|Membuat Program CRUD|[Click Here](#membuat-program-crud)|

## Praktikum
> ### Instruksi Praktikum
> 1. Persiapkan text editor misalnya **VSCode**.
>
> 2. Buat folder baru dengan nama **lab8_php_database** pada docroot webserver **(htdocs)**
>
> 3. Ikuti langkah-langkah praktikum yang akan dijelaskan berikutnya.

## Langkah-langkah Praktikum
1. **Persiapan**

    Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah database server menggunakan MySQL. Pastikan **MySQL Server** sudah dapat dijalankan melalui **XAMPP**.

2. **Menjalankan MySQL Server**

    Untuk menjalankan MySQL Server dari menu XAMPP Control.

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/1.png)

3. **Mengakses MySQL Client menggunakan PHP MyAdmin**
    
    Pastikan web server **Apache** dan **MySQL server** sudah dijalankan. Kemudian buka melalui browser: http://localhost/phpmyadmin/


### Membuat Database: Studi Kasus Data Barang

1. **Membuat Database**
    
    Bisa menggunakan script ini apabila membuat di Command Prompt:

    ```sql
    CREATE DATABASE latihan1;
    ```

    Atau bisa membuat database langsung seperti ini: 

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/2.png)

2. **Membuat Tabel**

    ```sql
    CREATE TABLE data_barang (
        id_barang int(10) auto_increment Primary Key,
        kategori varchar(30),
        nama varchar(30),
        gambar varchar(100),
        harga_beli decimal(10,0),
        harga_jual decimal(10,0),
        stok int(4)
    );
    ```

3. **Menambah Data**

    ```sql
    INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
    VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
           ('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
           ('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
    ```

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/3.png)


    **Output apabila success :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/4.png)


    **Output tampilan data:**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/5.png)


### Membuat Program CRUD

Buat folder **lab8_php_database** pada root directory web server **(c:\xampp\htdocs)**

![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/6.png)

Kemudian untuk mengakses directory tersebut pada web server dengan mengakses URL: http://localhost/lab8_php_database/

1. **Membuat file koneksi database**
    
    Buat file baru dengan nama **koneksi.php**
    
    ```php
    <?php
    $host = "localhost";
    $user = "root";
    $pass = "";
    $db = "latihan1";

    $conn = mysqli_connect($host, $user, $pass, $db);
    if ($conn == false)
    {
        echo "Koneksi ke server gagal.";
        die();
    }   else echo "Koneksi berhasil";
    ?>
    ```

    **Output :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/7.png)

    **Penjelasan :** Variabel `host` berisi alamat server MySQL, variabel `user` berisi nama pengguna database, variabel `pass` berisi password database, dan variabel `db` berisi nama database. Jika koneksi gagal, maka akan menampilkan pesan kesalahan dan menghentikan script. Jika koneksi berhasil, maka akan menampilkan pesan "Koneksi berhasil".


2. **Membuat file index untuk menampilkan data (*Read*)**

    Buat file baru dengan nama **index.php**

    ```php
    <?php
    include("koneksi.php");

    // query untuk menampilkan data
    $sql = 'SELECT * FROM data_barang';
    $result = mysqli_query($conn, $sql);

    ?>
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Data Barang</title>
        <link rel="stylesheet" href="style.css">
    </head>

    <body>
        <div class="container">
            <h1>Data Barang</h1>
            <div class="main">
                <a href="tambah.php">Tambah Barang</a>
                <table>
                    <tr>
                        <th>Gambar</th>
                        <th>Nama Barang</th>
                        <th>Kategori</th>
                        <th>Harga Jual</th>
                        <th>Harga Beli</th>
                        <th>Stok</th>
                        <th>Aksi</th>
                    </tr>
                    <?php if ($result) : ?>
                        <?php while ($row = mysqli_fetch_array($result)) : ?>
                            <tr>
                                <td><img src="gambar/<?= $row['gambar']; ?>" alt="<?=$row['nama']; ?>"></td>
                                <td><?= $row['nama']; ?></td>
                                <td><?= $row['kategori']; ?></td>
                                <td><?= $row['harga_beli']; ?></td>
                                <td><?= $row['harga_jual']; ?></td>
                                <td><?= $row['stok']; ?></td>
                                <td>
                                    <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a>
                                    <a href="hapus.php?id=<?= $row['id_barang']; ?>">Hapus</a>
                                </td>
                            </tr>
                        <?php endwhile; else : ?>
                        <tr>
                            <td colspan="7">Belum ada data</td>
                        </tr>
                    <?php endif; ?>
                </table>
            </div>
        </div>
    </body>
    </html>
    ```

    **Output :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/8.png)

    **Keterangan :** Untuk mengakses hasilnya melalui URL: http://localhost/lab8_php_database/index.php

3. **Menambah Data (*Create*)**

    Buat file baru dengan nama **tambah.php**

    ```php
    <?php
    error_reporting(E_ALL);
    include_once 'koneksi.php';

    if (isset($_POST['submit']))
    {
        $nama = $_POST['nama'];
        $kategori = $_POST['kategori'];
        $harga_jual = $_POST['harga_jual'];
        $harga_beli = $_POST['harga_beli'];
        $stok = $_POST['stok'];
        $file_gambar = $_FILES['file_gambar'];
        $gambar = null;
        if ($file_gambar['error'] == 0)
        {
            $filename = str_replace(' ', '_',$file_gambar['name']);
            $destination = dirname(__FILE__) .'/gambar/' . $filename;
            if(move_uploaded_file($file_gambar['tmp_name'], $destination))
            {
                $gambar = 'gambar/' . $filename;;
            }
        }
        $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, stok, gambar) ';
        $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}','{$harga_beli}', '{$stok}', '{$gambar}')";
        $result = mysqli_query($conn, $sql);
        header('location: index.php'); 
    }
    ?>
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Tambah Barang</title>
        <link rel="stylesheet" href="style.css">
    </head>

    <body>
        <div class="container">
            <h1>Tambah Barang</h1>
            <div class="main">
                <form method="post" action="tambah.php" enctype="multipart/form-data">
                    <div class="input">
                        <label> Nama Barang</label>
                        <input type="text" name="nama"/>
                    </div>
                    <div class="input">
                        <label>Kategori</label>
                        <select name="kategori">
                            <option value="Komputer">Komputer</option>
                            <option value="Elektronik">Elektronik</option>
                            <option value="HandPhone">HandPhone</option>
                        </select>
                    </div>
                    <div class="input">
                        <label>Harga Jual</label>
                        <input type="text" name="harga_jual" />
                    </div>
                    <div class="input">
                        <label>Harga Beli</label>
                        <input type="text" name="harga_beli" />
                    </div>
                    <div class="input">
                        <label>Stok</label>
                        <input type="text" name="stok" />
                    </div>
                    <div class="input">
                        <label>File Gambar</label>
                        <input type="file" name="file_gambar" />
                    </div>
                    <div class="submit">
                        <input type="submit" name="submit" value="Simpan" />
                    </div>
                </form>
            </div>
        </div>
    </body>
    </html>
    ```

    **Output :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/9.png)

    **Output apabila data berhasil ditambahkan :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/10.png)

4. **Mengubah Data (*Update*)**

    Buat file baru dengan nama **ubah.php**

    ```php
    <?php
    error_reporting(E_ALL);
    include_once 'koneksi.php';

    if (isset($_POST['submit']))
    {
        $id = $_POST['id'];
        $nama = $_POST['nama'];
        $kategori = $_POST['kategori'];
        $harga_jual = $_POST['harga_jual'];
        $harga_beli = $_POST['harga_beli'];
        $stok = $_POST['stok'];
        $file_gambar = $_FILES['file_gambar'];
        $gambar = null;

        if ($file_gambar['error'] == 0)
        {
            $filename = str_replace(' ', '_', $file_gambar['name']);
            $destination = dirname(__FILE__) . '/gambar/' . $filename;
            if (move_uploaded_file($file_gambar['tmp_name'], $destination))
            {
                $gambar = 'gambar/' . $filename;;
            }    
        }

        $sql = 'UPDATE data_barang SET ';
        $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
        $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
        if (!empty($gambar))
            $sql .= ", gambar = '{$gambar}' ";
        $sql .= "WHERE id_barang = '{$id}'";
        $result = mysqli_query($conn, $sql);   
        
        header('location: index.php');
    }

    $id = $_GET['id'];
    $sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);
    if (!$result) die('Error: Data tidak tersedia');
    $data = mysqli_fetch_array($result);

    function is_select($var, $val) {
        if ($var == $val) return 'selected="selected"';
        return false;
    }

    ?>
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Ubah Barang</title>
        <link rel="stylesheet" href="style.css">
    </head>

    <body>
        <div class="container">
            <h1>Ubah Barang</h1>
            <div class="main">
                <form method="post" action="ubah.php" enctype="multipart/form-data">
                <div class="input">
                    <label>Nama Barang</label>
                    <input type="text" name="nama" value="<?php echo $data['nama'];?>" />
                </div>
                    <div class="input">
                        <label>Kategori</label>
                        <select name="kategori">
                            <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                            <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                            <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                        </select>
                    </div>
                    <div class="input">
                        <label>Harga Jual</label>
                        <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" />
                    </div>
                    <div class="input">
                        <label>Harga Beli</label>
                        <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" />
                    </div>
                    <div class="input">
                        <label>Stok</label>
                        <input type="text" name="stok" value="<?php echo $data['stok'];?>" />
                    </div>
                    <div class="input">
                        <label>File Gambar</label>
                        <input type="file" name="file_gambar" />
                    </div>
                    <div class="submit">
                        <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                        <input type="submit" name="submit" value="Simpan" />
                    </div>
                </form> 
            </div>
        </div>
    </body>
    </html>
    ```

    **Output :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/11.png)

    **Output apabila data berhasil diubah :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/12.png)

5. **Menghapus Data (*Delete*)**

    Buat file baru dengan nama **hapus.php**

    **Script PHP**
    ```php
    <?php
    include_once 'koneksi.php';
    $id = $_GET['id'];
    $sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
    ?>
    ```

    **Output :**

    ![image](https://github.com/AnggitaRisqiNC/Lab8Web/blob/main/screenshot/13.png)

    **Penjelasan :** Untuk menghapus data, klik tombol "Hapus". Data akan dihapus secara definitif oleh program.

## Finish