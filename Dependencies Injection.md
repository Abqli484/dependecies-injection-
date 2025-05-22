# Dependency Injection (DI) dalam Pengembangan Android

**Dependency Injection (DI)** adalah teknik yang banyak digunakan dalam pemrograman dan sangat cocok untuk pengembangan Android. Dengan mengikuti prinsip-prinsip DI, Anda membangun dasar arsitektur aplikasi yang baik.

## Keuntungan Menerapkan Dependency Injection

- ♻️ Reusabilitas kode (kode dapat digunakan kembali dengan mudah)  
- 🔄 Kemudahan dalam *refactoring* (merombak atau menyusun ulang kode)  
- ✅ Kemudahan dalam pengujian (*testing*)

---

## Dasar-dasar Dependency Injection

Sebelum membahas *dependency injection* secara spesifik dalam Android, bagian ini memberikan gambaran umum tentang bagaimana DI bekerja.

### Apa itu Dependency Injection?

Dalam pemrograman, kelas-kelas sering kali membutuhkan referensi ke kelas lain. Misalnya:

> Sebuah kelas `Car` mungkin membutuhkan referensi ke kelas `Engine`.  
> Maka, `Engine` adalah **dependensi** bagi `Car`.

`Car` membutuhkan instance dari `Engine` agar bisa berjalan. Ada tiga cara umum untuk mendapatkan dependensi seperti ini:

1. **Membuat sendiri dependensinya di dalam kelas.**  
   Contoh: `Car` membuat dan menginisialisasi `Engine` sendiri.

2. **Mengambil dari tempat lain.**  
   Contoh: Beberapa API Android seperti `Context` dan `getSystemService()` bekerja dengan cara ini.

3. **Menerima sebagai parameter.**  
   Aplikasi memberikan dependensi saat objek dibuat (lewat konstruktor) atau melalui parameter fungsi.  
   Contoh: Konstruktor `Car` menerima objek `Engine` sebagai parameter.

✅ **Pendekatan ketiga inilah yang disebut Dependency Injection!**

Dengan DI, Anda *menyediakan* dependensi yang dibutuhkan oleh sebuah kelas dari luar, bukan membiarkan kelas tersebut mencarinya sendiri.

---

> **Catatan:**  
> Di Android, beberapa kelas seperti `Activity` dan `Fragment` diinstansiasi langsung oleh sistem, sehingga tidak memungkinkan menggunakan *constructor injection*. Dalam kasus seperti ini, kita menggunakan pendekatan lain seperti *field injection* atau *method injection*.
