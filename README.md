# Lab8web.

**Nama : Maulana Malik Ibrahim**

**NIM : 312410185**

**Kelas : TI 24 A2**

**Mata Kuliah : Pemrograman Web 1**

# Membuat Database: Studi Kasus Data Barang 

**CREATE DATABASE latihan1**


<img width="271" height="595" alt="Cuplikan layar 2025-11-18 184310" src="https://github.com/user-attachments/assets/05fe7ccc-db8f-4440-9dec-a2ebe67bfd77" />



# Membuat Tabel 

Code SQL
```
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

Output


<img width="1895" height="649" alt="Cuplikan layar 2025-11-18 183755" src="https://github.com/user-attachments/assets/93230d80-6e58-44da-bb6b-a82fe97d6795" />




# Menambahkan Data 

Code SQL
```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok) 
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5), 
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5), 
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

Output



<img width="1919" height="662" alt="Cuplikan layar 2025-11-18 183822" src="https://github.com/user-attachments/assets/53a1381c-9eaa-4fd5-a018-47d406d5a6a8" />




# Membuat Program CRUD 

Buat folder lab8_php_database pada root directory web server (d:\xampp\htdocs)


<img width="390" height="315" alt="Cuplikan layar 2025-11-18 185853" src="https://github.com/user-attachments/assets/8148b688-751e-4252-8a2a-c35bcc76afb1" />



**Untuk mengakses direktory tersebut pada web server dengan mengakses URL: http://localhost/lab8_php_database/**


# Membuat file koneksi database 

Buat file baru dengan nama koneksi.php 

Code
```
<?php 
$host = "localhost"; 
$user = "root"; 
$pass = ""; 
$db   = "latihan1"; 
 
$conn = mysqli_connect($host, $user, $pass, $db); 
if ($conn == false) 
{ 
    echo "Koneksi ke server gagal."; 
    die(); 
} else echo "Koneksi berhasil"; 
?>
```

Output

<img width="575" height="181" alt="Cuplikan layar 2025-11-18 190447" src="https://github.com/user-attachments/assets/88b5c13a-16ca-4c2c-a5ed-7c7cc28eb898" />



# Membuat file index untuk menampilkan data

Buat file baru dengan nama index.php

Code
```
<?php
include("koneksi.php");

$sql = "SELECT * FROM data_barang";
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Data Barang</title>
</head>
<body>

<div class="container">
    <h1>Data Barang</h1>
    <a href="tambah.php">Tambah Barang</a><br><br>

    <div class="main">
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

            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><?= $row['gambar']; ?></td>
                <td><?= $row['nama']; ?></td>
                <td><?= $row['kategori']; ?></td>

                <!-- urutan harga sesuai gambar -->
                <td><?= $row['harga_jual']; ?></td>
                <td><?= $row['harga_beli']; ?></td>

                <td><?= $row['stok']; ?></td>

                <td>
                    <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a>
                    &nbsp;
                    <a href="hapus.php?id=<?= $row['id_barang']; ?>">Hapus</a>
                </td>
            </tr>
            <?php endwhile; else: ?>
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

Output




<img width="1920" height="1200" alt="Cuplikan layar 2025-11-18 190105" src="https://github.com/user-attachments/assets/5d3a786f-d470-403a-8337-9e4e369df692" />




# Menambah Data (Create) 

Buat file baru dengan nama tambah.php 

Code 
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $nama        = $_POST['nama'];
    $kategori    = $_POST['kategori'];
    $harga_jual  = $_POST['harga_jual'];
    $harga_beli  = $_POST['harga_beli'];
    $stok        = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];

    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;

        if(move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = $filename;
        }
    }

    $sql = "INSERT INTO data_barang (nama, kategori, harga_jual, harga_beli, stok, gambar)
            VALUES ('{$nama}', '{$kategori}', '{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";

    mysqli_query($conn, $sql);
    header('location: index.php');
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Tambah Barang</title>

<style>
    .input {
        margin-bottom: 12px;
    }
    label {
        display: inline-block;
        width: 120px;
    }
    input[type="text"], select {
        width: 200px;
        padding: 4px;
    }
    .submit input {
        background: blue;
        color: white;
        padding: 6px 16px;
        border: none;
        cursor: pointer;
    }
</style>

</head>
<body>

<div class="container">
    <h1>Tambah Barang</h1>
    <div class="main">
        <form method="post" action="tambah.php" enctype="multipart/form-data">

            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" />
            </div>

            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option value="Komputer">Komputer</option>
                    <option value="Elektronik">Elektronik</option>
                    <option value="Hand Phone">Hand Phone</option>
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

Output





<img width="555" height="395" alt="Screenshot 2025-11-17 221157" src="https://github.com/user-attachments/assets/62a7b180-a376-45d8-b09b-7c45225db979" />







# Mengubah Data (Update) 

Buat file baru dengan nama ubah.php 

Code
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit'])) {

    $id         = $_POST['id'];
    $nama       = $_POST['nama'];
    $kategori   = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok       = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];

    $gambar = null;

    if ($file_gambar['error'] == 0) {
        $filename    = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;

        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = $filename;
        }
    }

    $sql  = "UPDATE data_barang SET ";
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', ";
    $sql .= "stok = '{$stok}' ";

    if (!empty($gambar)) {
        $sql .= ", gambar = '{$gambar}' ";
    }

    $sql .= "WHERE id_barang = '{$id}'";

    mysqli_query($conn, $sql);
    header('location: index.php');
}

$id  = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);

if (!$result) die('Error: Data tidak tersedia');

$data = mysqli_fetch_array($result);

function is_select($val, $current) {
    return ($val == $current) ? 'selected="selected"' : '';
}

?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Ubah Barang</title>

<style>
    .input { margin-bottom: 12px; }
    label { display: inline-block; width: 120px; }
    input[type="text"], select {
        width: 200px;
        padding: 4px;
    }
    .submit input {
        background: blue;
        color: white;
        padding: 6px 16px;
        border: none;
        cursor: pointer;
    }
</style>

</head>
<body>

<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">

        <form method="post" action="ubah.php" enctype="multipart/form-data">

            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?= $data['nama']; ?>" />
            </div>

            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option value="Komputer"   <?= is_select('Komputer', $data['kategori']); ?>>Komputer</option>
                    <option value="Elektronik" <?= is_select('Elektronik', $data['kategori']); ?>>
```

Output





<img width="616" height="412" alt="image" src="https://github.com/user-attachments/assets/c175a48c-ac54-404a-b5b6-dc9e51c67874" />






# Menghapus Data (Delete) 

Buat file baru dengan nama hapus.php 

Code
```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```

Output


<img width="1920" height="1200" alt="Cuplikan layar 2025-11-18 190642" src="https://github.com/user-attachments/assets/f654212c-8a1f-4bc7-a969-be2653485283" />









