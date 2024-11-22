# Presentasi_Data #

# Nama  : Lukman Eka Septiawan
# Kelas : TI-3C

## A. Praktikum 1: Converting Dart models into JSON

### 1. Di editor favorit Anda, buat proyek Flutter baru dan beri nama store_data

![image for practicum 1 step 1](images/p1-1.png)

### 2. Pada file main.dart, hapus kode yang ada dan tambahkan kode awal untuk aplikasi dengan kode berikut:

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter JSON Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({super.key});

  @override
  State<MyHomePage> createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('JSON'),
      ),
      body: Container(), 
    );
  }
}
```

### 3. Tambahkan folder baru ke root proyek Anda dengan nama assets.

![image for practicum 1 step 3](images/p1-3.png)

### 4. Di dalam folder aset, buat file baru bernama pizzalist.json dan salin konten yang tersedia di tautan https://gist.github.com/simoales/a33c1c2abe78b48a75ccfd5fa0de0620. File ini berisi daftar objek JSON

![image for practicum 1 step 3](images/p1-4.png)

### 5. Di file pubspec.yaml, tambahkan referensi ke folder aset baru, seperti yang ditunjukkan di sini:

```dart 
assets:
    - assets/
```

### 6. Pada kelas _MyHomePageState, di main.dart, tambahkan sebuah variabel state bernama pizzaString:

```dart
class _MyHomePageState extends State<MyHomePage> {
  String pizzaString = '';
```

### 7. Untuk membaca isi file pizzalist.json, di bagian bawah kelas _MyHomePageState di main.dart, tambahkan metode asinkron baru yang disebut readJsonFile, yang akan mengatur nilai pizzaString, seperti yang ditunjukkan di sini:

```dart
Future<void> readJsonFile() async {
    String myString = await DefaultAssetBundle.of(context).loadString('assets/pizza.json');
    setState(() {
      pizzaString = myString;
    });
  }
```

### 8. Pada kelas _MyHomePageState, timpa metode initState dan, di dalamnya, panggil metode readJsonFile: bahkan widget Teks sebagai child dari Container kita:

```dart
@override
void initState() {
    super.initState();
    readJsonFile();
}
```

### 9. Sekarang, kita ingin menampilkan JSON yang diambil di properti dalam Scaffold. Untuk melakukannya, tambahkan widget Teks sebagai child dari Container kita:

```dart
Widget build(BuildContext context) {
  return Scaffold(
      appBar: AppBar(
        title: const Text('JSON'),
      ),
      body: Text(pizzaString),
  );
}
```

### 10. Mari kita jalankan aplikasinya. Jika semuanya berjalan seperti yang diharapkan, Anda akan melihat konten file JSON di layar

![image for practicum 1 step 10](images/p1-10.png)

### 11. Kita ingin mengubah String ini menjadi sebuah List of Objects. Kita akan mulai dengan membuat kelas baru. Dalam folder lib aplikasi kita, buat file baru bernama pizza.dart.

![image for practicum 1 step 10](images/p1-11.png)

### 12. Di dalam file tersebut, tentukan properti kelas Pizza:

```dart
class Pizza {
  final int id;
  final String pizzaName;
  final String description;
  final double price;
  final String imageUrl;
}
```

### 13. Di dalam kelas Pizza, tentukan konstruktor bernama fromJson, yang akan mengambil sebuah Map sebagai parameter dan mengubah Map menjadi sebuah instance dari Pizza:

```dart
class Pizza {
  final int id;
  final String pizzaName;
  final String description;
  final double price;
  final String imageUrl;
  
