# Catatan Back-End
## Pengenalan Back-End
### Latihan Membuat permintaan http (http request)
> cURL atau Client URL merupakan software berbasis command line yang dapat melakukan transaksi data melalui beberapa protokol internet, salah satunya HTTP/S. cURL dapat diakses secara langsung tanpa proses install melalui Terminal (Linux dan Mac).

GET :
```
curl -X GET https://coffee-api.dicoding.dev/coffees -i
```
* curl : merupakan perintah untuk menggunakan program cURL pada Terminal atau CMD.
* -X GET : menetapkan HTTP method/verb yang kita gunakan. GET berarti kita ingin mendapatkan sebuah data.
* https://coffee-api.dicoding.dev/coffees : merupakan alamat request yang dituju.
* -i : memberikan informasi detail terhadap response yang diberikan (HTTP response headers).

POST :
```
curl -X POST -H "Content-Type: application/json" -d "{\"name\": \"Kopi Tubruk\"}" https://coffee-api.dicoding.dev/transactions -i
```
* -X POST : dalam request kali ini kita menggunakan method POST. Karena membeli bukan hanya meminta data, tapi akan mengubah jumlah stok kopi yang ada. Selain itu kita juga melampirkan data berupa kopi apa yang akan dipesan. Sehingga tidak masuk akal bila kita menggunakan GET request.
* -H "Content-Type: application/json" : Menetapkan nilai "Content-Type: application/json" pada Header request. Fungsinya untuk memberitahu server bahwa kita melampirkan data dalam bentuk JSON.
* -d <JSON Content> : merupakan data yang dilampirkan pada request. Data ini berformat JSON dan memiliki informasi kopi apa yang ingin dipesan.
* https://coffee-api.dicoding.dev/transactions : Merupakan alamat request yang dituju untuk membeli kopi.

### Events pada javascript
>Berkaca dari dunia nyata, program komputer juga harus bekerja dengan pola event-driven. Syukurlah dengan Node.js kita dapat menerapkan pola tersebut dengan mudah.
```Javascript
// TODO 1
const { EventEmitter} = require('events');
// Import variable EventEmitter dari core module events
 
const birthdayEventListener = (name) => {
    console.log(`Happy birthday ${name}!`);
  }
   
// TODO 2
myEmitter = new EventEmitter();
// myEmitter merupakan instance dari EventEmitter

// TODO 3
myEmitter.on('birthday', birthdayEventListener);
// Untuk menggunakan fungsi birthdayEventListener kita harus set sebuah event, dalam kasus ini adalah event 'birthday'

// TODO 4
myEmitter.emit('birthday', 'Rayhan');
// Menjalankan event 'birthday' yang mana akan memanggil fungsi birthdayEventListener dengan parameter 'Rayhan'
```

### Filesytem pada Javascript
>Untuk mengakses berkas pada komputer kita dapat menggunakan method fs.readFile(). Method ini menerima tiga argumen yakni: lokasi berkas, encoding, dan callback function yang akan terpanggil bila berkas berhasil/gagal diakses.
```Javascript
// TODO: tampilkan teks pada notes.txt pada console.

const fs = require('fs'); // Mengimport fs yang dapat mempermudah kita dalam mengakses filesystem
const { resolve } = require('path'); // Mengimport core modules path untuk menetapkan alamat berkas secara lengkap dan dinamis

const fileReadCallback = (error, data) => {
    if(error) {
        console.log('Gagal membaca berkas');
        return;
    }
    console.log(data);
};

const pathNotes = resolve(__dirname, 'notes.txt'); // Mengambil lokasi file notes.txt 
fs.readFile(pathNotes, 'utf-8', fileReadCallback); // Membaca file notes.txt


// Versi synchronous :
const data = fs.readFileSync(pathNotes, 'utf-8'); 
console.log(data);
```

### Readble Stream
>Teknik ini tidak membaca berkas secara sekaligus, tapi dengan mengirim bagian demi bagian.
```Javascript
const fs = require('fs');
const { resolve } = require('path');

const pathData = resolve(__dirname, './input.txt');
const readableStream = fs.createReadStream(pathData, {
    highWaterMark: 15
});

readableStream.on('readable', () => {
    try {
        process.stdout.write(`[${readableStream.read()}]`); // process.stdout.write akan mencetak semua output pada baris yang sama.
        // console.log(`[${readableStream.read()}]`);  // akan mencetak setiap output pada baris yang berbeda, sedangkan 
    } catch (error) {
        // Catch error
    }
});

readableStream.on('end', ()=>{
    console.log('Done');
});
```
>createReadStream() mengembalikan EventEmitter, di mana kita dapat menetapkan fungsi listener setiap kali event readable dibangkitkan. Event readable akan dibangkitkan ketika buffer sudah memiliki ukuran sesuai dengan nilai yang ditetapkan pada properti highWaterMark, dalam arti buffer sudah siap dibaca. Kemudian event end akan dibangkitkan setelah proses stream selesai.
### Writeabale Stream
>Fungsi ini menerima satu argumen yakni alamat berkas untuk menyimpan hasil data yang dituliskan. Berkas output akan dibuat secara otomatis jika tidak ada, namun bila berkas tersebut sudah ada, maka data sebelumnya akan tertimpa!
```Javascript
const fs = require('fs');
 
const writableStream = fs.createWriteStream('output.txt');
 
writableStream.write('Ini merupakan teks baris pertama!\n');
writableStream.write('Ini merupakan teks baris kedua!\n');
writableStream.end('Akhir dari writable stream!');
```