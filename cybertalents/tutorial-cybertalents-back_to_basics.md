## [Writeup] Cyber Talents: Back to Basics


Ini adalah tutorial dari soal latihan [Cyber Talents: Back to Basics](https://cybertalents.com/challenges/web/back-to-basics) untuk kategori Web Security. Berikut ini adalah penjelasan untuk soal tersebut:

```
not pretty much many options.
No need to open a link from a browser,
there is always a different way
```

Dan soalnya berada pada [tautan ini](http://35.197.254.240/backtobasics). Untuk menyelesaikan soal ini, kita akan menggunakan [HTTPie](https://httpie.org/), namun Anda juga dapat menggunakan aplikasi lainnya sesuai keinginan. Langkah pertama, kita akan melakukan request ke tautan soal tersebut menggunakan HTTPie dengan perintah seperti ini:

```bash
% http "http://35.197.254.240/backtobasics"
HTTP/1.1 301 Moved Permanently
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Length: 194
Content-Type: text/html
Date: Mon, 27 Jul 2020 03:55:44 GMT
Location: http://35.197.254.240/backtobasics/
Server: nginx/1.10.3 (Ubuntu)

<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.10.3 (Ubuntu)</center>
</body>
</html>
```

Bisa terlihat, bahwa kita akan diarahkan (_redirect_) ke halaman tersebut, jika menggunakan tautan tanpa diakhiri dengan **/** (_slash_). Coba kita kirimkan request menggunakan _slash_:

``` bash
% http "http://35.197.254.240/backtobasics/"
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 27 Jul 2020 03:57:48 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<script> document.location = "http://www.google.com"; </script>
```

Kali ini, kita akan diarahkan ke situs pencari google. Jika kita perhatikan, pada header web tersebut, terdapat beberapa metode yang dapat digunakan, yaitu **GET**, **POST**, **HEAD**, dan **OPTIONS**. Kita akan mencoba mengirimkan request dengan metode **POST** dengan menggunakan parameter acak:

```bash
% http "http://35.197.254.240/backtobasics/" "foo=bar"
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 27 Jul 2020 03:46:56 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<!--
var _0x7f88=["","join","reverse","split","log","ceab068d9522dc567177de8009f323b2"];function reverse(_0xa6e5x2){flag= _0xa6e5x2[_0x7f88[3]](_0x7f88[0])[_0x7f88[2]]()[_0x7f88[1]](_0x7f88[0])}console[_0x7f88[4]]= reverse;console[_0x7f88[4]](_0x7f88[5])
-->
```

Ternyata hasilnya, kita tidak di-redirect lagi, dan terdapat sebuah script javascript pada halaman tersebut. Jika diperhatikan, maka flag pada script di atas terdapat pada variabel `_0x7f88[5]` dan kemudian string tersebut akan dibalik (_reverse_) menjadi `2b323f9008ed771765cd2259d860baec`. Nah, itu adalah flag untuk soal ini.

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
