---
published: true
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
**Parameter ke-1** adalah tipe algoritma yang ingin kita gunakan, algoritma yang didukung oleh crypto sangat bergantung kepada library yang didukung openssl, untuk melihat algoritma apa saja yang di-support oleh openssl, ketikkan perintah ini pada layar console,
```
openssl list -cipher-algorithms
```

**Parameter ke-2** adalah `secret key` atau `salt`\
**Parameter ke-3** adalah key [Initialization Vector] (https://en.wikipedia.org/wiki/Initialization_vector)\
**Parameter ke-4** adalah parameter yang sifatnya opsional.

Oke langsung kita coba buat saja fungsi enkripsinya.

Pertama, algoritma yang kita gunakan pada percobaan ini adalah `AES-256-CBC`.
Lalu pada input parameter _key salt_ -nya bisa dari salah satu dari 5 tipe data berikut: _String, Buffer, TypedArray, DataView atau KeyObject_.

**Notes:** Harap diketahui bahwa fungsi enkripsi/dekripsi _Initilization Vector_ ini hanya menerima _key/salt_ yang berjumlah 256 bits atau 32 karakter. 1 bytes = 1 karakter. Jika lebih atau kurang dari itu maka akan terjadi _exception_.

### Contoh Key dengan tipe data String
Contoh: `'1234567890abcdef1234567890abcdef' // <-- 32 karakter`

Tapi, jika kita lihat rangkaian string di atas, tingkat _randomness_ datanya masih rendah. Masih sangat mudah terbaca atau diprediksi oleh orang lain. Sehingga bisa membuka celah keamanan di program aplikasi kita. Jadi cara penulisan seperti ini (hardcoded) sangat tidak direkomendasikan.

Banyak metode untuk meningkatkan _randomness_ data, namun pada tutorial kali ini, kita hanya akan menggunakan salah satu metode dari crypto, yaitu `randomBytes()`.

`randomBytes()` ini akan menghasilkan random bytes data bertipe Buffer.

```
crypto.randomBytes(10)
```
Untuk mencapai key yang berjumlah 256 bits, kita bisa rubah value-nya menjadi 32. Sehingga:
```
crypto.randomBytes(32);
```

Begitupun juga untuk parameter `iv`,
```
crypto.randomBytes(16);
```
Sehingga untuk meng-generate sebuah string yang terenkripsi, kita bisa menuliskannya sebagai berikut:
```
const cipher = createCipheriv('AES-256-CBC', crypto.randomBytes(32), crypto.randomBytes(16));
cipher.update('Halo Dunia', 'utf8', 'base64');
cipher.final('base64');
```

Begitupun juga dengan fungsi Dekripsi:
```
const decipher = createDeipheriv('AES-256-CBC', crypto.randomBytes(32), crypto.randomBytes(16));
decipher.update('Halo Dunia', 'utf8', 'base64');
decipher.final('base64');
```
**Notes:** Agar _secret key_ maupun _nonce (iv) key_ bisa disimpan ke sebuah media peyimpanan, Kita bisa meng- _encoding_ _returned value_ dari method `randomBytes()` yang berupa object Buffer, ke sebuah string dengan salah satu format _character encoding_ di bawah ini:
- utf8
- utf16le
- latin1
- base64
- hex
- ascii

Demikian, semoga bermanfaat. Cheerio :)