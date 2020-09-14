## [Writeup] Cyber Talents: Secret Browser


Tutorial kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Secret Browser](https://cybertalents.com/challenges/web/secret-browser) yang masuk ke dalam kategori web security. Berikut ini adalah petunjuk untuk soal tersebut:

```
The company employees is using company special
browser to view the website content.
```

Kita dapat mengakses halaman soal kali ini pada [tautan ini](http://35.197.254.240/secret-browser/). Kita akan menggunakan [HTTPie](https://httpie.org) untuk mengerjakan soal ini. Anda dapat menggunakan tools lain jika mau. Langkah pertama adalah mengakses halaman tersebut:

```bash
% http "http://35.197.254.240/secret-browser/"
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 01:01:28 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>PublicTradeCo company for trading</title>

    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>

      <h1> Welcome Guest , your are not using our company browser </h1>
      <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
      <!-- Include all compiled plugins (below), or include individual files as needed -->
      <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
      <script>

      </script>
    </body>
  </html>
```

Pada halaman tersebut muncul pesan bahwa kita tidak menggunakan broser perusahaan:

```
Welcome Guest , your are not using our company browser
```

Jika kita perhatikan, pada _title_ halaman tersebut tertulis **PublicTradeCo company for trading**. Berarti, nama perusahaannya adalah **PublicTradeCo**. Kita akan menggunakan _string_ tersebut sebagai header [User-Agent](https://en.wikipedia.org/wiki/User_agent). Coba kita kirimkan kembali request menggunakan header tersebut:

```bash
% http "http://35.197.254.240/secret-browser/" "User-Agent: PublicTradeCo"
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 01:05:50 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked
x-flag: W3lcomeC0mpanyUs3R

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>PublicTradeCo company for trading</title>

    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>

      <h1> Welcome employee , the flag you are looking for is here somewhere </h1>
      <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
      <!-- Include all compiled plugins (below), or include individual files as needed -->
      <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
      <script>

      </script>
    </body>
  </html>
```

Kali ini, kita berhasil mendapatkan pesan:

```
Welcome employee , the flag you are looking for is here somewhere
```

Namun, flagnya tidak kita temukan pada halaman tersebut. Flagnya berada pada response header. Perhatikan respon dari server berikut ini:

```
HTTP/1.1 200 OK
Allow: GET, POST, HEAD,OPTIONS
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 01:05:50 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked
x-flag: W3lcomeC0mpanyUs3R
```

Flag untuk soal kali ini adalah `W3lcomeC0mpanyUs3R`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
