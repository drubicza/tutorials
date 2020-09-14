## [Writeup] Cyber Talents: Cheers


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: Cheers](https://cybertalents.com/challenges/web/cheers) yang termasuk ke dalam kategori web security. Berikut ini adalah petunjuk dari soal tersebut:

```
Go search for what cheers you up
```

Adapun soalnya, dapat dilihat pada [tautan ini](http://ec2-54-93-122-202.eu-central-1.compute.amazonaws.com/ch33r5/). Coba kita buka tautan tersebut menggunakan [HTTPie](https://https://httpie.org/):

```bash
% http "http://ec2-54-93-122-202.eu-central-1.compute.amazonaws.com/ch33r5/"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 00:42:11 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<title>Warm Up Guys</title>
<link href='http://fonts.googleapis.com/css?family=Patua+One' rel='stylesheet' type='text/css'>
<font face="Patua One" size=4>

<br>Ops!!  Can You Fix This Error For Me Please ??! <br><br><br />
<b>Notice</b>:  Undefined index: welcome in <b>/var/www/html/ch33r5/index.php</b> on line <b>14</b><br />
```

Bisa terlihat bahwa halaman tersebut berisi pesan error ini:

```
Notice: Undefined index: welcome in /var/www/html/ch33r5/index.php on line 14
```

Berarti kita harus menambahkan parameter **welcome** pada request yang kirimkan ke halaman tersebut:

```bash
% http "http://ec2-54-93-122-202.eu-central-1.compute.amazonaws.com/ch33r5/?welcome=1"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 00:45:07 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<title>Warm Up Guys</title>
<link href='http://fonts.googleapis.com/css?family=Patua+One' rel='stylesheet' type='text/css'>
<font face="Patua One" size=4>

Hello !!! <br><br />
<b>Notice</b>:  Undefined index: gimme_flag in <b>/var/www/html/ch33r5/index.php</b> on line <b>19</b><br />
```

Ternyata masih muncul error, namun kali ini adalah parameter **gimme_flag** yang tidak ada:

```bash
Notice:  Undefined index: gimme_flag in /var/www/html/ch33r5/index.php on line 19
```

Kita akan menambahkan parameter tersebut pada request yang akan kita kirimkan ke halaman tersebut:

```bash
% http "http://ec2-54-93-122-202.eu-central-1.compute.amazonaws.com/ch33r5/?welcome=1&gimme_flag=1"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 00:47:27 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<title>Warm Up Guys</title>
<link href='http://fonts.googleapis.com/css?family=Patua+One' rel='stylesheet' type='text/css'>
<font face="Patua One" size=4>

Hello !!! <br><br>FLAG{k33p_c4lm_st4rt_c0d!ng}
```

Nah, kali ini sudah tidak muncul pesan error, dan kita mendapatkan flagnya `FLAG{k33p_c4lm_st4rt_c0d!ng}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
