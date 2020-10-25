# Reversing Registrasi Script 999 Dicebot


## Pendahuluan

Tutorial singkat kali ini akan membahas mengenai reverse engineering script 999 Dicebot. Namun, tidak semua isi script akan dibahas, melainkan hanya skema registrasinya. Script tersebut cukup menarik, karena ada embel-embel **premium**.


## Langkah-langkah

Untuk men-disassemble script tersebut dapat dilakukan cukup dengan menggunakan modul `dis` bawaan python. Karena script tersebut di-obfuscate berulang kali, maka kita juga harus melakukan proses deobfuscate berulang kali. Pada bagian akhir proses tersebut akan ditemukan python bytecode yang merupakan disassembly dari kode sumber aslinya. Ada beberapa hal penting pada kode tersebut, berikut ini adalah beberapa bagian tersebut:

```python
 42         366 SETUP_FINALLY           62 (to 430)

 43         368 LOAD_NAME               48 (c)
            370 LOAD_ATTR               53 (get)
            372 LOAD_CONST              29 ('https://pastebin.com/raw/eRSaBGiN')
            374 LOAD_CONST              30 ('user-agent')
            376 LOAD_NAME               46 (obj)
            378 LOAD_CONST              20 ('User-Agent')
            380 BINARY_SUBSCR
            382 BUILD_MAP                1
            384 LOAD_CONST              31 (('headers',))
            386 CALL_FUNCTION_KW         2
            388 STORE_NAME              54 (r)

 44         390 LOAD_NAME                1 (json)
            392 LOAD_METHOD             45 (loads)
            394 LOAD_NAME               54 (r)
            396 LOAD_ATTR               55 (text)
            398 CALL_METHOD              1
            400 LOAD_CONST              32 ('database')
            402 BINARY_SUBSCR
            404 STORE_NAME              56 (db)

 45         406 LOAD_NAME               56 (db)
            408 LOAD_CONST              33 ('version')
            410 BINARY_SUBSCR
            412 STORE_NAME              57 (versi)

 46         414 LOAD_NAME               56 (db)
            416 LOAD_CONST              34 ('tipe script')
            418 BINARY_SUBSCR
            420 LOAD_METHOD             58 (upper)
            422 CALL_METHOD              0
            424 STORE_NAME              59 (tipe_script)
            426 POP_BLOCK
            428 JUMP_FORWARD            32 (to 462)

 47     >>  430 POP_TOP
            432 POP_TOP
            434 POP_TOP

 48         436 LOAD_NAME               60 (print)
            438 LOAD_NAME               31 (red)
            440 LOAD_CONST              35 ('Database error, Please try again later!')
            442 BINARY_ADD
            444 CALL_FUNCTION            1
            446 POP_TOP

 49         448 LOAD_NAME                3 (sys)
            450 LOAD_METHOD             61 (exit)
            452 CALL_METHOD              0
            454 POP_TOP
            456 POP_EXCEPT
            458 JUMP_FORWARD             2 (to 462)
            460 END_FINALLY
```

Potongan disassembly di atas jika didekompilasi secara manual, maka hasilnya kurang lebih seperti ini:

```python
try:
    r = c.get('https://pastebin.com/raw/eRSaBGiN',headers={'user-agent':obj['User-Agent']})
    db = json.loads(r.text)['database']
    versi = db['version']
    tipe_script = db['tipe script'].upper()
except:
    print(red + 'Database error, Please try again later!')
    sys.exit()
```

