---
published: false
---
## Membuat Enkripsi-Dekripsi dengan Crypto Cipheriv

Pada kali ini saya akan share tutorial bagaimana cara mengimplementasikan salah satu fungsi dari Crypto, yaitu Cipheriv.

Hal pertama kali yang harus kamu lakukan adalah membuat project baru NodeJs, buat direktori baru bernama `encryption`. Lalu buat sebuah file baru di dalamnya bernama `encrypt.js`.  Lalu lakukan inisialisasi project dengan perintah:

```
npm init
```

Jika sudah, buka file encrypt.js lalu lakukan import crypto library:
```
import { createCipheriv, createDecipheriv } from 'crypto';
```

Yang perlu kita ketahui bahwa fungsi `createCipheriv()` maupun `createDecipheriv` memiliki beberapa parameter yang harus dipenuhi agar fungsi enkripsi-dekripsi yang kita buat berjalan sebagaimana mestinya. 

`crypto.createCipheriv(algorithm, key, iv[, options])`

`crypto.createDecipheriv(algorithm, key, iv[, options])`

**Keterangan:**\
Parameter pertama adalah tipe algoritma yang ingin kita gunakan,\
Parameter kedua adalah `secret key` atau `salt`\
Parameter ketiga adalah [Initialization Vector](https://en.wikipedia.org/wiki/Initialization_vector)\
Parameter keempat adalah parameter sifatnya opsional.

Oke langsung kita coba buat saja enkripsinya.

Pertama, algoritma yang kita gunakan pada percobaan ini adalah `aes-256-cbc`.
Lalu untuk menggenerate key salt kita bisa menggunakan `Buffer.from()`:
```
(Buffer.from(crypto.randomBytes(32), 'utf8’)).toString('base64').slice(0,32)
```
P.S. Harus diketahui bahwa agar proses enkripsi-dekripsi menggunakan Initialization Vectornya bisa berjalan, panjang key pada parameter `key` atau `salt` nya minimal harus sebanyak 32 karakter.

Lalu untuk menggenerate parameter Initilialization Vector kita bisa menggunakan `Buffer.from()` juga:
```
Buffer.from(crypto.randomBytes(16))
```

