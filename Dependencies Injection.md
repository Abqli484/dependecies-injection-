# 🧩 Dependency Injection: Penjelasan Sederhana dengan Kotlin

📅 Berdasarkan artikel *Dependency Injection Demystified (2006)*  
📝 Ditulis ulang dan diterjemahkan oleh [Nama Kamu]

---

## 🔍 Apa Itu Dependency Injection?

**Dependency Injection (DI)** adalah istilah keren untuk konsep yang sangat sederhana:

> **Memberikan sebuah objek dependency-nya (variabel yang dibutuhkan) dari luar, bukan membuatnya sendiri.**

---

## ⚡ Versi Singkat

Dependency Injection = *Memberikan instance variabel ke sebuah objek dari luar.*  
Itu saja!

---

## 🧩 Part 1: Tanpa Dependency Injection (Hard-coded Dependency)

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

