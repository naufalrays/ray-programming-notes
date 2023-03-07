# Catatan Javascript

## Struktur Data (Object, Array, dll)
### 1. Destructuring object :
```Javascript
const profile = {
    firstName: "John",
    lastName: "Doe",
    age: 18
}
 
// Membuat object
let firstName = "Ray";
let lastName = "Naufal";

const {firstName, age, isMale} = profile; // ERROR, Membuat variabel firstName, age, dan isMale dari object profile
// Tidak dapat mendeklarasikan variabel, karena line 7 sudah mendeklarasikan variabel terlebih dahulu

({firstName, age, isMale} = profile)  // Menginisialisasi nilai baru melalui destructuring object
// Dapat mendeklarasikan array, line 7 akan ketimpa (Destructuring object) 
// Jika tidak menuliskan tanda kurung, tanda kurung kurawal akan membuat JavaScript mengira kita membuat block statement, sementara block statement tidak bisa berada pada sisi kiri assignment.
 
console.log(firstName)
console.log(age)
console.log(isMale)
```

### 2. Membuat variabel baru dari object
```Javascript
const profile = {
    firstName: "John",
    lastName: "Doe",
    age: 18
}
 
const {firstName: localFirstName, lastName: localLastName, age: localAge} = profile; // Membuat variabel localFirstName, localLastName, dan juga localAge
 
// Mencetak Output : 
console.log(localFirstName); // John
console.log(localLastName); // Doe
console.log(localAge); // 18
```

### 3. Membuat variabel baru dari array 
```Javascript
const favorites = ["Seafood", "Salad", "Nugget", "Soup"];
 
const [firstFood, secondFood, thirdFood, fourthFood] = favorites; // Membuat variabel firstFood, secondFood, thirdFood, dan fourthFood
 
// Mencetak Output
console.log(firstFood); // Seafood
console.log(secondFood); // Salad
console.log(thirdFood); // Nugget
console.log(fourthFood); // Soup
```

### 4. Destructuring Array
```Javascript
const favorites = ["Seafood", "Salad", "Nugget", "Soup"];
 
// Membuat variabel myFood dan herFood
let myFood = "Ice Cream";
let herFood = "Noodles";

// Salah
const [myFood, herFood] = favorites; // Error, karena sudah pernah membuat variabel myFood dan herFood

// Benar
[myFood, herFood] = favorites; // Akan menimpa myFood dan juga herFood

// Mencetak Output
console.log(myFood); // Seafood
console.log(herFood); // Salad
```

### 5. Menukar urutan array
```Javascript
let a = 1;
let b = 2;

// Mencetak Output
console.log("Sebelum swap"); // Sebelum swap
console.log("Nilai a: " + a); // Nilai a: 1
console.log("Nilai b: " + b); // Nilai b: 2
 
[a, b] = [b, a] // menetapkan nilai a dengan nilai b dan nilai b dengan nilai a.
 
// Mencetak Output
console.log("Setelah swap"); // Setelah swap
console.log("Nilai a: " + a); // Nilai a: 2
console.log("Nilai b: " + b); // Nilai b: 1 
```

### 4. Jika melakukan destructuring array (Array tidak lengkap)
```Javascript
const favorites = ["Seafood"];
const [myFood, herFood] = favorites // Array index 1 tidak ada, jadi hasilnya undefined

// Solusi: 
// const [myFood, herFood = "Salad"] = favorites 
 
console.log(myFood);
console.log(herFood);
 
/* output:
Seafood
undefined
*/
```

## Function 
### 1. Rest Parameter
```Javascript
function sum(...numbers) { // numbers dalam bentuk array
    let result = 0;
    for (let number of numbers) { // number adalah 1, 2, 3, 4, 5 dan numbers adalah [1, 2, 3, 4, 5]
        result += number; 
        /* 
        0 + 1 = 1, 
        1 + 2 = 3, 
        3 + 3 = 6
        6 + 4 = 10 
        10 + 5 = 15
        */
    }
    return result;
}

// Mencetak Output
console.log(sum(1, 2, 3, 4, 5)); // 15
```

