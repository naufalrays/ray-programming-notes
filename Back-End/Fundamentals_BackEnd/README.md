# Catatan Back-End
## Dasar-Dasar Node.js untuk Back-End
### Menyiapkan Project
```
npm init --y
```
>--y pada akhir perintah tersebut berfungsi untuk menjawab seluruh pertanyaan yang diberikan NPM ketika membuat proyek baru dengan jawaban/nilai default.

### Menambahkan Script
```
"scripts": {
    "start": "node server.js" 
  }
```
```
npm run start
```
Ketika kita menjalankan sintaks di atas maka sama halnya kita menjalankan "node server.js"
### Membuat HTTP Server
>HTTP module memiliki banyak member seperti objek, properti, atau method yang berguna untuk melakukan hal-hal terkait protokol HTTP.
```Javascript
const requestListener = (request, response) => {
    response.setHeader('Content-Type', 'text/html');
 
    response.statusCode = 200;
    response.end('<h1>Halo HTTP Server!</h1>');
};
```
>Request listener memiliki 2 parameter, yakni request dan response. Seperti yang tertulis pada contoh kode di atas, request merupakan objek yang menyimpan informasi terkait permintaan yang dikirimkan oleh client. Di dalam objek ini kita bisa melihat alamat yang diminta, data yang dikirim, ataupun HTTP metode yang digunakan oleh client.
### Membuat localhost:5000 :
```Javascript
const http = require('http');
 
const requestListener = (request, response) => {
    response.setHeader('Content-Type', 'text/html');
 
    response.statusCode = 200;
    response.end('<h1>Halo HTTP Server!</h1>');
};
 
 
const server = http.createServer(requestListener);
 
const port = 5000;
const host = 'localhost';
 
server.listen(port, host, () => {
    console.log(`Server berjalan pada http://${host}:${port}`);
});
```
### Menambahkan Properti Method
>Dengan memiliki nilai method, kita bisa memberikan respons berbeda berdasarkan tipe method-nya.
```Javascript
const requestListener = (request, response) => {
    const { method } = request;
 
    if(method === 'GET') {
        // response ketika GET
    }
 
    if(method === 'POST') {
        // response ketika POST
    }
 
    // Anda bisa mengevaluasi tipe method lainnya
};
```
### Mendapatkan Body Request
```Javascript
if(method === 'POST') {
    let body = [];
        
    request.on('data', (chunk) => {
        body.push(chunk);
    });
    
    request.on('end', () => {
        body = Buffer.concat(body).toString();
        const { name } = JSON.parse(body); // Agar dapat mengolah response data json
        response.end(`<h1>Hai, ${name}!</h1>`);
    });
}
```
### Mendapatkan Nilai URL 
> Dalam http.clientRequest, untuk mendapatkan nilai url sangatlah mudah, semudah kita mendapatkan nilai request method yang digunakan.
```Javascript
const requestListener = (request, response) => {
    const { url } = request;
};
```
> Dengan mendapatkan nilai url, kita dapat merespons client sesuai dengan path yang ia minta.
```Javascript
const requestListener = (request, response) => {
    const { url } = request;
 
    if(url === '/') {
        // curl http://localhost:5000/
    }
 
    if(url === '/about') {
        // curl http://localhost:5000/about
    }
 
    // curl http://localhost:5000/<any>
};
```
> Kita juga bisa mengombinasikan evaluasi dengan method request. Alhasil, kita dapat menentukan respons lebih spesifik lagi.
```Javascript
const requestListener = (request, response) => {
    const { url, method } = request;

    if(url === '/') {
        if(method === 'GET') {
            // curl -X GET http://localhost:5000/
        }
        // curl -X <any> http://localhost:5000/
    }
    if(url === '/about') {
        if(method === 'GET') {
            // curl -X GET http://localhost:5000/about
        }
        if(method === 'POST') {
            // curl -X POST http://localhost:5000/about
        }
        // curl -X <any> http://localhost:5000/about
    }
    // curl -X <any> http://localhost:5000/<any>
};
```
Latihan :
* URL: '/'
    * Method: GET
        * Mengembalikan “Ini adalah homepage”.
    * Method: <any> (selain GET)
        * Mengembalikan “Halaman tidak dapat diakses dengan <any> request”.
* URL: ‘/about’
    * Method: GET
        * Mengembalikan “Halo! Ini adalah halaman about”.
    * Method: POST (dengan melampirkan data name pada body)
        * Mengembalikan “Halo, <name>! Ini adalah halaman about”.
    * Method: <any> (selain GET dan POST)
        * Mengembalikan “Halaman tidak dapat diakses dengan <any> request”.
* URL: <any> (selain / dan /about)
    * Method: <any>
        * Mengembalikan “Halaman tidak ditemukan!”.
```Javascript
const http = require('http');
 
