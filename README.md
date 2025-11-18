# Lab8web.

**Nama : Maulana Malik Ibrahim**

**NIM : 312410185**

**Kelas : TI 24 A2**

**Mata Kuliah : Pemrograman Web 1**

# Membuat Database: Studi Kasus Data Barang 

**CREATE DATABASE latihan1**


<img width="271" height="595" alt="Cuplikan layar 2025-11-18 184310" src="https://github.com/user-attachments/assets/0b1f318e-d755-4003-8c00-24b3721d5688" />


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



<img width="1895" height="649" alt="Cuplikan layar 2025-11-18 183755" src="https://github.com/user-attachments/assets/b1e95d38-4210-4a56-a28f-05f7f5956e54" />



# Menambahkan Data 

Code SQL
```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok) 
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5), 
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5), 
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

Output



<img width="1919" height="662" alt="Cuplikan layar 2025-11-18 183822" src="https://github.com/user-attachments/assets/80cc8ce9-493c-49b4-b662-99c0f120065e" />




# Membuat Program CRUD 

Buat folder lab8_php_database pada root directory web server (d:\xampp\htdocs)




<img width="474" height="314" alt="image" src="https://github.com/user-attachments/assets/b85afc5d-025d-4669-b6fd-63218ce3c739" />






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





<img width="764" height="253" alt="Screenshot 2025-11-17 195707" src="https://github.com/user-attachments/assets/9178b065-f1c0-47dc-83cd-3bb4d971608e" />






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





<img width="717" height="367" alt="Screenshot 2025-11-17 200448" src="https://github.com/user-attachments/assets/e6018349-ec70-4d0a-b9c4-5367e2dcf0ce" />







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





<img width="683" height="288" alt="Screenshot 2025-11-17 221410" src="https://github.com/user-attachments/assets/f7202684-9352-4bdf-83db-124794905aa2" />