## Object Oriented Programming (OOP)
```
1. Properti merupakan nilai di dalam objek yang menyimpan informasi tentang objek tersebut. (Variabel di dalam Class)
2. Method merupakan fungsi yang menggambarkan aksi yang dapat dilakukan oleh objek tersebut. (Fungsi di dalam Class)
```
### Object : 
```Javascript
const car = {
  // properties
  brand: 'Ford',
  color: 'red',
  maxSpeed: 200,
  chassisNumber: 'f-1',
  // methods
  drive: () => {
    console.log('driving');
  },
  reverse: () => {
    console.log('reversing');
  },
  turn: () => {
    console.log('turning');
  }
}
 
console.log(car.brand); // Ford
console.log(car.color); // red
console.log(car.maxSpeed); // 200
console.log(car.chassisNumber); // f-1
car.drive(); // driving
car.reverse(); // reversing
car.turn(); // turning
```
> JavaScript bukanlah class-based language, melainkan prototype-based language. 
### Object blueprint (Construction Function):
```Javascript
// Object blueprint
function Car(brand, color, maxSpeed, chassisNumber) {
  this.brand = brand;
  this.color = color;
  this.maxSpeed = maxSpeed;
  this.chassisNumber = chassisNumber;
}
 
Car.prototype.drive = function() {
  console.log(`${this.brand} ${this.color} is driving`);
}
 
Car.prototype.reverse = function() {
  console.log(`${this.brand} ${this.color} is reversing`);
}
 
Car.prototype.turn = function() {
  console.log(`${this.brand} ${this.color} is turning`);
}

// Membuat objek mobil dengan constructor function Car
const car1 = new Car('Toyota', 'Silver', 200, 'to-1');
const car2 = new Car('Honda', 'Black', 180, 'ho-1');
const car3 = new Car('Suzuki', 'Red', 220, 'su-1');
 
console.log(car1);
console.log(car2);
console.log(car3);
 
car1.drive();
car2.drive();
car3.drive();
 
/* Output
Car { brand: 'Toyota', color: 'Silver', maxSpeed: 200, chassisNumber: 'to-1' }
Car { brand: 'Honda', color: 'Black', maxSpeed: 180, chassisNumber: 'ho-1' }
Car { brand: 'Suzuki', color: 'Red', maxSpeed: 220, chassisNumber: 'su-1' }
 
Toyota Silver is driving
Honda Black is driving
Suzuki Red is driving
*/
```
```
1. Agar fungsi dapat mengembalikan sebuah objek, maka harus memanggil dengan menambahkan keyword new. Contoh, pada kode di atas, perhatikan cara memanggil fungsi Car untuk membuat objek car1, car2, dan car3.
2. Constructor function dapat memanfaatkan keyword this yang bernilai objek (instance) dirinya sendiri. Keyword this dapat dimanfaatkan untuk mengakses nilai properti atau method dari objek tersebut. Contoh, pada keyword this diatas digunakan untuk menetapkan nilai properti brand dari argumen fungsi. Selain itu, di dalam method drive, digunakan keyword this untuk mendapatkan nilai properti brand dan color.
3. Perlu diingat bahwa constructor function hanya dapat dibuat dengan reguler function. Tidak dapat membuat constructor function dengan arrow function. Arrow function tidak dapat dipanggil dengan keyword new.
```

