# Dependency Management pada GetX

## Apa Itu Dependency Management?

Di **GetX (Flutter)**, **Dependency Management** adalah fitur untuk mendaftarkan, menyimpan, menyediakan, dan mengelola object/class secara otomatis seperti Controller, Service, Repository, API Client, dan Database Helper tanpa harus membuat instance manual berulang kali.

Sederhananya:

> Dependency Management adalah cara GetX mengatur siapa yang membuat object, kapan dipakai, dan kapan dibuang.

Fitur ini menggunakan konsep **Dependency Injection (DI)**.

---

## Kenapa Penting?

Tanpa GetX:

```dart
final controller = HomeController();
```

Jika dipakai di banyak halaman:

* Object bisa dibuat berulang
* Sulit dikelola
* Boros memory
* Susah maintenance

Dengan GetX:

```dart
Get.put(HomeController());
```

Lalu panggil di mana saja:

```dart
final controller = Get.find<HomeController>();
```

Lebih efisien, bersih, dan rapi.

---

## Fungsi Utama Dependency Management

## 1. Get.put()

Membuat object langsung saat didaftarkan.

```dart
Get.put(HomeController());
```

Ambil kembali:

```dart
Get.find<HomeController>();
```

Cocok untuk:

* Controller utama
* Object yang langsung digunakan

---

## 2. Get.lazyPut()

Object dibuat saat pertama kali dipanggil.

```dart
Get.lazyPut(() => AuthController());
```

Kelebihan:

* Hemat memory
* Tidak langsung dibuat

Cocok untuk:

* Halaman yang jarang dibuka
* Controller sekunder

---

## 3. Get.putAsync()

Untuk object asynchronous seperti database atau SharedPreferences.

```dart
await Get.putAsync(() async {
  return await DatabaseService().init();
});
```

Cocok untuk:

* Inisialisasi startup app
* Firebase
* Local storage

---

## 4. Get.create()

Selalu membuat instance baru setiap dipanggil.

```dart
Get.create(() => CartController());
```

Cocok untuk:

* Controller temporary
* Object per halaman

---

## Contoh Implementasi Lengkap

### HomeController

```dart
import 'package:get/get.dart';

class HomeController extends GetxController {
  var count = 0.obs;

  void increment() {
    count++;
  }
}
```

---

### Register Dependency

```dart
void main() {
  Get.put(HomeController());
  runApp(MyApp());
}
```

---

### Gunakan di UI

```dart
class HomePage extends StatelessWidget {
  final controller = Get.find<HomeController>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Obx(() => Text('${controller.count}')),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: controller.increment,
      ),
    );
  }
}
```

---

## Binding (Best Practice)

Untuk project besar, dependency sebaiknya dikelola dengan Binding.

### HomeBinding

```dart
class HomeBinding extends Bindings {
  @override
  void dependencies() {
    Get.put(HomeController());
  }
}
```

---

### Route Setup

```dart
GetMaterialApp(
  getPages: [
    GetPage(
      name: '/home',
      page: () => HomePage(),
      binding: HomeBinding(),
    ),
  ],
);
```

Saat halaman dibuka:

* Controller otomatis dibuat
* Siap digunakan

---

## Permanent Dependency

Jika object harus hidup selama aplikasi berjalan:

```dart
Get.put(AuthService(), permanent: true);
```

Contoh:

* Login session
* Theme service
* Language service

---

## Auto Dispose

Secara default, GetX dapat menghapus dependency saat sudah tidak digunakan lagi.

Keuntungan:

* Hemat memory
* Object tidak menumpuk
* Lebih ringan

---

## Perbandingan Singkat

| Method       | Dibuat Kapan   | Cocok Untuk      |
| ------------ | -------------- | ---------------- |
| Get.put      | Langsung       | Controller utama |
| Get.lazyPut  | Saat dipanggil | Hemat memory     |
| Get.putAsync | Async startup  | Database / Prefs |
| Get.create   | Selalu baru    | Temporary object |

---

## Analogi Sederhana

Bayangkan aplikasi adalah kantor:

* Controller = Pegawai
* Service = Staff tetap
* Dependency Management = HRD

HRD mengatur:

* Siapa masuk kerja
* Kapan dipakai
* Kapan pulang

Kamu tinggal panggil saat butuh.

---

## Keuntungan Dependency Management GetX

* Kode lebih bersih
* Tidak perlu instance manual
* Reusable object
* Mudah maintenance
* Lebih scalable
* Cocok untuk Clean Architecture
* Memory management otomatis

---

## Struktur Project Besar

```text
lib/
│── app/
│   ├── modules/
│   │   ├── home/
│   │   │   ├── home_page.dart
│   │   │   ├── home_controller.dart
│   │   │   └── home_binding.dart
│   ├── routes/
│   │   ├── app_pages.dart
│   │   └── app_routes.dart
│── main.dart
```

---

## Kesimpulan

**Dependency Management pada GetX adalah sistem untuk membuat, menyimpan, menyediakan, dan menghapus object seperti Controller atau Service secara otomatis menggunakan Dependency Injection.**

Dengan fitur ini, aplikasi menjadi:

* Lebih rapi
* Lebih efisien
* Mudah dikembangkan
* Profesional

---

## Tiga Pilar GetX

1. State Management
2. Route Management
3. Dependency Management

---

## Author

Made with Flutter + GetX