  Pizza.fromJson(Map<String, dynamic> json) :
    id = json['id'],
    pizzaName = json['pizzaName'],
    description = json['description'],
    price = json['price'],
    imageUrl = json['imageUrl'];
}
```
### 14. Refaktor metode readJsonFile() pada kelas_MyHomePageState. Langkah pertama adalah mengubah String menjadi Map dengan memanggil metode jsonDecode. Pada method readJsonFile, tambahkan kode yang di cetak tebal berikut ini:
```dart 
Future<void> readJsonFile() async {
    String myString = await DefaultAssetBundle.of(context).loadString('assets/pizzalist.json');
    List pizzaMapList = jsonDecode(myString);
    setState(() {
      pizzaString = myString;
    });
}
 ```

### 15. Pastikan editor Anda secara otomatis menambahkan pernyataan impor untuk pustaka "dart:convert" di bagian atas file main.dart; jika tidak, tambahkan saja secara manual. Tambahkan juga pernyataan impor untuk kelas pizza:

```dart
import 'dart:convert';
import './pizza.dart';
```

### 16. Langkah terakhir adalah mengonversi string JSON kita menjadi List of native Dart objects. Kita dapat melakukan ini dengan mengulang pizzaMapList dan mengubahnya menjadi objek Pizza. Di dalam metode readJsonFile, di bawah metode jsonDecode, tambahkan kode berikut:

```dart
Future<void> readJsonFile() async {
    String myString = await DefaultAssetBundle.of(context).loadString('assets/pizzalist.json');
    List pizzaMapList = jsonDecode(myString);
    List<Pizza> myPizzas = [];
    for (var pizza in pizzaMapList) {
      Pizza myPizza = Pizza.fromJson(pizza);
      myPizzas.add(myPizza);
    }
    setState(() {
      pizzaString = myString;
    });
}
```

### 17. Hapus atau beri komentar pada metode setState yang mengatur String pizzaString dan kembalikan daftar objek Pizza sebagai gantinya:

```dart
// setState(() {
//   pizzaString = myString;
// });
return myPizzas;
```

### 18. Ubah signature metode sehingga Anda dapat menampilkan nilai balik secara eksplisit:

```dart
Future<List<Pizza>> readJsonFile() async {
    String myString = await DefaultAssetBundle.of(context).loadString('assets/pizzalist.json');
    List pizzaMapList = jsonDecode(myString);
    List<Pizza> myPizzas = [];
    for (var pizza in pizzaMapList) {
      Pizza myPizza = Pizza.fromJson(pizza);
      myPizzas.add(myPizza);
    }
    // setState(() {
    //   pizzaString = myString;
    // });
    return myPizzas;
}
```

### 19. Sekarang kita memiliki objek List of Pizza. Daripada hanya menampilkan sebuah Teks kepada pengguna, kita dapat menampilkan sebuah ListView yang berisi sekumpulan widget ListTile. Di bagian atas kelas _MyHomePageState, buat List<Pizza> bernama myPizzas:

```dart
List<Pizza> myPizzas = [];

class _MyHomePageState extends State<MyHomePage>
```

### 20. Dalam metode initState, pastikan Anda mengatur myPizzas dengan hasil panggilan ke readJsonFile:

```dart
@override
  void initState() {
    super.initState();
    readJsonFile().then((value) {
        setState(() {
            myPizzas = value;
        });
    });
}
```

### 21. Tambahkan kode berikut ini di dalam Scaffold, di dalam metode build():

```dart
@override
Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
            title: const Text('JSON'),
        ),
        body: ListView.builder(
            itemCount: myPizzas.length,
            itemBuilder: (context, index) {
                return ListTile(
                    title: Text(myPizzas[index].pizzaName),
                    subtitle: Text(myPizzas[index].description),
                );
            },
        ),
    );
}
```

### 22. Jalankan aplikasi. Antarmuka pengguna sekarang seharusnya jauh lebih ramah dan terlihat seperti yang ditunjukkan pada

![image for practicum 1 step 22](images/p1-22.png)

## B. Praktikum 2: Reading the JSON file

### 1. Tambahkan metode baru ke kelas Pizza, di file pizza.dart, yang disebut toJson. Ini akan mengembalikan sebuah Map<String, dynamic> dari objek:

```dart
Map<String, dynamic> toJson() {
    return {
        'id': id,
        'pizzaName': pizzaName,
        'description': description,
        'price': price,
        'imageUrl': imageUrl,
    };
}
```

### 2. Setelah Anda memiliki sebuah Map, Anda dapat menserialisasikannya kembali ke dalam string JSON. Tambahkan metode baru di di bagian bawah kelas _MyHomePageState, di dalam file main.dart, yang disebut convertToJSON:

```dart
class _MyHomePageState extends State<MyHomePage> {
    String pizzaString = '';
  
