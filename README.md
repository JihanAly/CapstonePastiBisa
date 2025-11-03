

# Quiz Sederhana

Sebuah sistem aplikasi berbasis desktop (Java Swing) untuk memfasilitasi proses ujian/quiz, yang didesain untuk penggunaan oleh tiga peran utama: **Admin**, **Guru**, dan **Siswa**.

-----

## ✨ Fitur Utama Program

Aplikasi ini mengadopsi arsitektur multi-role untuk memastikan pengelolaan dan pelaksanaan quiz yang terstruktur.

### 👤 Admin

| Fitur | Deskripsi |
| :--- | :--- |
| **Verifikasi Akun** | Melakukan persetujuan (`Verifikasi`) atau penolakan (`Tolak`) terhadap akun **Guru** dan **Siswa** yang baru mendaftar. |
| **Pengawasan** | Melihat daftar akun yang masih tertunda verifikasi. |

### 👨‍🏫 Guru

| Fitur | Deskripsi |
| :--- | :--- |
| **Manajemen Quiz** | Membuat quiz baru (`Tambah Quiz`), memperbarui soal yang sudah ada (`Update Quiz`), dan menghapus quiz beserta semua skor terkait (`Hapus Quiz`). |
| **Evaluasi Skor** | Melihat semua skor siswa untuk quiz yang dibuat, termasuk opsi untuk menghapus skor individu. |
| **Feedback** | Mengirimkan pesan/umpan balik ke siswa tertentu. |

### 👩‍🎓 Siswa

| Fitur | Deskripsi |
| :--- | :--- |
| **Pengerjaan Quiz** | Melihat daftar quiz yang tersedia dan mengerjakannya, dengan fitur navigasi antar soal (`Next`/`Kembali`) sebelum `Kumpulkan`. |
| **Rapor Nilai** | Melihat riwayat skor pribadi (jumlah benar, salah, dan total nilai) dari semua quiz yang telah dikerjakan. |
| **Pesan** | Melihat umpan balik/pesan yang dikirim oleh Guru. |

-----

## 🏗️ Penerapan Konsep Object-Oriented Programming (OOP)

Program ini dirancang menggunakan prinsip-prinsip OOP untuk modularitas dan kemudahan pemeliharaan.

### 1\. Enkapsulasi (Encapsulation)

  * **Penerapan:** Melindungi data internal objek dari akses langsung dan menyediakan antarmuka publik yang terkontrol melalui metode *getter* dan *setter*.
  * **Contoh Kode:** Kelas model seperti `QuestionItem` mendeklarasikan properti penting (`number`, `question`, `correct`) sebagai `private final` dan hanya mengeksposnya melalui metode *getter* (`getNumber()`, `getQuestion()`, `getCorrect()`), memastikan integritas data. Selain itu, kredensial sensitif database dalam `DatabaseConnection.java` dideklarasikan sebagai `private static final`.

### 2\. Pewarisan (Inheritance)

  * **Penerapan:** Kelas turunan mewarisi properti dan perilaku dari kelas induk untuk menghindari duplikasi kode.
  * **Contoh Kode:** Semua kelas *Controller* yang berhubungan dengan database, seperti `AdminController`, `AuthController`, dan `TeacherQuizController`, mewarisi fungsionalitas dasar database (seperti `execUpdate` dan `execQuery`) dari kelas induk **`BaseController`**.

### 3\. Abstraksi (Abstraction)

  * **Penerapan:** Menyembunyikan implementasi yang rumit dan hanya menampilkan fungsionalitas yang relevan kepada pengguna (atau kelas lain).
  * **Contoh Kode:** Kelas *View* (misalnya, `TeacherTambahQuiz.java`) hanya berinteraksi dengan `TeacherQuizController` melalui metode tingkat tinggi seperti `saveBatch(teacherId, quizTitle, items)`. Detail tentang koneksi database, penyiapan `PreparedStatement`, dan penanganan transaksi SQL (`INSERT INTO quiz...`) sepenuhnya tersembunyi di dalam lapisan *Controller* dan *Utilities*.

### 4\. Polimorfisme (Polymorphism)

  * **Penerapan:** Kemampuan suatu objek untuk mengambil banyak bentuk, diwujudkan melalui implementasi *interface*.
  * **Contoh Kode:** Kelas `StudentAuthentication` dan `TeacherAuthentication` keduanya memiliki metode **`registrationQuery()`**. Metode ini, meskipun namanya sama, memberikan implementasi SQL yang berbeda sesuai dengan peran (`INSERT INTO student...` atau `INSERT INTO teacher...`).

### 5\. Interface

  * **Penerapan:** Mendefinisikan kontrak standar yang harus diikuti oleh kelas-kelas yang terkait.
  * **Contoh Kode:** **`RegistrationMechanism`** adalah sebuah *interface* yang mendefinisikan metode **`registrationQuery()`**. Kelas konkret seperti `StudentAuthentication` dan `TeacherAuthentication` **wajib** mengimplementasikan antarmuka ini untuk menyediakan query SQL pendaftaran yang spesifik.

-----

## 📦 Struktur Folder / Package

Struktur *package* proyek ini mengikuti prinsip pemisahan tanggung jawab (Separation of Concerns):

```
ghenayari/capstonepastibisa
└── src/main/java/app
    ├── controller/
    │   ├── AdminController.java
    │   ├── AuthController.java
    │   ├── StudentQuizController.java
    │   └── TeacherQuizController.java
    ├── model/
    │   └── QuestionItem.java
    ├── utilities/
    │   ├── authentication/registration/
    │   │   ├── RegistrationMechanism.java
    │   │   ├── StudentAuthentication.java
    │   │   └── TeacherAuthentication.java
    │   ├── base/
    │   │   └── BaseController.java
    │   └── data/
    │       └── DatabaseConnection.java
    ├── view/
    │   ├── AdminDashboard.java
    │   ├── Login.java
    │   ├── StudentDashboard.java
    │   ├── StudentFeedback.java
    │   ├── StudentKerjakanQuiz.java
    │   ├── StudentRapor.java
    │   ├── TeacherDashboard.java
    │   ├── TeacherFeedback.java
    │   ├── TeacherLihatSkor.java
    │   ├── TeacherTambahQuiz.java
    │   └── TeacherUpdateQuiz.java
    └── MainApplication.java
```

| Package | Tanggung Jawab |
| :--- | :--- |
| `app` | Titik masuk utama aplikasi (`MainApplication`). |
| `app.controller` | Logika bisnis, mengelola alur data, dan berkoordinasi dengan *utilities* database. |
| `app.model` | Representasi struktur data (misalnya, objek soal quiz). |
| `app.view` | Komponen antarmuka pengguna (GUI), bertanggung jawab atas tampilan dan interaksi pengguna. |
| `app.utilities.data` | Fungsionalitas koneksi dan operasi dasar database (`DatabaseConnection`). |
| `app.utilities.base` | Kelas dasar yang menyediakan fungsionalitas umum untuk *Controller* (contoh: `BaseController`). |
| `app.utilities.authentication.registration` | Logika dan kontrak untuk mekanisme pendaftaran pengguna. |




