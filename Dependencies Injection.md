# 🧩 Dependency Injection: Penjelasan Sederhana dengan Kotlin

📅 Berdasarkan artikel *Dependency Injection Demystified (2006)*  

---

## 🔍 Apa Itu Dependency Injection?

**Dependency Injection (DI)** adalah istilah keren untuk konsep yang sangat sederhana:

> **Memberikan sebuah objek dependency-nya (variabel yang dibutuhkan) dari luar, bukan membuatnya sendiri.**

---

## ⚡ Versi Singkat

Dependency Injection = *Memberikan instance variabel ke sebuah objek dari luar.*  
Itu saja!

---

### 🧩 Part 1: Tanpa Dependency Injection (Hard-coded Dependency)

Di dalam sebuah class, biasanya kita punya beberapa variabel yang dipakai buat menjalankan metode-metodenya. Kita sebut aja variabel-variabel ini sebagai “dependensi”. Kebanyakan orang sih nyebutnya “variabel” atau kalau mau kerenan, “instance variable”.

```kotlin
class Example {
    private val myDatabase = DatabaseThingie() // dependency dibuat langsung di dalam class

    fun doStuff() {
        println("Mulai doStuff()...")
        myDatabase.getData()
        println("Selesai doStuff()")
    }
}

open class DatabaseThingie {
    open fun getData() {
        println("Mengambil data dari database asli.")
    }
}
```
Di sini kita punya sebuah variabel (atau dependensi) bernama myDatabase. Kita bikin sendiri di constructor-nya.

# 🧩 Part 2: Dependency Injection – Versi Lebih Fleksibel

Pada bagian ini, kita akan melihat bagaimana **Dependency Injection (DI)** membuat kode lebih fleksibel dengan **menyuntikkan dependency dari luar**, bukan membuatnya sendiri.

---

## 💡 Tujuan

Membuat class kita lebih **terbuka untuk modifikasi dan pengujian**, tanpa harus mengubah kode internalnya setiap kali ada perubahan dependency.

---

## 🧱 Contoh Kode Kotlin

```kotlin
class Example {
    private val myDatabase: DatabaseThingie

    // Constructor default
    constructor() {
        myDatabase = DatabaseThingie()
    }

    // Constructor dengan dependency injection
    constructor(useThisDatabaseInstead: DatabaseThingie) {
        myDatabase = useThisDatabaseInstead
    }

    fun doStuff() {
        println("Mulai doStuff()...")
        myDatabase.getData()
        println("Selesai doStuff()")
    }
}

open class DatabaseThingie {
    open fun getData() {
        println("Mengambil data dari database asli.")
    }
}
```


# 🧪 Part 3: Dependency Injection untuk Pengujian (Testing)

Salah satu alasan utama kita menggunakan Dependency Injection adalah agar kode **mudah diuji**. Dengan menyuntikkan dependency, kita bisa menggantinya dengan **mock object** saat pengujian.


## 🎯 Tujuan

- Menyuntikkan dependency palsu (mock) ke dalam class untuk memastikan perilakunya sesuai harapan.
- Memastikan bahwa metode tertentu benar-benar dipanggil saat program dijalankan.


## 🧱 Contoh Kode Kotlin

```kotlin
// Dependency asli
open class DatabaseThingie {
    open fun getData() {
        println("Mengambil data dari database asli.")
    }
}

// Mock untuk testing
class MockDatabase : DatabaseThingie() {
    var wasGetDataCalled = false

    override fun getData() {
        wasGetDataCalled = true
        println("Mock: getData() dipanggil.")
    }

    fun assertGetDataWasCalled() {
        check(wasGetDataCalled) { "getData() tidak dipanggil!" }
    }
}

// Class yang diuji
class Example(private val myDatabase: DatabaseThingie = DatabaseThingie()) {
    fun doStuff() {
        println("Mulai doStuff()...")
        myDatabase.getData()
        println("Selesai doStuff()")
    }
}

// Fungsi testing manual
fun testDoStuff() {
    val mockDatabase = MockDatabase()
    val example = Example(mockDatabase)

    example.doStuff()

    mockDatabase.assertGetDataWasCalled()
    println("✅ Test berhasil: getData() memang dipanggil.")
}
```