    String convertToJSON(List<Pizza> pizzas) {return jsonEncode(pizzas.map((pizza) => jsonEncode(pizza)).toList());
}
```

### 3. Metode ini mengubah objek List of Pizza kembali menjadi string Json dengan memanggil metode jsonEncode lagi di pustaka dart_convert.

### 4. Terakhir, mari panggil metode tersebut dan cetak string JSON di Debug Console. Tambahkan kode berikut ke metode readJsonFile, tepat sebelum mengembalikan List myPizzas:

```dart
Future<List<Pizza>> readJsonFile() async {
    String myString = await DefaultAssetBundle.of(context).loadString('assets/pizzalist.json');
    List pizzaMapList = jsonDecode(myString);
    List<Pizza> myPizzas = [];
    for (var pizza in pizzaMapList) {
      Pizza myPizza = Pizza.fromJson(pizza);
      myPizzas.add(myPizza);
    }
    // setState(() {
    //   pizzaString = myString;
    // });
    String json = convertToJSON(myPizzas);
    print(json);
    return myPizzas;
}
```

### 5. Jalankan aplikasi. Anda akan melihat string JSON dicetak, seperti yang ditunjukkan pada gambarberikut:

![image for practicum 2 step 5](images/p2-5.png)

## C. Praktikum 3: Saving data simply with SharedPreferences

### 1. Gunakan project pada pertemuan 11 bernama books. Pertama, tambahkan ketergantungan pada shared_preferences. Dari Terminal Anda, ketikkan perintah berikut

![image for practicum 3 step 1](images/p3-1.png)

### 2. Untuk memperbarui dependensi dalam proyek Anda, jalankan perintah flutter pub get dari jendela Terminal.

![image for practicum 3 step 2](images/p3-1.png)

### 3. Di bagian atas file main.dart, impor shared_preferences:

```dart
import 'dart:async';
import 'package:books/geolocation.dart';
import 'package:books/navigation_dialog.dart';
import 'package:books/navigation_first.dart';
import 'package:flutter/material.dart';
import 'package:http/http.dart';
import 'package:http/http.dart' as http;
import 'package:async/async.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  runApp(const MyApp());
}
```

### 4. Di bagian atas kelas _MyHomePageState, buat variabel status integer baru bernama appCounter:

```dart
int appCounter = 0;

class _FuturePageState extends State<FuturePage>
```

### 5. Dalam kelas _MyHomePageState, buat metode asinkron baru yang disebut readAndWritePreferences():

```dart
class _FuturePageState extends State<FuturePage> {
  String result = '';
  late Completer completer;

  Future readAndWritePreference() async {}
```

### 6. Di dalam metode readAndWritePreference, buatlah sebuah instance dari SharedPreferences:

```dart
Future readAndWritePreference() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
}
```

### 7. Setelah membuat instance preferensi, kita membuat kode yang mencoba baca nilai kunci appCounter. Jika nilainya nol, setel ke 0; lalu naikkan nilainya:

```dart
Future readAndWritePreference() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    appCounter = prefs.getInt('appCounter') ?? 0;
    appCounter++;
}
```

### 8. Setelah itu, atur nilai kunci appCounter di preferensi ke nilai baru:

```dart
Future readAndWritePreference() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    appCounter = prefs.getInt('appCounter') ?? 0;
    appCounter++;
    await prefs.setInt('appCounter', appCounter);
}
```

### 9. Memperbarui nilai status appCounter:

```dart
Future readAndWritePreference() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    appCounter = prefs.getInt('appCounter') ?? 0;
    appCounter++;
    await prefs.setInt('appCounter', appCounter);
    setState(() {
      appCounter = appCounter;
    });
}
```

### 10. Pada metode initState di kelas _MyHomePageState, panggil metode readAndWritePreference()dengan kode yang dicetak tebal:

```dart
@override
void initState() {
    super.initState();
    readAndWritePreference();
}
```

### 11. Dalam metode build, tambahkan kode berikut ini di dalam widget Container:

```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Back from the Future (Lukman)'),
      ),
      body: Center (
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceEvenly,
          children: [
            Text('You have opened the app $appCounter times.'),
            ElevatedButton(
              onPressed: () {},
              child: const Text('Reset counter'),
            ),
          ],
        ),
      ),
    );
}
```

### 12. Jalankan aplikasi. Saat pertama kali membukanya, Anda akan melihat layar yang mirip dengan yang berikut ini:

![image for practicum 4 step 12](images/p3-12.png)

### 13. Tambahkan metode baru ke kelas _MyHomePageState yang disebut deletePreference(), yang akan menghapus nilai yang disimpan:

```dart
Future deletePreference() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    await prefs.clear();
    setState(() {
      appCounter = 0;
    });
}
```

### 14. Dari properti onPressed dari widget ElevatedButton di metode build(), memanggil metodedeletePreference(), dengan kode di cetak tebal:

```dart
ElevatedButton(
    onPressed: () {
        deletePreference();
    },
    child: const Text('Reset counter'),
),
```

### 15. Jalankan aplikasi lagi. Sekarang, saat Anda menekan tombol Reset penghitung, nilai appCounter akan dihapus

![image for practicum 3 step 15](images/p3-15.gif)

## D. Praktikum 4: Accessing the filesystem, part 1: path_provider

### 1. menambahkan dependency yang relevan ke file pubspec.yaml. Tambahkan path_provider dengan mengetikkan perintah ini dari Terminal Anda:

```dart
dependencies:
  flutter:
    sdk: flutter
  path_provider: ^2.0.0
