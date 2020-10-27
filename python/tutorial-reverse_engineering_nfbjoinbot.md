# Reverse Engineering Script NFBJoinBot


## Pendahuluan

Tutorial singkat kali ini akan membahas cara melakukan reverse engineering terhadap script `nfbjoinbot.py`. Script tersebut konon kabarnya tidak dapat direverse-engineer.


## Langkah-Langkah

Pertama, cari tahu dulu tipe dari script tersebut:

```bash
% file nfbjoinbot.py
nfbjoinbot.py: data
```

File tersebut tipenya tidak dikenali. Coba kita ubah ekstensi file tersebut dan dekompilasi menggunakan [uncompyle6](https://pypi.org/project/uncompyle6/):

```bash
% mv nfbjoinbot.py nfbjoinbot.pyc
% uncompyle6 nfbjoinbot.pyc
Unknown type 64 @
Unknown type 0
Unknown type 0
Unknown type 0
...
ImportError: Ill-formed bytecode file nfbjoinbot.pyc
<class 'AssertionError'>; co_code should be one of the types (<class 'str'>, <class 'bytes'>, <class 'list'>, <class 'tuple'>); is type <class 'NoneType'>
```

Ternyata uncompyle6 tidak mampu untuk melakukan dekompilasi. Selanjutnya kita periksa _string_ yang ada pada file tersebut:

```bash
% strings nfbjoinbot.pyc
b64decodes`6
...
marshal
zlib
base64r
exec
loads
decompress
nfb.py
<module>
```

Bisa terlihat bahwa bagian dari file tersebut dikompres menggunakan **zlib**. Selanjutnya, kita akan menggunakan [binwalk](https://github.com/ReFirmLabs/binwalk) untuk mengekstrak bagian tersebut:

```bash
% binwalk -re nfbjoinbot.pyc

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
327813        0x50085         Zlib compressed data, default compression
```

Lalu kita periksa file yang berhasil diekstrak:

```bash
% file _nfbjoinbot.pyc.extracted/50085
_nfbjoinbot.pyc.extracted/50085: ASCII text, with very long lines, with no line terminators
```

Berikut ini adalah potongan dari file tersebut:

```
4wAAAAAAAAAAAAAAAAAAAAAGA ... PG1vZHVsZT4BAAAA8wAAAAA=
```

Dari informasi di atas, bisa disimpulkan bahwa _string_ pada file tersebut diencode menggunakan base64. Selanjutnya kita akan membuat script python sederhana untuk membaca dan melakukan decode terhadap file tersebut:

```python
#!/usr/bin/env python3
import sys
import base64
import marshal
import uncompyle6

with open('_nfbjoinbot.pyc.extracted/50085', 'r') as f:
    e = f.read()

uncompyle6.main.decompile(3.8, marshal.loads(base64.b64decode(e)), sys.stdout)
```

Simpan script tersebut, misalnya dengan nama **oke.py**, lalu jalankan:

```bash
% ./oke.py
# uncompyle6 version 3.7.4
# Python bytecode 3.8
# Decompiled from: Python 3.8.6 (default, Sep 25 2020, 00:00:00)
# [GCC 10.2.1 20200723 (Red Hat 10.2.1-1)]
# Embedded file name: <nfb>saeful<nfb>
import marshal, zlib
from base64 import b64decode as NFB
exec(marshal.loads(zlib.decompress(b'x\x9cl\x9aG\xb2 ... \x89\xf2\x1f\xae{\xf9\xcb')))
```

Karena kita berhasil melakukan dekompilasi pada langkah di atas, maka kita jalankan sekali lagi script tersebut dan menyimpan hasilnya ke file **0.py** dengan perintah seperti ini:

```bash
% ./oke.py > 0.py
```

Selanjutnya kita akan membuat shell script untuk melakukan dekompilasi secara otomatis. Berikut ini adalah scriptnya:

```bash
#!/usr/bin/bash

trap abrt INT

abrt() {
    echo "aborted!"
    if [[ -f "tmp" ]]; then rm -f "tmp"; fi
    exit 1
}

FILE="0.py"

while [[ true ]]; do
    rg -q 'exec' $FILE
    if [[ $? -ne 0 ]]; then break; fi
    echo -n "#"
    sed -i 's/exec(/import sys,uncompyle6;uncompyle6.main.decompile(3.8,/' $FILE
    sed -i 's/)))$/)),sys.stdout)/' $FILE
    python3 $FILE > tmp && mv -f tmp $FILE
done

rm -f tmp
echo "all done..."
```

Simpan kode di atas dengan nama **hu.sh**, lalu jalankan:

```bash
% ./hu.sh
###################################
Traceback (most recent call last):

...

Parse error at or near `<48>' instruction at offset 112

all done...
```

Ternyata pada bagian akhir tersebut, uncompyle6 gagal melakukan dekompilasi. Jadi kita harus men-disassemble file **0.py** tersebut menggunakan modul `dis` bawaan python. Buka file **0.py** tersebut, dan edit sehingga menjadi seperti ini:

```python
import marshal, zlib
from base64 import b64decode as NFB
from dis import dis; dis(marshal.loads(NFB(zlib.decompress(b'x\x9c ... \x1fn#\x8e<'))))
```

Jalankan file tersebut menggunakan python lalu simpan ke file **0.dis** dengan perintah berikut ini:

```bash
% python3 0.py > 0.dis
```

Dari file **0.dis** tersebut, kita dapat melakukan dekompilasi secara manual. Berikut ini adalah hasil dekompilasi secara manual dari beberapa baris awal pada disassembly tersebut:

```python
import os
if not os.path.exists('session'):
    os.makedirs('session')
from random import *
from urllib.request import *
try:
    from telethon import TelegramClient,events,sync
except:
    os.system('pip install telethon')
    from telethon import TelegramClient,events,sync

import telethon
from telethon.tl.functions.messages import *
from telethon.tl.functions.channels import *
from telethon.errors import *
from telethon.errors.rpcerrorlist import *
try:
    from bs4 import *
except:
    os.system('pip install bs4')
    from bs4 import *
from time import *

# ...
```

Begitu seterusnya, Anda dapat melakukan dekompilasi dari hasil diassembly tersebut.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