Bisa terlihat bahwa script tersebut mengirimkan permintaan ke sebuah tautan di [pastebin](https://pastebin.com/raw/eRSaBGiN), yang isinya adalah sebagai berikut:

```json
{
  "database": {
    "version": "1.5",
    "tipe script": "premium",
    "user premium": [
      "ranggawahyuf",
      "wandaaprilia",
      "hiruma.kun",
      "bogeg",
      "Zhaoulusi822",
      "sanaall",
      "ablenkcuy",
      "Putragaluh02",
      "Nevanevadaoy",
      "Frandesta Ginting",
      "modaltrx",
      "rolltonksz",
      "Kenzo_zyyn",
      "Virtex11",
      "hiruzen11",
      "termux999dice",
      "sapeiderman",
      "kkd20",
      "YOGANS",
      "FullGreen",
      "Emperiumjaya45",
      "M0rning",
      "kembreA4",
      "mfazima342",
      "brekele04",
      "bacot04",
      "Kon97",
      "Bayusr14",
      "Winko99",
      "EnrichoMR",
      "Zaydan0801",
      "soka11",
      "Mhdadityaprtama",
      "Dhietcorner22",
      "Eko kotek",
      "Hike@Me",
      "andizip",
      "Lefhynz",
      "buffalo bills",
      "Kaka200",
      "hikari11",
      "zagerd",
      "sijiikbal",
      "motumiband",
      "SuperHot2",
      "01yuni",
      "Avix999",
      "vivo999dice",
      "Aldyacong",
      "Ronisaragih20",
      "Thasan12",
      "motulucky",
      "motualiex",
      "haboek26",
      "Ratrio33",
      "nezuko145",
      "karanomori_shion",
      "malam881"
    ]
  }
}
```

Selanjutnya ada sebuah fungsi yang cukup menarik. Fungsi tersebut namanya `getdeviceid`. Berikut ini adalah potongan disassembly dari fungsi tersebut:

```python
Disassembly of <code object getdeviceid at 0x7f208e54c240, file "<EzzKun>", line 81>:
 83           0 LOAD_GLOBAL              0 (bytes)
              2 LOAD_GLOBAL              1 (os)
              4 LOAD_METHOD              2 (popen)
              6 LOAD_CONST               1 ('whoami')
              8 CALL_METHOD              1
             10 LOAD_METHOD              3 (read)
             12 CALL_METHOD              0
             14 LOAD_METHOD              4 (replace)
             16 LOAD_CONST               2 ('\n')
             18 LOAD_CONST               3 ('')
             20 CALL_METHOD              2
             22 LOAD_CONST               4 ('harry77')
             24 BINARY_ADD
             26 LOAD_CONST               5 ('utf-8')
             28 CALL_FUNCTION            2
             30 STORE_FAST               0 (getdev)

 84          32 LOAD_GLOBAL              5 (str)
             34 LOAD_GLOBAL              6 (binascii)
             36 LOAD_METHOD              7 (hexlify)
             38 LOAD_FAST                0 (getdev)
             40 CALL_METHOD              1
             42 CALL_FUNCTION            1
             44 LOAD_METHOD              4 (replace)
             46 LOAD_CONST               6 ('b')
             48 LOAD_CONST               3 ('')
             50 CALL_METHOD              2
             52 LOAD_METHOD              4 (replace)
             54 LOAD_CONST               7 ("'")
             56 LOAD_CONST               3 ('')
             58 CALL_METHOD              2
             60 RETURN_VALUE
```

Jika didekompilasi secara manual, maka fungsi tersebut bentuknya kurang lebih seperti ini:

```python
def getdeviceid():
    getdev = bytes(os.popen('whoami').read().replace('\n','') + 'harry77','utf-8')
    return str(binascii.hexlify(getdev)).replace('b','').replace("'",'')
```

Penjelasan fungsi di atas adalah sebagai berikut:

* Cari `username` menggunakan perintah `whoami`
* Tambahkan _string_ `harry77` ke bagian akhir `username`
* Ubah _string_ pada langkah di atas menjadi _string_ hexadecimal

Fungsi `getdeviceid` tersebut akan dipanggil di dalam fungsi `verif`. Disassembly fungsi tersebut cukup panjang, dan berikut ini hanya akan ditampilkan bagian yang penting saja:

```python
Disassembly of <code object verif at 0x7f208e54c030, file "<EzzKun>", line 86>:
 87           0 LOAD_FAST                0 (db)
              2 LOAD_CONST               1 ('tipe script')
              4 BINARY_SUBSCR
              6 LOAD_CONST               2 ('premium')
              8 COMPARE_OP               2 (==)
             10 EXTENDED_ARG             1
             12 POP_JUMP_IF_FALSE      338

 88          14 SETUP_FINALLY          144 (to 160)

 89          16 LOAD_GLOBAL              0 (str)
             18 LOAD_GLOBAL              1 (open)
             20 LOAD_CONST               3 ('lisensi.txt')
             22 LOAD_CONST               4 ('r')
             24 CALL_FUNCTION            2
             26 LOAD_METHOD              2 (read)
             28 CALL_METHOD              0
             30 CALL_FUNCTION            1
             32 STORE_FAST               1 (keylisensi)

 90          34 LOAD_FAST                1 (keylisensi)
             36 LOAD_GLOBAL              3 (getdeviceid)
             38 CALL_FUNCTION            0
             40 COMPARE_OP               2 (==)
             42 POP_JUMP_IF_FALSE       82

 91          44 LOAD_GLOBAL              4 (print)
             46 LOAD_GLOBAL              5 (hijau)
             48 LOAD_CONST               5 ('Welcome, ')
             50 BINARY_ADD
             52 LOAD_GLOBAL              6 (obj)
             54 LOAD_CONST               6 ('Account')
             56 BINARY_SUBSCR
             58 LOAD_CONST               7 ('Username')
             60 BINARY_SUBSCR
             62 BINARY_ADD
             64 LOAD_CONST               8 (' in Version Developer!')
             66 BINARY_ADD
             68 CALL_FUNCTION            1
             70 POP_TOP

 92          72 LOAD_GLOBAL              4 (print)
             74 LOAD_CONST               9 ('\x1b[1;34m≠==================================================≠')
             76 CALL_FUNCTION            1
             78 POP_TOP
             80 JUMP_FORWARD            74 (to 156)

 94     >>   82 LOAD_GLOBAL              7 (os)
             84 LOAD_ATTR                8 (path)
             86 LOAD_METHOD              9 (isfile)
             88 LOAD_CONST               3 ('lisensi.txt')
             90 CALL_METHOD              1
             92 POP_JUMP_IF_FALSE      104

 95          94 LOAD_GLOBAL              7 (os)
             96 LOAD_METHOD             10 (remove)
             98 LOAD_CONST               3 ('lisensi.txt')
            100 CALL_METHOD              1
            102 POP_TOP

 96     >>  104 LOAD_GLOBAL              4 (print)
            106 LOAD_GLOBAL             11 (red)
            108 LOAD_CONST              10 ('Key license invalid!')
            110 BINARY_ADD
            112 CALL_FUNCTION            1
            114 POP_TOP

 97         116 LOAD_GLOBAL              4 (print)
            118 LOAD_GLOBAL             12 (yellow)
            120 LOAD_CONST              11 ('Silahkan lakukan aktivasi multiakun!')
            122 BINARY_ADD
            124 CALL_FUNCTION            1
            126 POP_TOP

 98         128 LOAD_GLOBAL              4 (print)
            130 LOAD_GLOBAL             12 (yellow)
            132 LOAD_CONST              12 ('contact tele: ')
            134 BINARY_ADD
            136 LOAD_GLOBAL              5 (hijau)
            138 BINARY_ADD
            140 LOAD_CONST              13 ('@harry_rezpector')
            142 BINARY_ADD
            144 CALL_FUNCTION            1
            146 POP_TOP

 99         148 LOAD_GLOBAL             13 (sys)
            150 LOAD_METHOD             14 (exit)
            152 CALL_METHOD              0
            154 POP_TOP
        >>  156 POP_BLOCK
            158 JUMP_FORWARD           176 (to 336)
            ...
```

Jika diassembly tersebut didekompilasi secara manual, maka hasilnya kurang lebih seperti ini:

```python
def verif():
    if db['tipe script'] == 'premium':
        try:
            keylisensi = str(open('lisensi.txt','r').read())
            if keylisensi == getdeviceid():
                print(hijau + 'Welcome, ' + obj['Account']['Username'] + ' in Version Developer!')
                print('\x1b[1;34m≠==================================================≠')
            else:
                if os.path.isfile('lisensi.txt'):
                    os.remove('lisensi.txt')
                print(red + 'Key license invalid!')
                print(yellow + 'Silahkan lakukan aktivasi multiakun!')
                print(yellow + 'contact tele: ' + hijau + '@harry_rezpector')
                sys.exit()
        # ...
```

Pada potongan kode hasil dekompilasi manual di atas, bisa terlihat bahwa script tersebut akan membaca file `lisensi.txt` dan membandingkan isinya dengan _string_ hasil dari fungsi `getdeviceid`. Jika hasilnya sama, maka script akan menampilkan pesan **Welcome ... in Version Developer!**. Jadi secara sederhana, kita dapat membuat script yang akan membuat file `lisensi.txt` agar kita dapat menjalankan script tersebut. Berikut ini adalah scriptnya:

```python
import os
import binascii

with open('lisensi.txt','w') as f:
    f.write(str(binascii.hexlify(bytes(os.popen('whoami').read().replace('\n','') + 'harry77','utf-8'))).replace('b','').replace("'",''))
print("mission accomplished!")
```

Simpan dan jalankan script tersebut, maka file `lisensi.txt` akan dibuat dan diisi dengan informasi sesuai perangkat yang digunakan. Berikut ini adalah tampilan ketika script tersebut dijalankan tanpa adanya file `lisensi.txt`:

![Unregistered](https://raw.githubusercontent.com/drubicza/misc/master/others/ss-999_unregistered.jpg)

Dan berikut ini adalah tampilan ketika script tersebut dijalankan setelah ada file `lisensi.txt`:

![Registered](https://raw.githubusercontent.com/drubicza/misc/master/others/ss-999_registered.jpg)

Untuk daftar **user premium** yang terdapat pada tautan di [pastebin](https://pastebin.com/raw/eRSaBGiN), silakan Anda bereksperimen sendiri dan anggap saja itu sebagai pekerjaan rumah.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
