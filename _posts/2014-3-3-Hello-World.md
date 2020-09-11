---
layout: post
title: 'Membuat ''salt'' dengan library NodeJs (Crypto, Buffer) dan OpenSSL'
published: true
---

Hi, kali ini saya akan share contoh membuat _salt_ yang baik agar nantinya bisa digunakan untuk berbagai keperluan dalam membangun aplikasi seperti:
- Enkripsi dan Dekripsi password
- _Secret_ JsonWebToken
- Application ID
- dll

Untuk format encoding _output hash_-nya, ada banyak format encoding yang bisa kita pergunakan, namun pada umumnya yang sering digunakan adalah:
- Base 16 (Hex)
- Base 32
- Base 64

Informasi mengenai Base64 bisa kamu lihat di sini [Base64](https://en.wikipedia.org/wiki/Base64).\
Pada contoh kali ini, saya hanya akan menggunakan format encoding Base64.

### Crypto
```
crypto.createHash('sha256').update(String('1234!@#$%')).digest('base64')

//Output: NFrGkV0VBrdn/4Zeli7MFyn/8zI6QrHRCMLyzbceRwM=
```
### Buffer
```
(Buffer.from(crypto.randomBytes(32), 'utf8')).toString('base64');

//Output: QjGCzL1fC3zrGe3LlVyA+1/1JA+CVVxQm2/pqICBE7w=
```
### OpenSSL
Pertama-tama buka aplikasi console (CLI) di sistem operasi Linux kamu, lalu ketik:
```
$ openssl rand -base64 12

//Output: UvM9j9LMttg7Eni2
```
Nah setelah itu kamu bisa simpan string _salt_ nya di suatu tempat yang aman, yang menurut kamu tidak akan mudah diakses oleh orang lain. 
Misalnya seperti di [_environment variable_](https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/).

P.s. Jangan lupa untuk masing-masing library harus sudah ada terlebih dahulu di aplikasi atau sistem operasi kamu. 
Untuk Crypto kamu harus import dulu librarynya menggunakan import atau require().
```
import crypto from 'crypto'
```
Untuk OpenSSL, kamu bisa melakukan instalasi sesuai dengan _package manager_ masing-masing yang terinstall di sistem operasi kamu.
Sekian tips saya kali ini, semoga bermanfaat. Cheerio :)

