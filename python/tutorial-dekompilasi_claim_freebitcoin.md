# Tutorial Dekompilasi Script Claim Freebitco.in


## Pendahuluan

Tutorial kali ini akan membahas cara melakukan dekompilasi script freebitco.in versi 1.2. Script tersebut merupakan script yang dibuat untuk python 3.


## Langkah-langkah

* Langkah pertama adalah mencari tahu tipe dari script tersebut:

```bash
% file bot.py
bot.pyc: data
```

* Tipe dari file tersebut tidak dikenali. Oleh karena itu, kita akan mencoba melihat isinya menggunakan _hexdump_ :

```bash
% hexdump -C bot.py | head
00000000  55 0d 0d 0a 00 00 00 00  aa f5 3e 5f ed 70 01 00  |U.........>_.p..|
00000010  e3 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000020  00 04 00 00 00 40 00 00  00 73 16 00 00 00 65 00  |.....@...s....e.|
00000030  65 01 64 00 83 01 a0 02  64 01 a1 01 83 01 01 00  |e.d.....d.......|
00000040  64 02 53 00 29 03 da 07  6d 61 72 73 68 61 6c 73  |d.S.)...marshals|
00000050  95 72 00 00 e3 00 00 00  00 00 00 00 00 00 00 00  |.r..............|
00000060  00 00 00 00 00 05 00 00  00 40 00 00 00 73 1c 00  |.........@...s..|
00000070  00 00 65 00 64 00 a0 01  64 01 64 02 84 00 64 03  |..e.d...d.d...d.|
00000080  44 00 83 01 a1 01 83 01  01 00 64 04 53 00 29 05  |D.........d.S.).|
00000090  da 00 63 01 00 00 00 00  00 00 00 00 00 00 00 02  |..c.............|
```

* Dari hasil _hexdump_ di atas, bisa terlihat bahwa script tersebut merupakan python bytecode. Oleh karena itu, kita akan mengubah ekstensi dari file tersebut dari `py` menjadi `pyc`:

```bash
% mv bot.py bot.pyc
```

* Selanjutnya, kita akan menggunakan [uncompyle6](https://pypi.org/project/uncompyle6/) untuk melakukan dekompilasi, dan disimpan dengan nama **bot.py** :

```bash
% uncompyle6 bot.pyc > bot.py
```

* Setelah itu, buka file **bot.py** hasil dari perintah di atas menggunakan teks editor. Isinya kurang lebih seperti ini:

```python
exec(__import__('marshal').loads(b'\xe3\x00\x00 ... \x00\x00\x00'))
```

* Edit file tersebut hingga menjadi seperti ini:

```python
import sys
import uncompyle6
uncompyle6.main.decompile(3.8, __import__('marshal').loads(b'\xe3\x00\x00 ... \x00\x00\x00'),sys.stdout)
```

* Selanjutnya, jalankan script tersebut dan simpan hasilnya pada file sementara, lalu ubah nama file sementara tersebut dengan **bot.py** dan menimpa file aslinya menggunakan perintah berikut ini:

```python
% python3 bot.py > tmp && mv -f tmp bot.py
```

* Hasilnya akan tersimpan pada file **bot.py** dan isinya kurang lebih seperti ini:

```python
exec(''.join((chr(_) for _ in (105, 109, 112, 111, 114, 116, 32, 106, 115, 111, 110,
                               44, 32, 111, 115, 44, 32, 115, 121, 115, 10, 105,

                               # ... snip ...

                               100, 40, 41, 10, 32, 32, 117, 112, 100, 97, 116, 101,
                               40, 41, 10, 32, 32, 114, 117, 110, 40, 41, 10))))
```

* Edit file tersebut dan ganti pemanggilan fungsi `exec` menjadi `print` seperti ini:

```python
print(''.join((chr(_) for _ in (105, 109, 112, 111, 114, 116, 32, 106, 115, 111, 110,
                               44, 32, 111, 115, 44, 32, 115, 121, 115, 10, 105,

                               # ... snip ...

                               100, 40, 41, 10, 32, 32, 117, 112, 100, 97, 116, 101,
                               40, 41, 10, 32, 32, 114, 117, 110, 40, 41, 10))))
```

* Jalankan script tersebut, simpan hasilnya pada file sementara, lalu ubah nama file sementara tersebut menjadi nama script tersebut:

```bash
% python3 bot.py > tmp && mv -f tmp bot.py
```

* Terakhir, buka file **bot.py** menggunakan teks editor, maka hasilnya adalah kurang lebih seperti potongan kode ini:

```python
import json, os, sys
import pyfiglet
import random
import requests
import urllib3
from cowpy import cow
from lolpython import lol_py
from time import sleep, time
from random import randint
from freebitcoin.API import API
from freebitcoin.NonCaptcha import API as nc
from helpers.twocaptcha import TwocaptchaAPI

kuningT = '\33[93m'
hijauT = '\33[92m'
biruT = '\33[94m'
unguT = '\33[95m'
merahT = '\33[91m'
reset = '\33[0m'

with open('config.json', 'r') as myfile:
  data = myfile.read()

obj = json.loads(data)

key = obj["key"]
login = obj["akun"]["login"]
passwor = obj["akun"]["password"]


def countdownTimer(start_minute, start_second):
    total_second = start_minute * 60 + start_second
    while total_second:
        mins, secs = divmod(total_second, 60)
        print(kuningT + f'Menunggu Claim kembali ==> {mins:02d}:{secs:02d}' + reset, end='\r')
        sleep(1)
        total_second -= 1


def load_animation():
    print('\n' * 50)
    load_str = "Mohon sabar,, Sedang Proses..."
    ls_len = len(load_str)
    animation = "|/-\\"
    anicount = 0
    counttime = 0
    i = 0
    while (counttime != 100):
        sleep(0.03)
        load_str_list = list(load_str)
        x = ord(load_str_list[i])
        y = 0
        if x != 32 and x != 46:
            if x>90:
                y = x-32
            else:
                y = x + 32

            load_str_list[i]= chr(y)
        res =''
        for j in range(ls_len):
            res = res + load_str_list[j]
        sys.stdout.write("\r"+res + animation[anicount])
        sys.stdout.flush()
        load_str = res
        anicount = (anicount + 1)% 4
        i =(i + 1)% ls_len
        counttime = counttime + 1
    if os.name == "nt":
        os.system("cls")
    else:
        os.system("clear")

def text(s):
    for c in s +'\n':
        sys.stdout.write(c)
        sys.stdout.flush()
        sys.stdout.readable()
        sleep(random.random() *0.01)

def banner():
  os.system('clear')
  msg = cow.milk_random_cow("Wellcome")
  lol_py(msg)
  Label = pyfiglet.figlet_format("Airdrop Hunter")
  lol_py(Label)
  text(hijauT + '''# This is script for claim freebitco.in with buy API-key 2captcha
# Creator   : Mame299
# Suport By : @harry_rezpector @AkasakaID @xuzut And All Friends
# telegram  : @AirdropHunterrrr
# donate    : Doge: D7rzpq91xmUVkER6E1ndfinRjRS4jvBkgV
              LTC : M9nQQZXwHQaoNStJrBcr6UfdCqx2RJHz5e



Jangan Lupa Ngopi Hari ini...!!!                       Version 1.2
====================================================================\n'''+reset)

# ... snip ...

if __name__ == "__main__":
  os.system('clear')
  load_animation()
  banner()
  password()
  update()
  run()
```

* Bisa terlihat bahwa script tersebut berhasil didekompilasi dengan mudah.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