const requestListener = (request, response) => {
    response.setHeader('Content-Type', 'text/html');
    response.statusCode = 200;
 
    const { method, url } = request;
 
    if(url === '/') {
        if(method === 'GET') {
            response.end('<h1>Ini adalah homepage</h1>');
        } else {
            response.end(`<h1>Halaman tidak dapat diakses dengan ${method} request</h1>`);
        }
    } else if(url === '/about') {
        if(method === 'GET') {
            response.end('<h1>Halo! Ini adalah halaman about</h1>')
        } else if(method === 'POST') {
            let body = [];
    
            request.on('data', (chunk) => {
                body.push(chunk);
            });
 
            request.on('end', () => {
                body = Buffer.concat(body).toString();
                const { name } = JSON.parse(body);
                response.end(`<h1>Halo, ${name}! Ini adalah halaman about</h1>`);
            });
        } else {
            response.end(`<h1>Halaman tidak dapat diakses menggunakan ${method} request</h1>`);
        }
    } else {
        response.end('<h1>Halaman tidak ditemukan!</h1>');
    }
};
 
const server = http.createServer(requestListener);
 
const port = 5000;
const host = 'localhost';
 
server.listen(port, host, () => {
    console.log(`Server berjalan pada http://${host}:${port}`);
});
```
### Membuat Response Status
```Javascript
const requestListener = (request, response) => {
    response.setHeader('Content-Type', 'text/html');
 
    const { method, url } = request;
 
    if(url === '/') {
        if(method === 'GET') {
            response.statusCode = 200;
            response.end('<h1>Ini adalah homepage</h1>');
        } else {
            response.statusCode = 400;
            response.end(`<h1>Halaman tidak dapat diakses dengan ${method} request</h1>`);
        }
    } else if(url === '/about') {
        if(method === 'GET') {
            response.statusCode = 200;
            response.end('<h1>Halo! Ini adalah halaman about</h1>')
        } else if(method === 'POST') {
            let body = [];
    
            request.on('data', (chunk) => {
                body.push(chunk);
            });
 
            request.on('end', () => {
                body = Buffer.concat(body).toString();
                const { name } = JSON.parse(body);
                response.statusCode = 200;
                response.end(`<h1>Halo, ${name}! Ini adalah halaman about</h1>`);
            });
        } else {
            response.statusCode = 400;
            response.end(`<h1>Halaman tidak dapat diakses menggunakan ${method} request</h1>`);
        }
    } else {
        response.statusCode = 404;
        response.end('<h1>Halaman tidak ditemukan!</h1>');
    }
};
```
### Response Header
>Nah, pada latihan kali ini kita akan mengubah format HTML menjadi format JSON. Selain itu, kita akan tambahkan properti X-Powered-By pada header untuk memberitahu client teknologi server apa yang kita gunakan. 
```Javascript
response.setHeader('Content-Type', 'text/html'); // Menampilkan format HTML
response.setHeader('Content-Type', 'application/json'); // Menampilkan format json
response.setHeader('X-Powered-By', 'NodeJS'); // Mengetahui teknologi server yang digunakan
```
### Mengubah Body Menjadi Json
```Javascript
response.end(JSON.stringify({
    message: 'Halaman tidak ditemukan!',
}));
```
## Menggunakan Framework Hapi
### Menginstall Framework Hapi
>Untuk membuat HTTP server menggunakan Hapi, kita tidak lagi menggunakan core module http secara langsung. Namun, kita akan membuat server melalui modul pihak ketiga @hapi/hapi. Untuk menggunakan modul tersebut, kita perlu memasang terlebih dahulu melalui NPM dengan perintah.
```
npm install @hapi/hapi
```
Setelah proses pemasangan berhasil, barulah kita bisa menggunakan modul tersebut.
```Javascript
const Hapi = require('@hapi/hapi');
```
Pembuatan server menggunakan Hapi memiliki struktur kode yang berbeda dari cara asli. Berikut adalah dasar kode dalam membuat HTTP server pada Hapi:
```Javascript
const Hapi = require('@hapi/hapi');
 
