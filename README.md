# 🍲Sistem Pengelolaan Food Redistribution🍲
Nama: Regina Jelita Ningsih
<br> NIM: 2509116061
<br> Kelas: B (2025)

## 🍽️ Penjelasan Studi Kasus
Masalah **surplus makanan** sering muncul di hotel, restoran, usaha kuliner, dan juga dalam kegiatan pribadi seperti acara syukuran.
Sementara itu, masih banyak individu dan lembaga sosial seperti panti asuhan atau rumah singgah yang membutuhkan bantuan pangan. 
Program ini dibuat untuk menghubungkan kedua pihak melalui sistem pencatatan yang sederhana dan terstruktur.

Sistem ini mengelola empat entitas utama yang saling berkaitan, sebagai berikut:
1. **Donatur**, yaitu pihak yang memberikan donasi makanan. Donatur dibedakan menjadi dua jenis:
    * **Donatur Individu**, yaitu perseorangan yang berdonasi dari suatu kegiatan pribadi (misalnya acara syukuran, pernikahan, dan sebagainya).
    * **Donatur Instansi**, yaitu badan usaha seperti hotel, restoran, atau usaha kuliner yang menyumbangkan makanan secara rutin.
2. **Donasi**, yaitu data makanan yang didonasikan, mencakup nama makanan, jumlah porsi, dan status kelayakan konsumsi.
3. **Penerima**, yaitu pihak yang menerima penyaluran makanan, juga dibedakan menjadi dua jenis:
    * **Penerima Individu**, perseorangan yang membutuhkan bantuan berupa makanan (misalnya pemulung, pengamen, dan orang tidak mampu lainnya).
    * **Penerima Lembaga**, organisasi sosial seperti panti asuhan atau rumah singgah yang menyalurkan makanan kepada penghuninya.
4. **Penyaluran**, yaitu data aktivitas penyaluran yang menghubungkan sebuah donasi dengan penerima tertentu, lengkap dengan tanggal, jumlah porsi yang disalurkan, dan petugas yang bertanggung jawab.

```
========================================
       FOOD REDISTRIBUTION SYSTEM
========================================
[1] Donatur
[2] Donasi
[3] Penerima
[4] Penyaluran
[5] Keluar
>>
```

## 👾 Struktur Class
<img width="487" height="496" alt="image" src="https://github.com/user-attachments/assets/3c445f9a-2f89-4e15-ad97-0bfd927862ca" />
<br> Program ini disusun dengan _layered architecture_ sederhana, yang terbagi ke dalam empat _packages_:
 
- `model` -> merepresentasikan entitas/objek data (Donatur, Donasi, Penerima, Penyaluran).
- `service` -> menangani logika dan operasi CRUD untuk masing-masing entitas.
- `controller` -> mengatur alur menu utama program.
- `util` -> berisi fungsi bantu (helper), seperti validasi input.
- `main` -> *entry point* program.

```
Donatur (super class)
| DonaturIndividu (sub class)
| DonaturInstansi (sub class)
 
Penerima (super class)
| PenerimaIndividu (sub class)
| PenerimaLembaga (sub class)
```

Selain dua hierarki di atas, ada dua _class_ lain yang berdiri sendiri tanpa pewarisan, yaitu `Donasi`, 
yang menyimpan data makanan yang didonasikan, dan `Penyaluran`, yang menyimpan data transaksi penyaluran dan menghubungkan `Donasi` dengan `Penerima`. 
Hubungan antara keempat entitas ini dijembatani oleh ID seperti `idDonatur`, `idDonasi`, dan `idPenerima`, 
yang saling mereferensikan.

## ⭐ Inheritance 
### Donatur (Superclass)
Kelas `Donatur` adalah kelas dasar yang menyimpan atribut dan perilaku umum yang dimiliki semua jenis donatur, yaitu `idDonatur` dan `namaDonatur`. 
Kelas ini juga punya method `getJenisDonatur()` yang akan di-override oleh kelas turunannya.

```
public class Donatur {
    private int idDonatur;
    private String namaDonatur;

    public Donatur(int idDonatur, String namaDonatur) {
        this.idDonatur = idDonatur;
        this.namaDonatur = namaDonatur;
    }

    public int getIdDonatur() {
        return idDonatur;
    }

    public void setIdDonatur(int idDonatur) {
        this.idDonatur = idDonatur;
    }

    public String getNamaDonatur() {
        return namaDonatur;
    }

    public void setNamaDonatur(String namaDonatur) {
        this.namaDonatur = namaDonatur;
    }

    public String getJenisDonatur() {
        return "Donatur";
    }
}
```

