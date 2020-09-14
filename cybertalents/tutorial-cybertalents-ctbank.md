## [Writeup] Cyber Talents: CTBank


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: CTBank](https://cybertalents.com/challenges/forensics/ctbank) yang masuk dalam kategori _digital forensic_. Berikut ini adalah penjelasan untuk soal tersebut:

```
our client bank is under attack, may the logs will help
```

Selain petunjuk tersebut, kita akan diberi petunjuk berupa sebuah file yang dapat kita [unduh pada tautan ini](https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/access.7z). Unduh terlebih dahulu file tersebut, lalu ekstrak menggunakan [7zip](https://www.7-zip.org/):

```bash
% 7z x access.7z
```

Hasilnya berupa sebuah file dengan nama **access.log** yang tipenya adalah teks. Buka file tersebut menggunakan teks editor, maka kita dapat melihat baris berikut ini:

```
10.10.10.77 - - [13/Feb/2020:03:36:21 -0400] "GET /mutillidae/index.php?page=user-info.php&username='%20union%20all%20select%201,String.fromCharCode(%20102,%20108,%2097,%20103,%20123,%2033,%2095,%20108,%2048,%20118,%2051,%2095,%20115,%20113,%20108,%2095,%2033,%20110,%20106,%2051,%2099,%20116,%2033,%2048,%20110,%20125,%2010),3%20--+&password=&user-info-php-submit-button=View%20Account%20Details%20HTTP/1.1%22%20200%209582%20%22http://10.10.10.200/mutillidae/index.php?page=user-info.php&username=something&password=&user-info-php-submit-button=View%20Account%20Details%22%20%22 "Mozilla/5.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.101 Safari/537.36"
```

Jika diperhatikan, baris log tersebut merupakan serangan SQLinjection. Bagian yang perlu diperhatikan adalah yang ini:

```js
String.fromCharCode(%20102,%20108,%2097,%20103,%20123,%2033,%2095,%20108,%2048,%20118,%2051,%2095,%20115,%20113,%20108,%2095,%2033,%20110,%20106,%2051,%2099,%20116,%2033,%2048,%20110,%20125,%2010)
```

Kita akan mengabaikan **%20** pada bagian di atas, karena itu adalah urlencode untuk karakter spasi. Hasilnya seperti ini:

```js
String.fromCharCode(102,108,97,103,123,33,95,108,48,118,51,95,115,113,108,95,33,110,106,51,99,116,33,48,110,125,10)
```

Dari sini, ada banyak cara yang dapat digunakan untuk menerjemahkan bilangan tersebut menjadi karakter ASCII. Jika menggunakan javascript pada terminal, maka caranya seperti ini:

```bash
% js -e "print(String.fromCharCode(102,108,97,103,123,33,95,108,48,118,51,95,115,113,108,95,33,110,106,51,99,116,33,48,110,125,10));"
flag{!_l0v3_sql_!nj3ct!0n}
```

Bisa terlihat bahwa flagnya adalah `flag{!_l0v3_sql_!nj3ct!0n}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