const init = async () => {
    const server = Hapi.server({
        port: 5000,
        host: 'localhost',
    });
 
    await server.start();
    console.log(`Server berjalan pada ${server.info.uri}`);
}
 
init();
```
### Method/Verb Request dan Routing
>Routing pada Hapi tidak dilakukan di dalam request handler seperti cara native. Namun, ia memanfaatkan objek route configuration yang disimpan pada method server.route().
```Javascript
const init = async () => {
 
    const server = Hapi.server({
        port: 5000,
        host: 'localhost'
    });
 
    server.route({
        method: 'GET',
        path: '/',
        handler: (request, h) => {
            return 'Hello World!';
        }
    });
 
    await server.start();
    console.log(`Server berjalan pada ${server.info.uri}`);
};
```
#### Single Responsibility Principle
>Untuk memenuhi kaidah Single Responsibility Principle maka kita harus membuat route ada pada file javascript lainnya.  Dengan begitu, satu berkas JavaScript hanya memiliki satu fungsi atau tanggung jawab saja
```
server.js : 
```
```Javascript
const Hapi = require('@hapi/hapi');
const routes = require('./routes');
 
const init = async () => {
    const server = Hapi.server({
        port: 5000,
        host: 'localhost',
    });
 
    server.route(routes);
 
    await server.start();
    console.log(`Server berjalan pada ${server.info.uri}`);
};
 
init();
```
```
routes.js :
```
```Javascript
const routes = [
    {
        method: 'GET',
        path: '/',
        handler: (request, h) => {
            return 'Homepage';
        },
    },
    {
        method: 'GET',
        path: '/about',
        handler: (request, h) => {
            return 'About page';
        },
    },
];
 
module.exports = routes;
```
Latihan :
* URL: '/'
    * Method: GET
        * Mengembalikan "Homepage".
    * Method: <any> (selain GET)
        * Mengembalikan "Halaman tidak dapat diakses dengan method tersebut".
* URL: '/about'
    * Method: GET
        * Mengembalikan "About page".
    * Method: <any> (selain GET)
        * Mengembalikan "Halaman tidak dapat diakses dengan method tersebut".
* URL: <any> (selain / dan /about)
    * Method: <any>
        * Mengembalikan "Halaman tidak ditemukan!".
```Javascript
const routes = [
    {
        method: 'GET',
        path: '/',
        handler: (request, h) => {
            return 'Homepage';
        },
    },
    {
        method: '*',
        path: '/',
        handler: (request, h) => {
            return 'Halaman tidak dapat diakses dengan method tersebut';
        }
    },
    {
        method: 'GET',
        path: '/about',
        handler: (request, h) => {
            return 'About page';
        },
    }, 
    {
        method: '*',
        path: '/about',
        handler: (request, h) => {
            return 'Halaman tidak dapat diakses dengan method tersebut';
        }
    },
    {
        method: '*',
        path: '/{any*}',
        handler: (request, h) => {
            return 'Halaman tidak ditemukan';
        }
    }
];
 
module.exports = routes;
```
### Teknik Path Parameter
>Di Framework Hapi, Path parameter sangat mudah untuk diterapkan. Cukup dengan membungkus path dengan tanda { }. Sebagai contoh:
```Javascript
server.route({
    method: 'GET',
    path: '/users/{username}',
    handler: (request, h) => {
        const { username } = request.params;
        return `Hello, ${username}!`;
    },
});
```
Jika kita ingin tidak wajib untuk memberikan nilai pada params :
```Javascript
{
        method: 'GET',
        path: '/hello/{name?}',
        handler: (request, h) => {
            const { name = 'stranger' } = request.params;
            return `Hello, ${name}!`;
        }
    },
```
Maka nilai default dari name adalah stranger
### Query Parameters
>Untuk bisa mendapatkan nilai dari query parameter digunakan request.query
```Javascript
server.route({
    method: 'GET',
    path: '/',
    handler: (request, h) => {
        const { name, location } = request.query;
        return `Hello, ${name} from ${location}`;
    },
 });