### DonaturIndividu dan DonaturInstansi (Subclass)
Kedua kelas ini menggunakan `extends` agar bisa mewarisi semua atribut dan method dari `Donatur`. 
Dengan begitu, kita tidak perlu lagi menulis ulang `idDonatur`, `namaDonatur`, dan getter-setter-nya. 
Setiap subclass hanya perlu menambah atribut khusus miliknya sendiri.

```
public class DonaturIndividu extends Donatur {
    private String jenisKegiatan;

    public DonaturIndividu(int idDonatur, String namaDonatur, String jenisKegiatan) {
        super(idDonatur, namaDonatur);
        this.jenisKegiatan = jenisKegiatan;
    }

    public String getJenisKegiatan() {
        return jenisKegiatan;
    }

    public void setJenisKegiatan(String jenisKegiatan) {
        this.jenisKegiatan = jenisKegiatan;
    }

    @Override
    public String getJenisDonatur() {
        return "Donatur Individu";
    }
}
```

```
public class DonaturInstansi extends Donatur {
    private String namaInstansi;
    private String jenisInstansi;

    public DonaturInstansi(
            int idDonatur,
            String namaDonatur,
            String namaInstansi,
            String jenisInstansi) {

        super(idDonatur, namaDonatur);
        this.namaInstansi = namaInstansi;
        this.jenisInstansi = jenisInstansi;
    }

    public String getNamaInstansi() {
        return namaInstansi;
    }

    public void setNamaInstansi(String namaInstansi) {
        this.namaInstansi = namaInstansi;
    }

    public String getJenisInstansi() {
        return jenisInstansi;
    }

    public void setJenisInstansi(String jenisInstansi) {
        this.jenisInstansi = jenisInstansi;
    }

    @Override
    public String getJenisDonatur() {
        return "Donatur Instansi";
    }
}
```

### Penerima (Superclass)
Konsep yang sama seperti pada `Donatur` juga digunakan dalam hierarki `Penerima`. 
Kelas `Penerima` memiliki atribut dasar yang dimiliki oleh semua jenis penerima, yaitu `idPenerima`, 
serta method `getJenisPenerima()` yang nantinya akan di-override oleh subclass-nya.

```
public class Penerima {
    private int idPenerima;

    public Penerima(int idPenerima) {
        this.idPenerima = idPenerima;
    }

    public int getIdPenerima() {
        return idPenerima;
    }

    public void setIdPenerima(int idPenerima) {
        this.idPenerima = idPenerima;
    }

    public String getJenisPenerima() {
        return "Penerima";
    }
}
```
### PenerimaIndividu dan PenerimaLembaga (Subclass)
Seperti halnya `DonaturIndividu` dan `DonaturInstansi`, kedua kelas ini memakai `extends` untuk mewarisi `idPenerima` dari `Penerima`. 
Mereka juga memanggil `super(idPenerima)` di constructor, lalu menambahkan atribut khusus masing-masing.

```
public class PenerimaIndividu extends Penerima {
    private String deskripsiPenerima;

    public PenerimaIndividu(int idPenerima, String deskripsiPenerima) {
        super(idPenerima);
        this.deskripsiPenerima = deskripsiPenerima;
    }

    public String getDeskripsiPenerima() {
        return deskripsiPenerima;
    }

    public void setDeskripsiPenerima(String deskripsiPenerima) {
        this.deskripsiPenerima = deskripsiPenerima;
    }

    @Override
    public String getJenisPenerima() {
        return "Individu";
    }
}
```