### Sintaks JavaScript modern (ES6) menawarkan cara membuat constructor function dengan menggunakan keyword class. Hal ini membuat penerapan OOP di JavaScript mirip seperti bahasa pemrograman berbasis class.
```Javascript
class Car {
  constructor(brand, color, maxSpeed, chassisNumber) {
    this.brand = brand;
    this.color = color;
    this.maxSpeed = maxSpeed;
    this.chassisNumber = chassisNumber;
  }
 
  drive() {
    console.log(`${this.brand} ${this.color} is driving`);
  }
 
  reverse() {
    console.log(`${this.brand} ${this.color} is reversing`);
  }
 
  turn() {
    console.log(`${this.brand} ${this.color} is turning`);
  }
}
 
// Membuat objek mobil dengan constructor function Car
const car1 = new Car('Toyota', 'Silver', 200, 'to-1');
const car2 = new Car('Honda', 'Black', 180, 'ho-1');
const car3 = new Car('Suzuki', 'Red', 220, 'su-1');
 
console.log(car1);
console.log(car2);
console.log(car3);
 
car1.drive();
car2.drive();
car3.drive();
 
/* Output
Car { brand: 'Toyota', color: 'Silver', maxSpeed: 200, chassisNumber: 'to-1' }
Car { brand: 'Honda', color: 'Black', maxSpeed: 180, chassisNumber: 'ho-1' }
Car { brand: 'Suzuki', color: 'Red', maxSpeed: 220, chassisNumber: 'su-1' }
 
Toyota Silver is driving
Honda Black is driving
Suzuki Red is driving
*/
```
```
Cara membuat string interpolation : 
(`${variabel_dummy}`) Betul
("${variabel_dummy}") Salah
```

### JavaScript versi ES2022 secara resmi mengenalkan cara dalam menetapkan hak akses private pada properti dan method objek, yakni dengan menambahkan tanda # di awal penamaan properti atau method.
```Javascript
class Car {
  #chassisNumber = null; // enclosing class
 
  constructor(brand, color, maxSpeed) {
    this.brand = brand;
    this.color = color;
    this.maxSpeed = maxSpeed;
    this.#chassisNumber = this.#generateChassisNumber();
 }
 
  get chassisNumber() {
    return this.#chassisNumber;
  }
 
  set chassisNumber(chassisNumber) {
    console.log('you are not allowed to change the chassis number');
  }
 
  // Methods
  drive() {
    console.log(`${this.brand} ${this.color} is driving`);
  }
 
  reverse() {
    console.log(`${this.brand} ${this.color} is reversing`);
  }
 
  turn(direction) {
    console.log(`${this.brand} ${this.color} is turning ${direction}`);
  }
 
  #generateChassisNumber() {
    return `${this.brand}-${Math.floor(Math.random() * 1000)}`;
  }
}
```

### Inheritance

```Javascript
// Superclass
class MailService {
  constructor(sender) {
    this.sender = sender;
  }
 
  sendMessage(message, receiver) {
    console.log(`${this.sender} sent ${message} to ${receiver}`);
  }
}
 
// Subclass contoh inheritance
class WhatsAppService extends MailService {
  sendBroadcastMessage(message, receivers) { // [1]
      this.sendMessage(message, receiver);
    }
  }
 
// Subclass contoh inheritance
class EmailService extends MailService {
  sendDelayedMessage(message, receiver, delay) { // [1]
    setTimeout(() => {
      this.sendMessage(message, receiver);
    }, delay);
  }
}
```
> [1] Di dalam masing-masing subclass, kita bisa mendefinisikan method yang spesifik, seperti sendBroadcastMessage() untuk WhatsAppService dan sendDelayedMessage() untuk EmailService.

Lihatlah bahwa di dalam subclass WhatsAppService dan EmailService kita tetap memiliki akses terhadap method dari superclass melalui keyword this karena subclass mewarisi sifat dari superclass.

Dengan teknik pewarisan seperti ini, akhirnya kita bisa membuat dua objek serupa dengan cara yang jauh lebih efektif.
```Javascript
const whatsapp = new WhatsAppService('+6281234567890');
const email = new EmailService('dimas@dicoding.com');
 
whatsapp.sendMessage('Hello', '+6289876543210');
whatsapp.sendBroadcastMessage('Hello', ['+6289876543210', '+6282234567890']);
whatsapp.sendDelayedMessage(); // Error
 
email.sendMessage('Hello', 'john@doe.com');
email.sendDelayedMessage('Hello', 'john@doe.com', 3000);
email.sendBroadcastMessage(); // Error

```

