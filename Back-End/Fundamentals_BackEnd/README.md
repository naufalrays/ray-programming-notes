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