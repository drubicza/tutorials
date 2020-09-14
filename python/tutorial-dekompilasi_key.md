# Dekompilasi Script Key

## Pendahuluan

Pada tutorial kali ini, akan dibahas mengenai cara melakukan dekompilasi terhadap script [key](https://github.com/Mr-XsZ/Key).

## Langkah-langkah

* Pertama kloning terlebih dahulu script tersebut:

```bash
% git clone "https://github.com/Mr-XsZ/Key"
```

* Pindah ke sub direktori hasil kloning:

```bash
% cd Key
```

* Selanjutnya, kita periksa tipe file **key.py**:

```bash
% file key.py
key.py: data
```

* Tipenya tidak dikenali, jadi kita menggunakan **hexdump** untuk melihat bagian akhir dari file tersebut:

```bash
% hexdump -C key.py | tail
00001f90  77 02 90 ba a6 b2 90 ee  8b f0 1e ac 5a fe 3b ee  |w...........Z.;.|
00001fa0  65 ff 16 2c fe 36 3c c3  fc f3 2f 94 33 ab d6 29  |e..,.6<.../.3..)|
00001fb0  08 da 07 6d 61 72 73 68  61 6c da 04 7a 6c 69 62  |...marshal..zlib|
00001fc0  da 06 62 61 73 65 36 34  72 02 00 00 00 da 03 46  |..base64r......F|
00001fd0  6f 58 da 04 65 78 65 63  da 05 6c 6f 61 64 73 da  |oX..exec..loads.|
00001fe0  0a 64 65 63 6f 6d 70 72  65 73 73 a9 00 72 0a 00  |.decompress..r..|
00001ff0  00 00 72 0a 00 00 00 72  07 00 00 00 da 08 3c 6d  |..r....r......<m|
00002000  6f 64 75 6c 65 3e 02 00  00 00 73 04 00 00 00 10  |odule>....s.....|
00002010  00 0c 00                                          |...|
```

* Bisa terlihat bahwa file tersebut merupakan file _python bytecode_ dan kemungkinan besar menggunakan _bytecode_ python v3.8 karena belum dikenali. Selanjutnya kita ubah ekstensinya menjadi **.pyc** dan melakukan dekompilasi menggunakan [uncompyle6](https://github.com/rocky/python-uncompyle6) dan hasilnya disimpan pada file **out.py**:

```bash
% mv key.py key.pyc
% uncompyle6 key.pyc > out.py
```

* Hasilnya pada file **out.py** kurang lebih seperti ini:

```python
# uncompyle6 version 3.7.4
# Python bytecode 3.8 (3413)
# Decompiled from: Python 3.8.5 (default, Aug 12 2020, 00:00:00)
# [GCC 10.2.1 20200723 (Red Hat 10.2.1-1)]
# Embedded file name: exec
# Compiled at: 2020-06-30 11:41:50
# Size of source mod 2**32: 23224 bytes
import marshal, zlib
from base64 import b64decode as FoX
exec(marshal.loads(FoX(zlib.decompress(b'x\x9cm\x9a\xb5 ... \xf3/\x943\xab\xd6'))))
# okay decompiling key.pyc
```

* Jika kita menjalankan script tersebut, maka akan muncul tampilan seperti ini:

```bash
% python3 out.py

                 ╦╔═╔═╗╦ ╦
                 ╠╩╗║╣ ╚╦╝
                 ╩ ╩╚═╝ ╩
╔═════════════════════════════════════════╗
║*  Author :  Angga                       ║
║*  Wa     :  082211661007                ║
║*  Github :  Https://Github.com/Mr-XsZ   ║
╚═════════════════════════════════════════╝

1. Standart
2. My Version
99. Exit`

[+] Masukkan Pilihan:
```

* Pada tampilan di atas terdapat _string_ berupa nomor WA pembuat script tersebut, yaitu `082211661007`. Kita akan membuat sebuah shell script menggunakan bash scripting untuk melakukan proses dekompilasi secara otomatis dengan memanfaatkan _string_ tersebut. Jadi jika _string_ berupa nomor WA tersebut sudah ditemukan, maka proses dekompilasi akan berhenti secara otomatis. Berikut ini adalah script tersebut:

```bash
#!/usr/bin/bash

echo -n "[i] Decompiling "
while true; do
    rg -q "082211661007" "out.py"
    if [[ $? == 0 ]]; then
        echo -e "\n[+] All done!"
        break
    fi
    echo -n "."
    sed -i 's/exec(/import sys,uncompyle6\nuncompyle6.main.decompile(3.8,/' "out.py"
    sed -i 's/))))/))),sys.stdout)/' "out.py"
    python3 "out.py" > "temp" && mv -f "temp" "out.py"
done
```

* Simpan kode di atas dengan nama **dekrip.sh**, set _executable_, lalu jalankan seperti ini:

```bash
% chmod +x dekrip.sh
% ./dekrip.sh
[i] Decompiling ......................
[+] All done!
```

* Setelah proses dekompilasi selesai, maka file **out.py** akan berisi script **key.py** yang sudah didekompilasi. Berikut ini adalah isi dari script tersebut:

```python
import os, sys, time
w = '\x1b[97;4m'
m = '\x1b[91;1m'
h = '\x1b[92;1m'
k = '\x1b[93;1m'
u = '\x1b[94;1m'
p = '\x1b[95;1m'
a = '\x1b[96;1m'
s = '\x1b[97;1m'
b = '\x1b[30;2m'
n = '\x1b[30;0m'
zz = ['.   ', '..  ', '... ', '.... ', '.....\n']
logo = '\n\x1b[1;92m                 ╦╔═╔═╗╦ ╦\n\x1b[1;92m                 ╠╩╗║╣ ╚╦╝\n\x1b[1;92m                 ╩ ╩╚═╝ ╩ \n\x1b[1;97m╔═════════════════════════════════════════╗\n\x1b[1;97m║\x1b[1;93m* \x1b[1;97m Author \x1b[1;91m: \x1b[1;97m Angga                       ║\n\x1b[1;97m║\x1b[1;93m* \x1b[1;97m Wa \x1b[1;91m    : \x1b[1;97m 082211661007                ║\n\x1b[1;97m║\x1b[1;93m* \x1b[1;97m Github \x1b[1;91m: \x1b[1;97m Https://Github.com/Mr-XsZ   ║\n\x1b[1;97m╚═════════════════════════════════════════╝\n'

def menu():
    lagi = 'y'
    while lagi == 'y':
        print(h, logo)
        print('\n{}1. {}Standart\n{}2. {}My Version\n{}99. {}Exit`\n'.format(m, k, m, k, m, k, m, k))
        pil = int(input('\n{}[{}+{}] Masukkan Pilihan:{} '.format(h, m, h, a)))
        if pil == 1:
            stndrt()
        if pil == 2:
            myv()
        if pil == 99:
            thx()
            sys.exit(1)
        lagi = input('{}[{}*{}]Kembali ke Menu??{}[{}y/n{}]{}:{} '.format(h, m, h, u, m, u, h, a))


def stndrt():
    a = '\x1b[92m'
    b = '\x1b[91m'
    c = '\x1b[0m'
    os.system('clear')
    print(a + '\t  Key for help you')
    print(b + '\t    By Angga')
    print('\t Subcribe channel : https://www.youtube.com/channel/UCLU9H65QrIC6u2UetU6476w ')
    print(a + '++++++++++++++++++++++++++++++++++++++++')
    print('\nProses..')
    time.sleep(1)
    print(b + '\n[!] Making termux properties directory..')
    time.sleep(1)
    try:
        os.mkdir('/data/data/com.termux/files/home/.termux')
    except:
        pass
    else:
        print(a + '[!]Success !')
        time.sleep(1)
        print(b + '\n[!] Making setup file..')
        time.sleep(1)
        key = "extra-keys = [['ESC','/','-','HOME','UP','>'], ['TAB','CTRL','ALT','LEFT','DOWN','RIGHT']]"
        kontol = open('/data/data/com.termux/files/home/.termux/termux.properties', 'w')
        kontol.write(key)
        kontol.close()
        time.sleep(1)
        print(a + '[!] Success !')
        time.sleep(1)
        print(b + '\n[!] Setting up..')
        time.sleep(2)
        os.system('termux-reload-settings')
        print(a + '[!] Successfully !! ^^' + c + '\n\nSupport dgn cara subcribe channel saya  : https://www.youtube.com/channel/UCLU9H65QrIC6u2UetU6476w \n\n')


def myv():
    a = '\x1b[92m'
    b = '\x1b[91m'
    c = '\x1b[0m'
    os.system('clear')
    print(a + '\t  Shorcut for help you')
    print(b + '\t    By Angga')
    print('\t Subcribe channel : https://www.youtube.com/channel/UCLU9H65QrIC6u2UetU6476w ')
    print(a + '++++++++++++++++++++++++++++++++++++++++')
    print('\nProses..')
    time.sleep(1)
    print(b + '\n[!] Making termux properties directory..')
    time.sleep(1)
    try:
        os.mkdir('/data/data/com.termux/files/home/.termux')
    except:
        pass
    else:
        print(a + '[!]Success !')
        time.sleep(1)
        print(b + '\n[!] Making setup file..')
        time.sleep(1)
        key = "extra-keys = [['ESC','/','-','HOME','END','UP'],['TAB','CTRL','ALT','LEFT','RIGHT','DOWN','PGDN']]"
        kontol = open('/data/data/com.termux/files/home/.termux/termux.properties', 'w')
        kontol.write(key)
        kontol.close()
        time.sleep(1)
        print(a + '[!] Success !')
        time.sleep(1)
        print(b + '\n[!] Setting up..')
        time.sleep(2)
        os.system('termux-reload-settings')
        print(a + '[!] Successfully !! ^^' + c + '\n\nSupport dgn cara subcribe channel saya  : https://www.youtube.com/channel/UCLU9H65QrIC6u2UetU6476w \n\n')


def thx():
    print(w, 'Thanks for use!', n)
    print(u)
    os.system('figlet Thanks for use !')
    print(s)
    with open('README.md') as (md):
        print(md.read())
        md.close


if __name__ == '__main__':
    menu()
```

* Bisa terlihat bahwa script tersebut telah berhasil didekompilasi.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