```
public class PenerimaLembaga extends Penerima {
    private String namaLembaga;
    private String jenisLembaga;
    private String namaPengelola;

    public PenerimaLembaga(
            int idPenerima,
            String namaLembaga,
            String jenisLembaga,
            String namaPengelola) {

        super(idPenerima);
        this.namaLembaga = namaLembaga;
        this.jenisLembaga = jenisLembaga;
        this.namaPengelola = namaPengelola;
    }

    public String getNamaLembaga() {
        return namaLembaga;
    }

    public void setNamaLembaga(String namaLembaga) {
        this.namaLembaga = namaLembaga;
    }

    public String getJenisLembaga() {
        return jenisLembaga;
    }

    public void setJenisLembaga(String jenisLembaga) {
        this.jenisLembaga = jenisLembaga;
    }

    public String getNamaPengelola() {
        return namaPengelola;
    }

    public void setNamaPengelola(String namaPengelola) {
        this.namaPengelola = namaPengelola;
    }

    @Override
    public String getJenisPenerima() {
        return "Lembaga";
    }
}
```

## 🥗 Alur Program
Bagian ini menunjukkan bagaimana program benar-benar terlihat dan berjalan saat dieksekusi di terminal, sehingga pembaca yang belum sempat mencoba sendiri tetap bisa membayangkan alur penggunaannya dari awal hingga akhir. Urutan tangkapan layar di bawah ini disusun mengikuti skenario penggunaan yang wajar: mulai dari program dibuka, menjelajahi tiap menu data, mencoba fitur tambah/ubah/hapus, sampai akhirnya keluar dari program.
 
**1. Tampilan Menu Utama**
<br> Saat program pertama kali dijalankan, akan muncul judul dari sistem beserta lima pilihan menu, mulai dari Donatur, Donasi, Penerima, Penyaluran, dan Keluar.
Cukup input angka sesuai menu yang ingin dituju, lalu menekan Enter. 
Tampilan menu ini akan muncul berulang kali setiap kali pengguna kembali dari salah satu sub-menu, karena disusun dengan struktur perulangan yang baru berhenti ketika pengguna memilih "Keluar".

<img width="610" height="320" alt="image" src="https://github.com/user-attachments/assets/e0be2f9e-4ded-4d95-a2d2-5e41bb0af20e" />


<br> **2. Menu Data Donatur**
<br> Setelah memilih menu “Donatur”, pengguna akan melihat daftar semua donatur yang sudah tercatat, lengkap dengan ID, nama, dan jenisnya. 
Tampilan data bisa berbeda untuk setiap donatur, jika donatur berjenis “Individu”, akan ada baris tambahan “Jenis Kegiatan” (misalnya Acara Syukuran). Untuk donatur “Instansi”, yang muncul adalah “Nama Instansi” dan “Jenis Instansi”. 
Perbedaan tampilan ini menunjukkan konsep inheritance dan instanceof yang sudah dijelaskan sebelumnya. Di bawah daftar, ada sub-menu Tambah, Update, Hapus, dan Keluar untuk mengelola data donatur.

<img width="501" height="895" alt="image" src="https://github.com/user-attachments/assets/683732cb-002c-46cd-9d54-cb6097331ff4" />


<br> **3. Menambah Data Donatur Baru**
<br> _Output_ program memperlihatkan, proses saat pengguna memilih opsi “Tambah” di menu Donatur. 
Program akan meminta ID dan nama donatur terlebih dahulu, lalu menanyakan jenis donatur yang ingin dibuat (Individu atau Instansi) lewat sub-menu. 
Pertanyaan berikutnya akan menyesuaikan pilihan pengguna, jika memilih Individu, program hanya menanyakan “Jenis Kegiatan”; jika memilih Instansi, program menanyakan “Nama Instansi” dan “Jenis Instansi”. 
Setelah semua data diisi, akan muncul pesan konfirmasi bahwa data berhasil ditambahkan, dan data baru langsung muncul di daftar Donatur.

<img width="502" height="567" alt="image" src="https://github.com/user-attachments/assets/8d600a93-5ab2-47a9-bb5b-69756c2d7402" />
<img width="492" height="602" alt="image" src="https://github.com/user-attachments/assets/456ebe61-d36d-4b5c-8c00-a4b16aed7e5d" />


<br> **4. Menu Data Donasi**
<br> Menu ini menampilkan semua data donasi makanan yang sudah tercatat, termasuk ID Donasi, ID Donatur, nama makanan, jumlah porsi, dan status kelayakan konsumsi. 
Kolom “ID Donatur” penting karena menunjukkan hubungan antara data donasi dan donatur, meskipun nama donatur belum langsung ditampilkan di tabel Donasi. 
Seperti pada menu Donatur sebelumnya, di bawah tabel juga ada sub-menu Tambah, Update, dan Hapus untuk mengelola data donasi.