```
### Body/Payload Request
Ketika menggunakan Node.js, untuk mendapatkan data pada body request--meskipun datanya hanya sebatas teks--kita harus berurusan dengan Readable Stream. Di mana untuk mendapatkan data melalui stream tak semudah seperti kita menginisialisasikan sebuah nilai pada variabel. 

Ketika menggunakan Hapi, kita tidak lagi berurusan dengan stream untuk mendapatkan datanya. Di balik layar, Hapi secara default akan mengubah payload JSON menjadi objek JavaScript. Dengan begitu, Anda tak lagi berurusan dengan JSON.parse()!

Kapan pun client mengirimkan payload berupa JSON, payload tersebut dapat diakses pada route handler melalui properti request.payload. Contohnya seperti ini:
```Javascript
server.route({
    method: 'POST',
    path: '/login',
    handler: (request, h) => {
        const { username, password } = request.payload;
        return `Welcome ${username}!`;
    },
});
```
Contoh json yang diberikan :
```json
{ "username": "harrypotter", "password": "encryptedpassword" }
```
### Response Toolkit
Untuk menambahkan status code :
```Javascript
server.route({
    method: 'POST',
    path: '/user',
    handler: (request, h) => {
        return h.response('created').code(201);
    },
});
```
Parameter h:
```Javascript
// Detailed notation
const handler = (request, h) => {
    const response = h.response('success');
    response.type('text/plain');
    response.header('X-Custom', 'some-value');
    return response;
};
 
// Chained notation
const handler = (request, h) => {
    return h.response('success')
        .type('text/plain')
        .header('X-Custom', 'some-value');
};
```
## RESTApi dengan Nodemon
### Nodemon
>Tools pertama adalah nodemon, ia bisa dikatakan wajib digunakan selama proses pengembangan. Pasalnya, dengan tools ini kita tak perlu menjalankan ulang server ketika terjadi perubahan pada berkas JavaScript. Nodemon akan mendeteksi perubahan kode JavaScript dan mengeksekusi ulang secara otomatis.
```
npm install nodemon --save-dev
```
Di dalam package.json, buat npm runner script baru untuk menjalankan server.js menggunakan nodemon.
```json
"scripts": {
    "start": "nodemon server.js"
 },
```
### ESLint
>ESLint dapat membantu atau membimbing kita untuk selalu menuliskan kode JavaScript dengan gaya yang konsisten. Seperti yang Anda tahu, JavaScript tidak memiliki aturan yang baku untuk gaya penulisan kode, bahkan penggunaan semicolon. Karena itu, terkadang kita jadi tidak konsisten dalam menuliskannya.
```
npm install eslint --save-dev
```
Sebelum digunakan, kita perlu melakukan konfigurasi terlebih dahulu. Caranya dengan menggunakan perintah berikut di Terminal proyek.
```
npx eslint --init
```
Setelah membuat konfigurasi ESLint, selanjutnya kita gunakan ESLint untuk memeriksa kode JavaScript yang ada pada proyek. Namun sebelum itu, kita perlu menambahkan npm runner berikut di dalam berkas package.json:
```json
"scripts": {
  "start": "nodemon server.js",
  "lint": "eslint ./"
},
```
Jalankan perintah di bawah pada Terminal proyek, lalu perhatikan hasilnya.
```
npm run lint
```
### Generate id menggunakan NanoId
```
npm install nanoid@3.x.x
```
>Catatan: Pastikan memasang nanoid dengan versi 3.x.x. Karena jika menggunakan versi terbaru, nanoid tidak dapat digunakan dengan format module CommonJS.

Cara menggunakan nanoid:
```Javascript
const id = nanoid(16);
```
### CORS (Cross-origin resource sharing)
>Cross-origin resource sharing (CORS) adalah mekanisme yang memungkinkan server web untuk memberikan akses terbatas pada sumber daya (resource) yang dimilikinya kepada domain lain yang berbeda (origin) dari domain tempat sumber daya tersebut berasal. Dalam konteks web, "origin" didefinisikan sebagai gabungan dari skema (http atau https), domain, dan nomor port dari URL.
Cara penggunaannya pada Hapi:
Menambahkan options.cors di konfigurasi route:
```Javascript
{
  method: 'POST',
  path: '/notes',
  handler: addNoteHandler,
  options: {
    cors: {
      origin: ['*'],
    },
  },
},
```
Bila ingin cakupannya lebih luas alias CORS diaktifkan di seluruh route yang ada di server, Anda bisa tetapkan CORS pada konfigurasi ketika hendak membuat server dengan menambahkan properti routes.cors. Contohnya seperti ini:
```Javascript
const server = Hapi.server({
  port: 5000,
  host: 'localhost',
  routes: {
    cors: {
      origin: ['*'],
    },
  },
});
```