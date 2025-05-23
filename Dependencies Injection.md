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
## Contoh Penggunaan Dependency Injection

### Tanpa Dependency Injection

Berikut contoh representasi kelas `Car` yang membuat sendiri dependensi `Engine`:

```kotlin
class Engine {
    fun start() {
        println("Engine started.")
    }
}

class Car {

    private val engine = Engine()

    fun start() {
        engine.start()
    }
}

fun main(args: Array<String>) {
    val car = Car()
    car.start()
}
```
📌 Mengapa ini bukan Dependency Injection?

Karena kelas Car membuat sendiri objek Engine, maka ini bukan dependency injection. Hal ini bisa menimbulkan beberapa masalah:

1. **Tightly Coupled**
`Car` dan `Engine` menjadi terlalu bergantung satu sama lain. Misalnya, jika ingin menggunakan ElectricEngine, Anda harus membuat kelas Car baru. Ini menyulitkan untuk mengganti atau mengembangkan.

2. **Sulit untuk Pengujian**
Karena `Car` membuat instance nyata dari `Engine`, Anda tidak bisa dengan mudah mengganti `Engine` dengan versi mock atau fake untuk keperluan testing.

### Dengan Dependency Injection (Constructor Injection)
Dengan DI, Anda menyediakan objek `Engine` dari luar ke konstruktor `Car`:

```kotlin
class Engine {
    fun start() {
        println("Engine started.")
    }
}

class Car(private val engine: Engine) {
    fun start() {
        engine.start()
    }
}

fun main(args: Array<String>){
    val engine = Engine()
    val car = Car(engine) // DI lewat constructor
    car.start()
}
```
Fungsi main menggunakan `Car`. Karena `Car` bergantung pada `Engine`, aplikasi membuat instance dari `Engine` terlebih dahulu, lalu menggunakannya untuk membuat instance dari `Car`.

✅ Keuntungan Pendekatan Ini:

1. Reusabilitas Kelas `Car`
Anda bisa mengirimkan berbagai implementasi Engine ke dalam `Car`. Contoh: ElectricEngine, GasEngine, dll. Car tetap berfungsi tanpa perlu diubah.

2. Mudah Diuji
Anda bisa memberikan test double seperti FakeEngine untuk pengujian unit test.

### Alternatif Dependency Injection (Field Injection (Setter Injection))
Beberapa kelas framework Android seperti activity dan fragment diinstansiasi oleh sistem, sehingga constructor injection tidak memungkinkan. 
Dengan field injection, dependensi diinisialisasi setelah objek kelas dibuat. Kodenya akan terlihat seperti ini:

```kotlin
class Engine {
    fun start() {
        println("Engine started.")
    }
}

class Car {
    lateinit var engine: Engine

    fun start() {
        engine.start()
    }
}

fun main(args: Array<String>){
    val car = Car()
    car.engine = Engine() // setter injection
    car.start()
}
```
