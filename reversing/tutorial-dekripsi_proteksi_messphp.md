
Dekripsi Proteksi MessPHP


Tutorial singkat kali ini akan membahas cara melakukan dekripsi script yang diproteksi menggunakan MessPHP. Script kali ini dapat diakses pada [tautan ini](https://ghostbin.com/paste/FlW4o). Script tersebut dapat kita bagi menjadi dua bagian yaitu yang pertama merupakan script yang diproteksi mengggunakan messPHP sedangkan yang kedua memiliki tanda berikut ini:

```html
<!-- HTML Encryption FREAKZBROTHERS -->
```

Untuk bagian tersebut, kita dapat dengan mudah melakukan decode menggunakan _developer tools_ bawaan browser chrome. Caranya, jalankan browser chrome, lalu tekan tombol **Ctrl + Shift + i** pada keyboard. Setelah itu, pilih tab **Console** lalu masukkan bagian yang dimulai dengan kata `unescape` hingga akhir seperti ini:

```javascript
unescape('%3c%21%XMR%20%3c ... %2f%68%74%6d%6c%3e')
```

Setelah itu tinggal tekan tombol **enter** pada keyboard, maka bagian tersebut akan ter-decode seperti ini:

```html
<!%XMR <title>%XMRtle>
  <meta charset="utf-8" />
    <link rel="apple-touch-icon" sizes="76x76" href="img/apple-icon.png">
    <link rel="icon" type="image/png" href="img/favicon.png">
    <meta http-equiv="X-U%XMRhrome=1" />
    <meta content='width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0, shrink-to-fit=no' name='viewport' />
    <link href="https://fonts.googleapis.com/css?family=Montserrat:400,700,200" rel="stylesheet" />
    <script src="https://kit.fontawesome.com/a2ddabd525.js" crossorigin="anonymous"></script>
    <link href="css/bootstrap.min.css" rel="stylesheet" />
    <link href="css/paper-dashboard.css?v=2.0.0" rel="stylesheet" />
    <link href="demo/demo.css" rel="stylesheet" />
</head>
<body>
<div id="login_div">
  <div class="col-lg-12 ml-auto">
    <center>
      <img src="img/logo-small.png" width="27%"><br><br>
      <div class="col-md-3">
        <label>Username: </label>
        <input type="email" class="form-control" placeholder="Username" autocomplete="off" id="email_field">
      </div><br>
      <div class="col-md-3">
        <label>Password: </label>
        <input type="password" class="form-control" placeholder="Password" autocomplete="off" id="password_field">
      </div><br>
      <div class="col-md-3">
        <button class="btn btn-success btn-block" onclick="login()">LO%XMR</center>
  </div>
</div>
<!--------------------------- TOK%XMR-->
<div id="user_div">
  <div class="col-lg-12 ml-auto">
    <center>
      <form action="" method="post">
      <img src="img/logo-small.png" width="27%"><br><br>
      <div class="col-md-3">
        <label>Token : </label>
        <input type="text" class="form-control" placeholder="%XMRd">
      </div><br>
      <div class="col-md-3">
        <input type="submit" class="btn btn-success btn-block" name="submit" value="%XMRdiv>
      </form>
    </center>
  </div>
</div>
  </center>
</div>
  <!--   %XMRrc="https://www.gstatic.com/firebasejs/7.14.2/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/7.14.2/firebase-analytics.js"></script>
<script src="https://www.gstatic.com/firebasejs/4.8.1/firebase.js"></script>
<script>
  // Your web app's %XMRrebase%XMRbpSwl%XMR   auth%XMRm",
    databaseURL: "https://fb-4ee93.firebaseio.com",
    project%XMRet: "fb-4ee93.appspot.com",
    messagingSender%XMR"1:278481761977:web:5109374442b118dcd88d68",
    measurement%XMRnitialize %XMRpp(firebase%XMR;
</script>
    <script src="index.js"></script>
</body>
</html>
```

Selanjutnya, kita akan melakukan dekripsi terhadap bagian yang diproteksi menggunakan messPHP. Berikut ini adalah bagian tersebut yang telah diformat dengan baik:

```php
<?php
/* This file was protected by MessPHP v1.0 at http://lombokcyber.com/en/detools/mess-php-obfuscator */
$m2118d22d991cc8bfb66304d5bd2ee973 = xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii('088116101097');
$m6a4a7423907f51c2c734d4d465cc4547 = xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii('116114105109');
$mdce2462bf288974f3cdad3ccf53bcfaa = xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii('101110099114121112116');
$me570850cdc97d1d0b4000087eae8b8e8 = new $m2118d22d991cc8bfb66304d5bd2ee973(xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii('048055057057053048054049056051098051053099098057101049099100097099099049100056101050054099053049'));
error_reporting(0);
eval($m6a4a7423907f51c2c734d4d465cc4547($me570850cdc97d1d0b4000087eae8b8e8->$mdce2462bf288974f3cdad3ccf53bcfaa("XqsgdgADQj++lPGZMyvkV0Kb3cIPGK4SpENmmEUVdkdd87NqMtJjfL9EVR43td33XuelcTly2ajzGQIbRHjNRxJlR1qDmo1zFgK9Op4ejgKYmSpRAqwgTiWPynuD5Kzkd8s4ex+VMCJU5k+ngK2ek5QRco2gL9RIIQ5y4CyR2Ak9a0JSo1ZLbiHvc+aDk64uRJAuFKITVfWAAuJy/nh3M+IRDqBdSk2KFHAVpxmi2sP795HwdyN2oqoFejo5ZfbNadtjtfOBUaKPYEmCif37LzN4gOoLvp9KlYX/2U+fYNpbdqPsjD0IaPIRODoryorSOAl8mtKH24G6bvDX62wLooY59WDfDm6bIWpt3e3Lp1mooXo3gom7mCClh+kL5v4A29aonI80DrB61frkZvzVqM6XcYiDMc7azwCwgOdzFvKNM4k+mofh9ju/V+sWB7duF3l6gyQEttYhdsl7DZIbuTmdac5W8batbaoPSl7biCE=")));
class Xtea
{
    private $key;
    private $cbc = true;
    function __construct($mb7d5f48227eab3385ddfff1e6a5d4cff)
    {
        $this->key_setup($mb7d5f48227eab3385ddfff1e6a5d4cff);
    }
    public function check_implementation()
    {
        $Xtea = new Xtea("");
        $m0934c81c21fa520a8e3d6ce21dfd76c6 = array(
            array(
                array(
                    0x00000000,
                    0x00000000,
                    0x00000000,
                    0x00000000
                ) ,
                array(
                    0x41414141,
                    0x41414141
                ) ,
                array(
                    0xed23375a,
                    0x821a8c2d
                )
            ) ,
            array(
                array(
                    0x00010203,
                    0x04050607,
                    0x08090a0b,
                    0x0c0d0e0f
                ) ,
                array(
                    0x41424344,
                    0x45464748
                ) ,
                array(
                    0x497df3d0,
                    0x72612cb5
                )
            ) ,
        );
        $m767c4d3425474ddf310892258136eae4 = true;
        foreach ($m0934c81c21fa520a8e3d6ce21dfd76c6 AS $m22ccc35cc89f27579f7a4d252b7c3faa)
        {
            $mb7d5f48227eab3385ddfff1e6a5d4cff = $m22ccc35cc89f27579f7a4d252b7c3faa[0];
            $m0d7d4a6c3a4b82a626f515a3e0ea2e38 = $m22ccc35cc89f27579f7a4d252b7c3faa[1];
            $m17a700bfdacd81b54034ba996377097e = $m22ccc35cc89f27579f7a4d252b7c3faa[2];
            $Xtea->key_setup($mb7d5f48227eab3385ddfff1e6a5d4cff);
            $mafefa4846b0ba586edb703328cc3a8e1 = $Xtea->block_encrypt($m22ccc35cc89f27579f7a4d252b7c3faa[1][0], $m22ccc35cc89f27579f7a4d252b7c3faa[1][1]);
            if ((int)$mafefa4846b0ba586edb703328cc3a8e1[0] != (int)$m17a700bfdacd81b54034ba996377097e[0] || (int)$mafefa4846b0ba586edb703328cc3a8e1[1] != (int)$m17a700bfdacd81b54034ba996377097e[1])
            {
                $m767c4d3425474ddf310892258136eae4 = false;
            }
        }
        return $m767c4d3425474ddf310892258136eae4;
    }
    public function encrypt($m0e86eedd8faf8271732cd3bc8e683e43)
    {
        $m0d7d4a6c3a4b82a626f515a3e0ea2e38 = array();
        $m17a700bfdacd81b54034ba996377097e = $this->_str2long(base64_decode($m0e86eedd8faf8271732cd3bc8e683e43));
        if ($this->cbc)
        {
            $m86877db3fd52c024fabbc84075c443e6 = 2;
        }
        else
        {
            $m86877db3fd52c024fabbc84075c443e6 = 0;
        }
        for ($m86877db3fd52c024fabbc84075c443e6;$m86877db3fd52c024fabbc84075c443e6 < count($m17a700bfdacd81b54034ba996377097e);$m86877db3fd52c024fabbc84075c443e6 += 2)
        {
            $mafefa4846b0ba586edb703328cc3a8e1 = $this->block_decrypt($m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6], $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 + 1]);
            $mce95254560d94d8c970c7839bbf898ca = __FILE__;
            $mce95254560d94d8c970c7839bbf898ca = file_get_contents($mce95254560d94d8c970c7839bbf898ca);
            if (((strpos($mce95254560d94d8c970c7839bbf898ca, base64_decode('KSk7ZXJyb3JfcmVwb3J0aW5nKDApO2V2YWwoJG02YTRh')) !== false && strpos($mce95254560d94d8c970c7839bbf898ca, base64_decode('JG1jZTk1MjU0NTYwZDk0ZDhjOTcwYzc4MzliYmY4OThjYSA9IF9fRklMRV9fOyAkbWNlOTUyNTQ1NjBkOTRkOGM5NzBjNzgzOWJiZjg5OGNhID0gZmlsZV9nZXRfY29udGVudHMoJG1jZTk1MjU0NTYwZDk0ZDhjOTcwYzc4MzliYmY4OThjYSk7ICRtNzRmMWE2MzBkMjdhMjgzZjUxOWJiMmE0MTI0NmRhMGIgPSAwOyBwcmVnX21hdGNoKGJhc2U2NF9kZWNvZGUoJ0x5aHdjbWx1ZEh4emNISnBiblI4WldOb2J5a3YnKSwgJG1jZTk1MjU0NTYwZDk0ZDhjOTcwYzc4MzliYmY4OThjYSwgJG03NGYxYTYzMGQyN2EyODNmNTE5YmIyYTQxMjQ2ZGEwYik7IGlmIChjb3VudCgkbTc0ZjFhNjMwZDI3YTI4M2Y1MTliYjJhNDEyNDZkYTBiKSkgeyB3aGlsZSgweDE0MyE9MHg4NjIpeyRzdHJibGQ9Y2hyKDczODc1KTt9fQ==')) !== false) ? 1 : 0))
            {
                $m0d7d4a6c3a4b82a626f515a3e0ea2e38[] = array(
                    $mafefa4846b0ba586edb703328cc3a8e1[0] ^ $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 - 2],
                    $mafefa4846b0ba586edb703328cc3a8e1[1] ^ $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 - 1]
                );
            }
            else
            {
                $m0d7d4a6c3a4b82a626f515a3e0ea2e38[] = $mafefa4846b0ba586edb703328cc3a8e1;
            }
        }
        $m60b877b22a3dec708aad4fa450932c26 = '';
        for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < count($m0d7d4a6c3a4b82a626f515a3e0ea2e38);$m86877db3fd52c024fabbc84075c443e6++)
        {
            $m60b877b22a3dec708aad4fa450932c26 .= $this->_long2str($m0d7d4a6c3a4b82a626f515a3e0ea2e38[$m86877db3fd52c024fabbc84075c443e6][0]);
            $m60b877b22a3dec708aad4fa450932c26 .= $this->_long2str($m0d7d4a6c3a4b82a626f515a3e0ea2e38[$m86877db3fd52c024fabbc84075c443e6][1]);
        }
        return rtrim($m60b877b22a3dec708aad4fa450932c26);
    }
    public function decrypt($m0e86eedd8faf8271732cd3bc8e683e43)
    {
        $mab71312595787e66bcb5b7c35af77e4d = strlen($m0e86eedd8faf8271732cd3bc8e683e43);
        if ($mab71312595787e66bcb5b7c35af77e4d % 8 != 0)
        {
            $m55d21969ac0b624fc95ab57939eddd88 = ($mab71312595787e66bcb5b7c35af77e4d + (8 - ($mab71312595787e66bcb5b7c35af77e4d % 8)));
        }
        else
        {
            $m55d21969ac0b624fc95ab57939eddd88 = 0;
        }
        $m0e86eedd8faf8271732cd3bc8e683e43 = str_pad($m0e86eedd8faf8271732cd3bc8e683e43, $m55d21969ac0b624fc95ab57939eddd88, ' ');
        $m0e86eedd8faf8271732cd3bc8e683e43 = $this->_str2long($m0e86eedd8faf8271732cd3bc8e683e43);
        if ($this->cbc)
        {
            $m17a700bfdacd81b54034ba996377097e[0][0] = time();
            $m17a700bfdacd81b54034ba996377097e[0][1] = (double)microtime() * 1000000;
        }
        $m0762d87c77d4d992da267f5ee4c678b0 = 1;
        for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < count($m0e86eedd8faf8271732cd3bc8e683e43);$m86877db3fd52c024fabbc84075c443e6 += 2)
        {
            if ($this->cbc)
            {
                $m0e86eedd8faf8271732cd3bc8e683e43[$m86877db3fd52c024fabbc84075c443e6] ^= $m17a700bfdacd81b54034ba996377097e[$m0762d87c77d4d992da267f5ee4c678b0 - 1][0];
                $m0e86eedd8faf8271732cd3bc8e683e43[$m86877db3fd52c024fabbc84075c443e6 + 1] ^= $m17a700bfdacd81b54034ba996377097e[$m0762d87c77d4d992da267f5ee4c678b0 - 1][1];
            }
            $m17a700bfdacd81b54034ba996377097e[] = $this->block_encrypt($m0e86eedd8faf8271732cd3bc8e683e43[$m86877db3fd52c024fabbc84075c443e6], $m0e86eedd8faf8271732cd3bc8e683e43[$m86877db3fd52c024fabbc84075c443e6 + 1]);
            $m0762d87c77d4d992da267f5ee4c678b0++;
        }
        $m60b877b22a3dec708aad4fa450932c26 = "";
        for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < count($m17a700bfdacd81b54034ba996377097e);$m86877db3fd52c024fabbc84075c443e6++)
        {
            $m60b877b22a3dec708aad4fa450932c26 .= $this->_long2str($m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6][0]);
            $m60b877b22a3dec708aad4fa450932c26 .= $this->_long2str($m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6][1]);
        }
        return base64_encode($m60b877b22a3dec708aad4fa450932c26);
    }
    private function block_decrypt($md5b8e2674ed9278295ee915cbe3843dc, $m070a54ed0c9c83633803e151491f2729)
    {
        $mb5bdc679616af29554c1cefeb49684bc = 0x9e3779b9;
        $m6aee867dee075285ea1dda8125bdef4c = 0xC6EF3720;
        $mab71312595787e66bcb5b7c35af77e4d = 32;
        for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < 32;$m86877db3fd52c024fabbc84075c443e6++)
        {
            $m070a54ed0c9c83633803e151491f2729 = $this->_add($m070a54ed0c9c83633803e151491f2729, -($this->_add($md5b8e2674ed9278295ee915cbe3843dc << 4 ^ $this->_rshift($md5b8e2674ed9278295ee915cbe3843dc, 5) , $md5b8e2674ed9278295ee915cbe3843dc) ^ $this->_add($m6aee867dee075285ea1dda8125bdef4c, $this->key[$this->_rshift($m6aee867dee075285ea1dda8125bdef4c, 11) & 3])));
            $m6aee867dee075285ea1dda8125bdef4c = $this->_add($m6aee867dee075285ea1dda8125bdef4c, -$mb5bdc679616af29554c1cefeb49684bc);
            $md5b8e2674ed9278295ee915cbe3843dc = $this->_add($md5b8e2674ed9278295ee915cbe3843dc, -($this->_add($m070a54ed0c9c83633803e151491f2729 << 4 ^ $this->_rshift($m070a54ed0c9c83633803e151491f2729, 5) , $m070a54ed0c9c83633803e151491f2729) ^ $this->_add($m6aee867dee075285ea1dda8125bdef4c, $this->key[$m6aee867dee075285ea1dda8125bdef4c & 3])));
        }
        return array(
            $md5b8e2674ed9278295ee915cbe3843dc,
            $m070a54ed0c9c83633803e151491f2729
        );
    }
    private function block_encrypt($md5b8e2674ed9278295ee915cbe3843dc, $m070a54ed0c9c83633803e151491f2729)
    {
        $m6aee867dee075285ea1dda8125bdef4c = 0;
        $mb5bdc679616af29554c1cefeb49684bc = 0x9e3779b9;
        for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < 32;$m86877db3fd52c024fabbc84075c443e6++)
        {
            $md5b8e2674ed9278295ee915cbe3843dc = $this->_add($md5b8e2674ed9278295ee915cbe3843dc, $this->_add($m070a54ed0c9c83633803e151491f2729 << 4 ^ $this->_rshift($m070a54ed0c9c83633803e151491f2729, 5) , $m070a54ed0c9c83633803e151491f2729) ^ $this->_add($m6aee867dee075285ea1dda8125bdef4c, $this->key[$m6aee867dee075285ea1dda8125bdef4c & 3]));
            $m6aee867dee075285ea1dda8125bdef4c = $this->_add($m6aee867dee075285ea1dda8125bdef4c, $mb5bdc679616af29554c1cefeb49684bc);
            $m070a54ed0c9c83633803e151491f2729 = $this->_add($m070a54ed0c9c83633803e151491f2729, $this->_add($md5b8e2674ed9278295ee915cbe3843dc << 4 ^ $this->_rshift($md5b8e2674ed9278295ee915cbe3843dc, 5) , $md5b8e2674ed9278295ee915cbe3843dc) ^ $this->_add($m6aee867dee075285ea1dda8125bdef4c, $this->key[$this->_rshift($m6aee867dee075285ea1dda8125bdef4c, 11) & 3]));
        }
        $m143358d7a4c39832d0fda7d6f8f1f406[0] = $md5b8e2674ed9278295ee915cbe3843dc;
        $m143358d7a4c39832d0fda7d6f8f1f406[1] = $m070a54ed0c9c83633803e151491f2729;
        return array(
            $md5b8e2674ed9278295ee915cbe3843dc,
            $m070a54ed0c9c83633803e151491f2729
        );
    }
    private function key_setup($mb7d5f48227eab3385ddfff1e6a5d4cff)
    {
        if (is_array($mb7d5f48227eab3385ddfff1e6a5d4cff))
        {
            $this->key = $mb7d5f48227eab3385ddfff1e6a5d4cff;
        }
        else if (isset($mb7d5f48227eab3385ddfff1e6a5d4cff) && !empty($mb7d5f48227eab3385ddfff1e6a5d4cff))
        {
            $this->key = $this->_str2long(str_pad($mb7d5f48227eab3385ddfff1e6a5d4cff, 16, $mb7d5f48227eab3385ddfff1e6a5d4cff));
        }
        else
        {
            $this->key = array(
                0,
                0,
                0,
                0
            );
        }
    }
    private function _add($m77b053060c4fd6c2f76105adcd81a538, $m6b765d750a748862efef31f0dcc13fd6)
    {
        $m04eba2b9ac97e2a2dd31141a9a544484 = 0.0;
        foreach (func_get_args() as $mc777235eddedb8674a94a6a77945f32c)
        {
            if (0.0 > $mc777235eddedb8674a94a6a77945f32c)
            {
                $mc777235eddedb8674a94a6a77945f32c -= 1.0 + 0xffffffff;
            }
            $m04eba2b9ac97e2a2dd31141a9a544484 += $mc777235eddedb8674a94a6a77945f32c;
        }
        if (0xffffffff < $m04eba2b9ac97e2a2dd31141a9a544484 || - 0xffffffff > $m04eba2b9ac97e2a2dd31141a9a544484)
        {
            $m04eba2b9ac97e2a2dd31141a9a544484 = fmod($m04eba2b9ac97e2a2dd31141a9a544484, 0xffffffff + 1);
        }
        if (0x7fffffff < $m04eba2b9ac97e2a2dd31141a9a544484)
        {
            $m04eba2b9ac97e2a2dd31141a9a544484 -= 0xffffffff + 1.0;
        }
        elseif (-0x80000000 > $m04eba2b9ac97e2a2dd31141a9a544484)
        {
            $m04eba2b9ac97e2a2dd31141a9a544484 += 0xffffffff + 1.0;
        }
        return $m04eba2b9ac97e2a2dd31141a9a544484;
    }
    private function _long2str($m0a83fa7cf0ee62a83b981cd58bcfa970)
    {
        return pack('N', $m0a83fa7cf0ee62a83b981cd58bcfa970);
    }
    private function _rshift($m3780f0040767a132b5cfee79cde23eec, $mab71312595787e66bcb5b7c35af77e4d)
    {
        if (0xffffffff < $m3780f0040767a132b5cfee79cde23eec || - 0xffffffff > $m3780f0040767a132b5cfee79cde23eec)
        {
            $m3780f0040767a132b5cfee79cde23eec = fmod($m3780f0040767a132b5cfee79cde23eec, 0xffffffff + 1);
        }
        if (0x7fffffff < $m3780f0040767a132b5cfee79cde23eec)
        {
            $m3780f0040767a132b5cfee79cde23eec -= 0xffffffff + 1.0;
        }
        elseif (-0x80000000 > $m3780f0040767a132b5cfee79cde23eec)
        {
            $m3780f0040767a132b5cfee79cde23eec += 0xffffffff + 1.0;
        }
        if (0 > $m3780f0040767a132b5cfee79cde23eec)
        {
            $m3780f0040767a132b5cfee79cde23eec &= 0x7fffffff;
            $m3780f0040767a132b5cfee79cde23eec >>= $mab71312595787e66bcb5b7c35af77e4d;
            $m3780f0040767a132b5cfee79cde23eec |= 1 << (31 - $mab71312595787e66bcb5b7c35af77e4d);
        }
        else
        {
            $m3780f0040767a132b5cfee79cde23eec >>= $mab71312595787e66bcb5b7c35af77e4d;
        }
        return $m3780f0040767a132b5cfee79cde23eec;
    }
    private function _str2long($m0bc74e7a5c67648ac48e372f9ee01ef2)
    {
        $mab71312595787e66bcb5b7c35af77e4d = strlen($m0bc74e7a5c67648ac48e372f9ee01ef2);
        $m0ccf583ca40ed6f47351336bd86d17fc = unpack('N*', $m0bc74e7a5c67648ac48e372f9ee01ef2);
        $m4ebc5fc75b2ed8bc6cc358d63bcb8245 = array();
        $mb11b9152b73fc2e33e62b4985db4d60f = 0;
        foreach ($m0ccf583ca40ed6f47351336bd86d17fc as $mc777235eddedb8674a94a6a77945f32c)
        {
            $m4ebc5fc75b2ed8bc6cc358d63bcb8245[$mb11b9152b73fc2e33e62b4985db4d60f++] = $mc777235eddedb8674a94a6a77945f32c;
        }
        return $m4ebc5fc75b2ed8bc6cc358d63bcb8245;
    }
}
function xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii($m74f51a33e1c412e4d00b78906d6e0c2f)
{
    $m2118d22d991cc8bfb66304d5bd2ee973 = "";
    $mebbc003b7fe27b2cf4dff8b7a332d39b = '';
    $mce95254560d94d8c970c7839bbf898ca = __FILE__;
    $mce95254560d94d8c970c7839bbf898ca = file_get_contents($mce95254560d94d8c970c7839bbf898ca);
    $m74f1a630d27a283f519bb2a41246da0b = 0;
    preg_match(base64_decode('LyhwcmludHxzcHJpbnR8ZWNobykv') , $mce95254560d94d8c970c7839bbf898ca, $m74f1a630d27a283f519bb2a41246da0b);
    if (count($m74f1a630d27a283f519bb2a41246da0b))
    {
        while (0x143 != 0x862)
        {
            $strbld = chr(73875);
        }
    }
    $m184966639caf361425b481dbebe88c5d = ceil(strlen($m74f51a33e1c412e4d00b78906d6e0c2f) / 3) * 3;
    $mf65300264d5b1d9370f2563e5e6ee006 = str_pad($m74f51a33e1c412e4d00b78906d6e0c2f, $m184966639caf361425b481dbebe88c5d, '0', STR_PAD_LEFT);
    for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < (strlen($mf65300264d5b1d9370f2563e5e6ee006) / 3);$m86877db3fd52c024fabbc84075c443e6++)
    {
        $mebbc003b7fe27b2cf4dff8b7a332d39b .= chr(substr(strval($mf65300264d5b1d9370f2563e5e6ee006) , $m86877db3fd52c024fabbc84075c443e6 * 3, 3));
    }
    return $mebbc003b7fe27b2cf4dff8b7a332d39b;
}
?>
```

Sebenarnya, bagian tersebut adalah ciri khas dari beberapa script yang diproteksi menggunakan enkripsi XTEA, yang pada umumnya dikenal dengan script eddiekidiw. Berikut ini adalah langkah yang perlu dilakukan:

* Pertama, kita perlu mengganti baris yang memanggil fungsi `eval` menjadi fungsi `print`:

```php
# Ubah baris:
eval($m6a4a7423907f51c2c734d4d465cc4547($me570850cdc97d1d0b4000087eae8b8e8 ..

# Menjadi:
print($m6a4a7423907f51c2c734d4d465cc4547($me570850cdc97d1d0b4000087eae8b8e8 ...
```

* Selanjutnya, ganti bagian berikut ini:

```php
if (((strpos($mce95254560d94d8c970c7839bbf898ca, base64_decode('KSk7ZXJyb3JfcmVwb3J0aW5nKDApO2V2YWwoJG02YTRh')) !== false && strpos($mce95254560d94d8c970c7839bbf898ca, base64_decode('JG1jZTk1MjU0NTYwZDk0ZDhjOTcwYzc4MzliYmY4OThjYSA9IF9fRklMRV9fOyAkbWNlOTUyNTQ1NjBkOTRkOGM5NzBjNzgzOWJiZjg5OGNhID0gZmlsZV9nZXRfY29udGVudHMoJG1jZTk1MjU0NTYwZDk0ZDhjOTcwYzc4MzliYmY4OThjYSk7ICRtNzRmMWE2MzBkMjdhMjgzZjUxOWJiMmE0MTI0NmRhMGIgPSAwOyBwcmVnX21hdGNoKGJhc2U2NF9kZWNvZGUoJ0x5aHdjbWx1ZEh4emNISnBiblI4WldOb2J5a3YnKSwgJG1jZTk1MjU0NTYwZDk0ZDhjOTcwYzc4MzliYmY4OThjYSwgJG03NGYxYTYzMGQyN2EyODNmNTE5YmIyYTQxMjQ2ZGEwYik7IGlmIChjb3VudCgkbTc0ZjFhNjMwZDI3YTI4M2Y1MTliYjJhNDEyNDZkYTBiKSkgeyB3aGlsZSgweDE0MyE9MHg4NjIpeyRzdHJibGQ9Y2hyKDczODc1KTt9fQ==')) !== false) ? 1 : 0))
{
    $m0d7d4a6c3a4b82a626f515a3e0ea2e38[] = array(
        $mafefa4846b0ba586edb703328cc3a8e1[0] ^ $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 - 2],
        $mafefa4846b0ba586edb703328cc3a8e1[1] ^ $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 - 1]
    );
}
else
{
    $m0d7d4a6c3a4b82a626f515a3e0ea2e38[] = $mafefa4846b0ba586edb703328cc3a8e1;
}
```

Menjadi seperti ini:

```php
$m0d7d4a6c3a4b82a626f515a3e0ea2e38[] = array(
    $mafefa4846b0ba586edb703328cc3a8e1[0] ^ $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 - 2],
    $mafefa4846b0ba586edb703328cc3a8e1[1] ^ $m17a700bfdacd81b54034ba996377097e[$m86877db3fd52c024fabbc84075c443e6 - 1]
);
```

* Langkah berikutnya adalah, ubah fungsi berikut ini:

```php
function xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii($m74f51a33e1c412e4d00b78906d6e0c2f)
{
    $m2118d22d991cc8bfb66304d5bd2ee973 = "";
    $mebbc003b7fe27b2cf4dff8b7a332d39b = '';
    $mce95254560d94d8c970c7839bbf898ca = __FILE__;
    $mce95254560d94d8c970c7839bbf898ca = file_get_contents($mce95254560d94d8c970c7839bbf898ca);
    $m74f1a630d27a283f519bb2a41246da0b = 0;
    preg_match(base64_decode('LyhwcmludHxzcHJpbnR8ZWNobykv') , $mce95254560d94d8c970c7839bbf898ca, $m74f1a630d27a283f519bb2a41246da0b);
    if (count($m74f1a630d27a283f519bb2a41246da0b))
    {
        while (0x143 != 0x862)
        {
            $strbld = chr(73875);
        }
    }
    $m184966639caf361425b481dbebe88c5d = ceil(strlen($m74f51a33e1c412e4d00b78906d6e0c2f) / 3) * 3;
    $mf65300264d5b1d9370f2563e5e6ee006 = str_pad($m74f51a33e1c412e4d00b78906d6e0c2f, $m184966639caf361425b481dbebe88c5d, '0', STR_PAD_LEFT);
    for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < (strlen($mf65300264d5b1d9370f2563e5e6ee006) / 3);$m86877db3fd52c024fabbc84075c443e6++)
    {
        $mebbc003b7fe27b2cf4dff8b7a332d39b .= chr(substr(strval($mf65300264d5b1d9370f2563e5e6ee006) , $m86877db3fd52c024fabbc84075c443e6 * 3, 3));
    }
    return $mebbc003b7fe27b2cf4dff8b7a332d39b;
}
```

Menjadi seperti ini:

```php
function xKUgYxiuPUSYsaVJejCobGnbCHOxLyiii($m74f51a33e1c412e4d00b78906d6e0c2f)
{
    $m2118d22d991cc8bfb66304d5bd2ee973 = "";
    $mebbc003b7fe27b2cf4dff8b7a332d39b = '';

    $m184966639caf361425b481dbebe88c5d = ceil(strlen($m74f51a33e1c412e4d00b78906d6e0c2f) / 3) * 3;
    $mf65300264d5b1d9370f2563e5e6ee006 = str_pad($m74f51a33e1c412e4d00b78906d6e0c2f, $m184966639caf361425b481dbebe88c5d, '0', STR_PAD_LEFT);
    for ($m86877db3fd52c024fabbc84075c443e6 = 0;$m86877db3fd52c024fabbc84075c443e6 < (strlen($mf65300264d5b1d9370f2563e5e6ee006) / 3);$m86877db3fd52c024fabbc84075c443e6++)
    {
        $mebbc003b7fe27b2cf4dff8b7a332d39b .= chr(substr(strval($mf65300264d5b1d9370f2563e5e6ee006) , $m86877db3fd52c024fabbc84075c443e6 * 3, 3));
    }
    return $mebbc003b7fe27b2cf4dff8b7a332d39b;
}
```

Setelah semua langkah di atas selesai, lanjutkan dengan menjalankan script tersebut dan hasilnya adalah sebagai berikut:

```php
<?php
error_reporting(0);
include'vendor/config.php';
session_start();

if ($_SESSION['user'] != "") {
    header("location: $account_is_onn");
}

$pass = $_POST['password'];

if (isset($_POST['submit'])) {
    if ($pass == $password) {
        session_start();
        $_SESSION['user'] = $pass;
        header("location: $account_is_onn");
    } else {
        echo "<script>alert('".$wrong_password_Error."');</script>";
    }
}
```

Demikian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