### Overriding
```Javascript
class MailService {
  constructor(sender) {
    this.sender = sender;
  }
 
  sendMessage(message, receiver) {
    console.log(`${this.sender} sent ${message} to ${receiver}`);
  }

}

class WhatsAppService extends MailService {
  constructor(sender, isBusiness) {
    super(sender); // WAJIB menggunakan super
    this.isBusiness = isBusiness;
  }
  
 
  // Overriding method
  sendMessage(message, receiver) {
    // memanggil method sendMessage pada superclass
    super.sendMessage(message, receiver); // Overriding menggunakan super(Data Superclass)
    console.log(`${this.sender} sent ${message} to ${receiver} via WhatsApp`); // Overriding tetapi tidak menggunakan super(Data Superclass)
  }
}


const mailService = new MailService('someSender');
const whatsappService = new WhatsAppService('+6281234567890', true);

mailService.sendMessage('Hai, apa kabar?', 'Ray');
whatsappService.sendMessage('Hai, apa kabar?', '+62812345678');

/**
* Output:
* someSender sent Hai, apa kabar? to someReceiver
* +6281234567890 sent Hai, apa kabar? to +62812345678
* +6281234567890 sent Hai, apa kabar? to +62812345678 via WhatsApp
*/
```

### Object Composition
```
1. Object composition, dia tidak memperdulikan peran, melainkan kode distrukturkan berdasarkan kemampuan yang dapat dilakukan, seperti buildUI(), buildAPI(), dan deployApp().
2. Agar lebih mudah dalam membuat objek, kita bisa membuat sebuah fungsi sebagai object creator dengan mengomposisikan kemampuan-kemampuan yang dibutuhkan. Di JavaScript, kita bisa secara mudah mengomposisikan objek dengan menggunakan method Object.assign()
```
```Javascript
class Developer {
  constructor(name) {
    this.name = name;
  }
 
  commitChanges() {
    console.log(`${this.name} is committing changes...`);
  }
}
 
function canBuildUI(developer) {
  return {
    buildUI: () => {
      console.log(`${developer.name} is building UI...`);
    }
  }
}
 
function canBuildAPI(developer) {
  return {
    buildAPI: () => {
      console.log(`${developer.name} is building API...`);
    }
  }
}
 
function canDeployApp(developer) {
  return {
    deployApp: () => {
      console.log(`${developer.name} is deploying app...`);
    }
  }
}
 
function createFrontEndDeveloper(name) {
  const developer = new Developer(name);
  return Object.assign(developer, canBuildUI(developer));
}
 
function createBackEndDeveloper(name) {
  const developer = new Developer(name);
  return Object.assign(developer, canBuildAPI(developer));
}
 
function createDevOps(name) {
  const developer = new Developer(name);
  return Object.assign(developer, canDeployApp(developer));
}
 
function createFullStackDeveloper(name) {
  const developer = new Developer(name);
  return Object.assign(developer, canBuildUI(developer), canBuildAPI(developer), canDeployApp(developer));
}
 
const frontEndDeveloper = createFrontEndDeveloper('Fulan');
frontEndDeveloper.commitChanges();
frontEndDeveloper.buildUI();
console.log(`is ${frontEndDeveloper.name} developer? `, frontEndDeveloper instanceof Developer);
 
const backEndDeveloper = createBackEndDeveloper('Fulana');
backEndDeveloper.commitChanges();
backEndDeveloper.buildAPI();
console.log(`is ${backEndDeveloper.name} developer? `, backEndDeveloper instanceof Developer);
 
const devOpsDeveloper = createDevOps('Fulani');
devOpsDeveloper.commitChanges();
devOpsDeveloper.deployApp();
console.log(`is ${devOpsDeveloper.name} developer? `, devOpsDeveloper instanceof Developer);
 
const fullStackDeveloper = createFullStackDeveloper('Fulanah');
fullStackDeveloper.buildUI();
fullStackDeveloper.buildAPI();
fullStackDeveloper.deployApp();
console.log(`is ${fullStackDeveloper.name} developer? `, fullStackDeveloper instanceof Developer);
 
/**
* Output:
* Fulan is committing changes...
* Fulan is building UI...
* is Fulan developer?  true
* Fulana is committing changes...
* Fulana is building API...
* is Fulana developer?  true
* Fulani is committing changes...
* Fulani is deploying app...
* is Fulani developer?  true
* Fulanah is building UI...
* Fulanah is building API...
* Fulanah is deploying app...
* is Fulanah developer?  true
*/
```
### Built-in Class