<img width="500" height="920" alt="image" src="https://github.com/user-attachments/assets/dc3d2176-f3a8-4462-a2b0-481f8f2d54ae" />


<br> **5. Menu Data Penerima**
<br> Seperti pada menu Donatur, menu ini menampilkan daftar penerima manfaat dengan tampilan yang berbeda tergantung jenisnya. 
Untuk penerima “Individu”, hanya ada satu baris tambahan “Deskripsi Penerima” yang berisi deskripsi spesifik dari penerima donasi yang identitas formal nya tidak dicatat seperti penerima yang berasar dari suatu lembanga. 
Untuk penerima “Lembaga”, tampilannya lebih lengkap dengan “Nama Lembaga”, “Jenis Lembaga”, dan “Nama Pengelola”. 
Perbedaan ini berasal dari jenis objek yang disimpan di `ArrayList<Penerima>`, bukan dari tabel data yang berbeda.

<img width="577" height="860" alt="image" src="https://github.com/user-attachments/assets/a32d80fe-33e2-4a98-a0d7-666a3a74d8c1" />


<br> **6. Menu Data Penyaluran**
<br> Menu ini menampilkan detail aktivitas penyaluran makanan yang berupa ID Penyaluran, ID Donasi, ID Penerima, nama kegiatan, tanggal penyaluran, jumlah porsi yang disalurkan, dan nama petugas yang menangani. 
Tabel ini merangkum seluruh alur sistem, mulai dari makanan dari donatur mana, disalurkan ke penerima mana, kapan, dan oleh siapa.

<img width="470" height="903" alt="image" src="https://github.com/user-attachments/assets/85434b81-3553-4c63-a941-edf0156eabd9" />


<br> **7. Proses Update Data**
<br> Berdasarkan _output_ sistem, terdapat langkah-langkah saat pengguna memperbarui data yang sudah ada. 
Proses ini bisa dilakukan di menu Donatur, Donasi, Penerima, atau Penyaluran, karena polanya sama. 
Setelah pengguna memasukkan ID data yang ingin diubah, program menampilkan ulang data lama sebagai pengingat, lalu meminta konfirmasi dengan mengetik “y” (ya) atau “n” (tidak). 
Jika pengguna menjawab “y”, program akan meminta input data baru satu per satu, lalu menampilkan pesan bahwa data berhasil diperbarui. 
Konfirmasi dua langkah ini dibuat untuk mencegah perubahan data yang tidak disengaja.

<img width="430" height="652" alt="image" src="https://github.com/user-attachments/assets/ba68ddf1-3037-4cf8-8d66-c4497309c28c" />
<img width="393" height="268" alt="image" src="https://github.com/user-attachments/assets/748ff4bf-89af-4a7d-a87f-6895e7861ac2" />


<br> **8. Proses Hapus Data**
<br> Prosesnya mirip dengan update data, pengguna akan memasukkan ID data yang ingin dihapus, lalu program menampilkan detail lengkap data tersebut sebagai konfirmasi. 
Setelah itu, program menanyakan “Yakin ingin menghapus data? (y/n)”. Jika pengguna mengetik “y”, data langsung dihapus dari daftar dan muncul pesan bahwa penghapusan berhasil. 
Jika mengetik selain itu, proses dibatalkan dan data tetap ada. Tangkapan layar sebaiknya diambil saat konfirmasi ini agar jelas data mana yang akan dihapus.

<img width="383" height="585" alt="image" src="https://github.com/user-attachments/assets/15e3e4f2-d44a-48f7-a60b-b3fa43ae02e5" />
<img width="388" height="537" alt="image" src="https://github.com/user-attachments/assets/b9b4dcb3-5751-4384-b17d-da8afd2f4809" />


<br> **9. Keluar dari Program**
<br> Tangkapan layar terakhir memperlihatkan pesan penutup (“Terima kasih sudah menggunakan Food Redistribution System”) yang muncul saat pengguna memilih menu “Keluar” di menu utama. 
Setelah pesan ini tampil, program benar-benar berhenti berjalan. Perulangan pada Controller yang menjaga menu tetap muncul dihentikan, dan kendali kembali ke sistem operasi.

<img width="542" height="330" alt="image" src="https://github.com/user-attachments/assets/076797a4-66ac-4502-872e-4e9171822450" />