```

### 2. Di bagian atas file main.dart, tambahkan impor path_provider:

```dart
import 'package:flutter/material.dart';
import 'package:path_provider/path_provider.dart';
```

### 3. Di bagian atas kelas _MyHomePageState, tambahkan variabel State yang akan kita gunakan untuk memperbarui antarmuka pengguna:

```dart
class _MyHomePageState extends State<MyHomePage> {
  String documentsPath = '';
  String tempPath = '';
```

### 4. Masih dalam kelas _MyHomePageState, tambahkan metode untuk mengambil direktori temporary dan dokumen:

```dart
Future getPaths() async {
    final docDir = await getApplicationDocumentsDirectory();
    final tempDir = await getTemporaryDirectory();
    setState(() {
      documentsPath = docDir.path;
      tempPath = tempDir.path;
    });
}
```

### 5. Pada metode initState dari kelas _MyHomePageState, panggil metode getPaths:

```dart
@override
void initState() {
    super.initState();
    getPaths();
}
```

### 6. Pada metode build _MyHomePageState, buat UI dengan dua widget Teks yang menunjukkan path yang diambil:

```dart
@override
Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text(widget.title),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly,
        children: [
          Text('Doc path: $documentsPath'),
          Text('Temp path: $tempPath'),
        ],
      )
    );
}
```

### 7. Jalankan aplikasi. Anda akan melihat layar yang terlihat seperti berikut ini:

![image for practicum 4 step 7](images/p4-7.png)

## E. Praktikum 5: Accessing the filesystem, part 2: Working with directories

### 1. Di bagian atas berkas main.dart, impor pustaka dart:io:

```dart

```

### 2. Di bagian atas kelas _MyHomePageState, di file main.dart, buat dua variabel State baru untuk file dan isinya:

```dart

```

### 3. Masih dalam kelas MyHomePageState, buat metode baru bernama writeFile dan gunakan kelas File dari pustaka dart:io untuk membuat file baru:

```dart

```

### 4. Dalam metode initState, setelah memanggil metode getPaths, dalam metode then, buat sebuah file dan panggil metode writeFile:
### 5. Buat metode untuk membaca file:
### 6. Dalam metode build, di widget Column, perbarui antarmuka pengguna dengan ElevatedButton. Ketika pengguna menekan tombol, tombol akan mencoba membaca konten file dan menampilkannya di layar, cek kode cetak tebal:
### 7. Jalankan aplikasi dan tekan tombol Baca File. Di bawah tombol tersebut, Anda akan melihat teks Margherita, Capricciosa, Napoli, seperti yang ditunjukkan pada tangkapan layar berikut:

## F. Praktikum 6: Using secure storage to store data