Date: 
```Javascript
const date = new Date();
 
const timeInJakarta = date.toLocaleString('id-ID', {
  timeZone: 'Asia/Jakarta',
});
 
const timeInTokyo = date.toLocaleString('ja-JP', {
  timeZone: 'Asia/Tokyo',
});
 
const timeInMakassar = date.toLocaleString('id-ID', {
  timeZone: 'Asia/Makassar',
});
 
console.log(timeInJakarta);
console.log(timeInTokyo);
console.log(timeInMakassar);
 
/**
* Output:
* 22/12/2022 10.37.14
* 2022/12/22 12:37:14
* 22/12/2022 11.37.14
*/
```
Dengan class Array, kita bisa membuat struktur data dalam bentuk array :
```Javascript
const myArray = new Array('a', 'b', 'c', 'a', 'b', 'c');
console.log(myArray); // ['a', 'b', 'c', 'a', 'b', 'c']
```

## Functional Programming

### Paradigma Functional Programming
Cara lama (Gaya penulisan kode imperatif) : 
```Javascript
const names = ['Harry', 'Ron', 'Jeff', 'Thomas'];

const newNamesWithExcMark = [];

for(let i = 0; i < names.length; i++) {
  newNamesWithExcMark.push(`${names[i]}!`);
}

console.log(newNamesWithExcMark);

/* output:
   [ 'Harry!', 'Ron!', 'Jeff!', 'Thomas!' ]
*/
```
Gaya penulisan kode deklaratif:
```Javascript
const names = ['Harry', 'Ron', 'Jeff', 'Thomas'];

const newNamesWithExcMark = names.map((name) => `${name}!`);

console.log(newNamesWithExcMark);

/* output:
 * [ 'Harry!', 'Ron!', 'Jeff!', 'Thomas!' ]
 */
```

### Cara cepat menambahkan object key + value :
```Javascript
const createPersonWithAge = (age, ss, person) => {
  return {...person, age , ss};
};

const person = {
  name: 'Bobo'
};

const newPerson = createPersonWithAge(18,19, person);

console.log({
  person,
  newPerson
});

/**
 * Output:
 *  { 
 *    person: { name: 'Bobo' },
 *    newPerson: { name: 'Bobo', age: 18 } 
 *  }
*/
```
#### Immutability
Cara lama (Bukan Pure FUNCTION):
```Javascript
const user = {
    firstname: 'Harry',
    lastName: 'Protter', // ups, typo!
}

const renameLastNameUser = (newLastName, user) => {
    user.lastName = newLastName;
}

renameLastNameUser('Potter', user);

console.log(user);

/**
 * output:
 * { firstname: 'Harry', lastName: 'Potter' }
 * 
 */
```
Pure FUNCTION : 
```Javascript
const user = {
    firstname: 'Harry',
    lastName: 'Protter', // ups, typo!
}

const createUserWithNewLastName = (newLastName, user) => {
    return { ...user, lastName: newLastName }
}

const newUser = createUserWithNewLastName('Potter', user);

console.log(newUser);

/**
 * output:
 * { firstname: 'Harry', lastName: 'Potter' }
 * 
 */
```
#### Rekursif
Cara bukan rekursif: 
```Javascript
const countDown = start => {
  do {
    console.log(start);
    start -=1;
  }
  while(start > 0);
};
 
countDown(10);
```
Cara rekursif:
```Javascript
const countDown = start => {
  console.log(start);
  if(start > 0) countDown(start-1);
};

countDown(10);
```
### Reusable Function
#### Fungsi Array Map :
```Javascript
const newArray = ['Harry', 'Ron', 'Jeff', 'Thomas'].map((name) => { return `${name}!`});

console.log(newArray);

/**
 * [ 'Harry!', 'Ron!', 'Jeff!', 'Thomas!' ]
 * 
 */
```
#### Fungsi .filter() : 
```Javascript
const truthyArray = [1, '', 'Hallo', 0, null, 'Harry', 14].filter((item) => Boolean(item));

console.log(truthyArray);

/**
 * output:
 * [ 1, 'Hallo', 'Harry', 14 ]
 * 
 */
```

Contoh lain fungsi filter :
```Javascript
const students = [
  {
    name: 'Harry',
    score: 60,
  },
  {
    name: 'James',
    score: 88,
  },
  {
    name: 'Ron',
    score: 90,
  },
  {
    name: 'Bethy',
    score: 75,
  }
];

const eligibleForScholarshipStudents = students.filter((student) => student.score > 85);

console.log(eligibleForScholarshipStudents);

/**
 * output:
 * [ { name: 'James', score: 88 }, { name: 'Ron', score: 90 } ]
 * 
 */
```
#### Array some :
```Javascript
const array = [1, 2, 3, 4, 5];
const even = array.some(element => element % 2 === 0);

console.log(even);
/** 
output true
**/
```
#### Array find :
```Javascript
const students = [
  {
    name: 'Harry',
    score: 60,
  },
  {
    name: 'James',
    score: 88,
  },
  {
    name: 'Ron',
    score: 90,
  },
  {
    name: 'Bethy',
    score: 75,
  }
];

const findJames = students.find(student => student.name === 'James');
console.log(findJames);

/**
output
{ name: 'James', score: 88 }
**/
```
#### Array sort :
```Javascript
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// output: [ 'Dec', 'Feb', 'Jan', 'March' ]

const array1 = [1, 30, 4, 1000, 101, 121];
array1.sort();
console.log(array1);
// output: [ 1, 1000, 101, 121, 30, 4 ]
```
#### Buat array sort sendiri :
```Javascript
const array1 = [1, 30, 4, 1000];

const compareNumber = (a, b) => {
  return a - b;
};
const sorting = array1.sort(compareNumber);
console.log(sorting);

/**
output
[ 1, 4, 30, 1000 ]
**/ 
```
```
Pada compare function, fungsi akan membandingkan 2 nilai yang akan menghasilkan 3 result yaitu negatif (-), 0, dan positif (+).
1. Jika, negative maka `a` akan diletakkan sebelum `b`
2. Jika, positive maka `b` akan diletakkan sebelum `a`
3. Jika, 0 maka tidak ada perubahan posisi.
```
#### Array foreach:
Cara imperatif :
```Javascript
const names = ['Harry', 'Ron', 'Jeff', 'Thomas'];
 
for(let i = 0; i < names.length; i++) {
  console.log(`Hello, ${names[i]}!`);
}
```
Cara deklaratif:
```Javascript
const names = ['Harry', 'Ron', 'Jeff', 'Thomas'];
 
names.forEach((name) => {
  console.log(`Hello, ${name}!`);
});
 
/**
 * output:
 * Hello, Harry!
 * Hello, Ron!
 * Hello, Jeff!
 * Hello, Thomas!
 * 
 */
```
```
Namun, ketahuilah bahwa ketika menggunakan forEach, kita tidak bisa menggunakan operator break atau continue pada proses perulangan (Anda bisa melakukannya pada perulangan for). Hal ini juga berlaku ketika pada fungsi map dan filter.
```
```Javascript
const names = ['Harry', 'Ron', 'Jeff', 'Thomas'];
 
for(let i = 0; i < names.length; i++) {
  if(names[i] === 'Jeff') continue; // Bisa!
  
  console.log(`Hello, ${names[i]}!`);
}
 
names.forEach((name) => {
  if(name === 'Jeff') continue; // Tidak Bisa!
  console.log(`Hello, ${name}`);
});
```
#### Contoh filter + map
```Javascript
/**
 * TODO:
 * Buatlah variabel greatAuthors yang merupakan array
 * berdasarkan hasil filter() dan map() dari books:
 *   - Gunakan fungsi filter untuk mengembalikan nilai item books
 *     yang hanya memiliki nilai sales lebih dari 1000000.
 *   - Gunakan fungsi map pada books yang sudah ter-filter,
 *     untuk mengembalikan nilai string dengan format:
 *     - `${author} adalah penulis buku ${title} yang sangat hebat!`
 *
 * Catatan: Jangan ubah nilai atau struktur dari books
 */

const books = [
  { title: 'The Da Vinci Code', author: 'Dan Brown', sales: 5094805 },
  { title: 'The Ghost', author: 'Robert Harris', sales: 807311 },
  { title: 'White Teeth', author: 'Zadie Smith', sales: 815586 },
  { title: 'Fifty Shades of Grey', author: 'E. L. James', sales: 3758936 },
  { title: 'Jamie\'s Italy', author: 'Jamie Oliver', sales: 906968 },
  { title: 'I Can Make You Thin', author: 'Paul McKenna', sales: 905086 },
  { title: 'Harry Potter and the Deathly Hallows', author: 'J.K Rowling', sales: 4475152 },
];

// TODO
var greatAuthors = books.filter((book) => book.sales > 1000000).map((book) => `${book.author} adalah penulis buku ${book.title} yang sangat hebat!`);
/**
 * Jangan hapus kode di bawah ini
 */

module.exports = { books, greatAuthors };
```
### Penanganan Error
#### Try-catch finally
```
Selain try dan catch, ada satu blok lagi yang ada dalam mekanisme error handling pada JavaScript, yaitu finally. Blok finally akan tetap dijalankan tanpa peduli apa pun hasil yang terjadi pada blok try-catch.
```
```Javascript
try {
    console.log("Awal blok try");
    console.log("Akhir blok try");
} catch (error) {
    console.log("Baris ini diabaikan");
} finally {
    console.log("Akan tetap dieksekusi");
}
 
/* output
Awal blok try
Akhir blok try
Akan tetap dieksekusi
*/
```
#### Cara membuat error pada try-catch menggunakan throw
```Javascript
let json = '{ "age": 20 }';
 
try {
    let user = JSON.parse(json);
 
    if (!user.name) {
        throw new SyntaxError("'name' is required.");
    }
 
    console.log(user.name); // undefined
    console.log(user.age);  // 20
} catch (error) {
    console.log(`JSON Error: ${error.message}`);
}
 
/* output
JSON Error: 'name' is required.
*/
```
#### Cara menampilkan error sesuai error yang muncul (If statement):
```Javascript
try {
    // ...
} catch (error) {
    if (error instanceof SyntaxError) {
        console.log(`JSON Error: ${error.message}`);
    } else if (error instanceof ReferenceError) {
        console.log(error.message);
    } else {
        console.log(error.stack);
    }
}
```
#### Membuat kelas error sendiri
```Javascript
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}
 
let json = '{ "age": 30 }';
 
try {
    let user = JSON.parse(json);
 
    if (!user.name) {
        throw new ValidationError("'name' is required.");
    }
    if (!user.age) {
        throw new ValidationError("'age' is required.");
    }
 
    console.log(user.name);
    console.log(user.age);
} catch (error) {
    if (error instanceof SyntaxError) {
        console.log(`JSON Syntax Error: ${error.message}`);
    } else if (error instanceof ValidationError) {
        console.log(`Invalid data: ${error.message}`);
    } else if (error instanceof ReferenceError) {
        console.log(error.message);
    } else {
        console.log(error.stack);
    }
}
 
/* output
Invalid data: 'name' is required.
*/
```
### Asynchronous 
#### Menggunakan callback : 
```Javascript
function getUsers(isOffline, callback) {
  // simulate network delay
  setTimeout(() => {
    const users = ['John', 'Jack', 'Abigail'];
 
    if (isOffline) {
      callback(new Error('cannot retrieve users due offline'), null);
      return;
    }
 
    callback(null, users);
  }, 3000);
}
 
function usersCallback(error, users) {
  if (error) {
    console.log('process failed:', error.message);
    return;
  }
 
  console.log('process success:', users);
}
 
getUsers(false, usersCallback); // process success: ['John', 'Jack', 'Abigail']
getUsers(true, usersCallback); // process failed: cannot retrieve users due offline
```
#### Menggunakan Promise :
```Javascript
function getUsers(isOffline) {
  // return a promise object
  return new Promise((resolve, reject) => {

    // simulate network delay
    setTimeout(() => {
      const users = ['John', 'Jack', 'Abigail'];
    
      if (isOffline) {
        reject(new Error('cannot retrieve users due offline'));
        return;
      }

      resolve(users);
    }, 3000);
  });
}

getUsers(false)
  .then(users => console.log(users))
  .catch(err => console.log(err.message));
```
#### Menggunakan callback dan mengubah ke promise
```Javascript
/**
 * @TODO
 * Ubahlah fungsi asynchronous callback-based getProvinces menjadi Promise-based.
 *
 * Catatan:
 * - Anda boleh menggunakan util.promisify untuk mengubah fungsi callback-based menjadi Promise-based.
 * - Jika nama fungsinya berubah, sesuaikan nilai yang diekspor tanpa mengubah nama properti di akhir berkas ini.
 */

function getProvinces(countryId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (countryId === 'id') {
        resolve([
          { id: 'id-jk', name: 'Jakarta' },
          { id: 'id-bt', name: 'Banten' },
          { id: 'id-jr', name: 'Jawa Barat' },
        ]);
      } else {
        reject(new Error('Country not found'));
      }
    }, 1000);
  });
}

/* function getProvinces(countryId, callback) {
  setTimeout(() => {
    if (countryId === 'id') {
      callback(null, [
        { id: 'id-jk', name: 'Jakarta' },
        { id: 'id-bt', name: 'Banten' },
        { id: 'id-jr', name: 'Jawa Barat' },
      ]);
      return;
    }

    callback(new Error('Country not found'), null);
  }, 1000);
} */

module.exports = { getProvinces: getProvinces };
```

#### Dengan menggunakan async-await
```Javascript
async function watchMovie() {
  try {
    const money = await withDrawMoney(10);
    const ticket = await buyCinemaTicket(money);
    const result = await goInsideCinema(ticket);
 
    console.log(result);
  } catch (error) {
    console.log(error.message);
  }
}
 
watchMovie().then(() => console.log('done'));
 
/** output */
// enjoy the movie
// done
```
### NPM (Node Package Manager)
```Javascript
npm install lodash // Untuk menginstall dependency (lodash)
npm install lodash --save-dev // Untuk menginstall dev  dependency (lodash)
npm uninstall lodash // Untuk menghapus dependency (lodash)
npm uninstall lodash --save-dev // Untuk menghapus dev dependency (lodash)
```

#### Package Lodash
```Javascript
import _ from 'lodash';
 
const myArray = [1, 2, 3, 4];
const sum = _.sum(myArray);
 
console.log(sum);
 
/* output
10
*/
```
