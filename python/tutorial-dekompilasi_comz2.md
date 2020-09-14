# Tutorial Dekompilasi Comz2


## Pendahuluan

Tutorial ini akan membahas cara untuk melakukan dekompilasi script [comz2](https://github.com/drubicza/comz-1) yang pernah [diposting di situs ini](https://e-sec2.org/j/c/453), setelah kurang lebih hampir 1 tahun lamanya. Caranya cukup mudah. Berikut ini adalah langkah-langkahnya:

## Langkah-langkah

* Pertama, unduh terlebih dahulu script comz2:

```bash
% wget -O comz2.pyc "https://github.com/drubicza/comz-1/blob/master/comz.pyc?raw=true"
```

* Setelah itu, kita mencari tahu tipe dari script tersebut:

```bash
% file comz.pyc
comz.pyc: python 2.7 byte-compiled
```

* Dekompilasi script tersebut menggunakan [uncompyle6](https://pypi.org/project/uncompyle6/) dan simpan hasilnya pada file **0.py**:

```bash
% uncompyle6 comz.pyc > 0.py
```

* Hasilnya adalah file **0.py** yang isinya kurang lebih seperti ini:

```python
# uncompyle6 version 3.7.3
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: da_enc.py
# Compiled at: 2019-09-21 16:47:17
import marshal
exec marshal.loads('c\x00\x00\x00\x00\x00 ... \xff\x00\xff\x00\xff\x00R\x01')
# okay decompiling comz.pyc
```

* Ubah script tersebut hingga menjadi seperti ini, lalu jalankan:

```python
import sys
import marshal
import uncompyle6
uncompyle6.main.decompile(2.7, marshal.loads('c\x00\x00\x00\x00\x00 ... \xff\x00\xff\x00\xff\x00R\x01'), sys.stdout)
```

* Ternyata proses dekompilasi menggunakan **uncompyle6** gagal dengan pesan error seperti ini:

```bash
% python2.7 0.py
# uncompyle6 version 3.7.3
# Python bytecode 2.7
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: <sazxt>
Traceback (most recent call last):
  File "0.py", line 10, in <module>
    ff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xd2\x01\x0c\x01\t\x01\x16\x01\x16\x01\x06\x01\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00\xff\x00R\x01'), sys.stdout)
  File "build/bdist.linux-x86_64/egg/uncompyle6/main.py", line 141, in decompile
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 2573, in code_deparse
  File "build/bdist.linux-x86_64/egg/uncompyle6/scanners/scanner2.py", line 407, in ingest
KeyError: 992776
```

* Karena langkah di atas gagal, maka kita akan mengubah file **0.py** menjadi **1.pyc** dengan cara mengedit file **0.py** menjadi seperti ini:

```python
e = 'c\x00\x00\x00\x00\x00 ... xff\x00\xff\x00R\x01'
with open("1.pyc","wb") as f:
    f.write('\x03\xf3\x0d\x0a\xeb\x56\x92\x5a' + e)
```

* Simpan file **0.py** yang telah diubah tersebut, kemudian jalankan untuk menghasilkan file **1.pyc**.

```bash
% python2.7 0.py
```

* Selanjutnya, kita akan gunakan [pycdc](https://github.com/zrax/pycdc) untuk melakukan proses dekompilasi terhadap file **1.pyc**:

```bash
% pycdc 1.pyc > 2.py
```

* Hasil dekompilasi menggunakan **pycdc** yang disimpan pada file **2.py** kurang lebih seperti ini:

```python
# Source Generated with Decompyle++
# File: 1.pyc (Python 2.7)

z = [
    98,
    104,
    67,
    125,
    203,
    #...snip...
    213,
    185,
    47,
    232,
    96]

_ = [
    66,
    22,
    77,
    65,
    7,
    #...snip...
    16,
    11,
    104,
    20,
    101]

__ = [
    594,
    594,
    594,
    594,
    594,
    #...snip...
    594,
    594,
    594,
    594,
    594]

OoO_ = [
    45,
    42,
    39,
    39,
    39,
    #...snip...
    39,
    39,
    90,
    39,
    40]

import marshal

OO = lambda _: marshal.loads(_)
u = ({ } < ()) - ({ } < ())
p = ({ } < ()) - ({ } < ())
v = []
exec (lambda : (() > ()) + (() < ())).func_code.co_lnotab.join(map(chr, [ #... snip ... ]))
exec OO(''.join([ chr(i) for i in lx ]).decode('hex'))
```

* Bisa terlihat bahwa script tersebut memiliki 4 buah _list_ yaitu `z`,`_`,`__` dan `OoO_`. Selain itu ada variabel `u` dan `p` yang jika di-deobfuscate maka hasilnya adalah:

```python
u = 0
p = 0
```

* Selanjutnya, pada bagian `exec` yang pertama, terdapat kode yang di-obfuscate. Cukup ganti `exec` menjadi `print` lalu jalankan, maka hasilnya seperti ini:

```python
lx E []
for _EqnaVarnames in range(len(__)):
        .append(__[y] / _]p])
        y,p=(0,0)
fqr _bli in range(ten(OoO_)):
        lx.append(OoOa[] 3 vcp])
        y -= 1
        p 3E 1
```

* Bisa terlihat bahwa hasil deobfuscate di atas kurang sempurna. Oleh karena itu, kita perlu mencari kode yang benar. Jalankan script **comz2** menggunakan python 2.7:

```bash
% python2.7 comz.pyc
         ╔════════════════════════════════╗
           ⏣ Author   : Sazxt
           ⏣ My Team  : Black Coder Crush
           ⏣ whatsapp : 083892081021
           ⏣ CodeName : ComPilez v1.5 update !
         ╚════════════════════════════════╝

[-] Compile Marshal
[+] Available Version Update [ V.1.5 ]

{ 01 } Compile Marshal (repeat : max 400)
{ 02 } Compile Marshal > base64 (repeat : max 400)
{ 03 } Compile Marshal > base64 > pycom
{ 04 } Compile By Sazxt 1 [Hard]
{ 05 } Compile By Sazxt 2 [easy]
{ 06 } Compile By Sazxt 3
{ 07 } Compile By Sazxt 4
{ 08 } Compile Zlib (repeat : max 200)
{ 09 } Compile Base64 (repeat : max 200)
{ 10 } Compile Base16 (repeat : max 200)
{ 11 } Compile Base32 (repeat : max 200)
{ 12 } Compile Base64&marshal (repeat : max 200)
{ 13 } Compile By Sazxt 5
{ 14 } Compile By Sazxt 6
{ 15 } Pyc inject note
{ EX } Exit (E Or X)

[!] Pilih >>
```

* Jalankan terminal baru, lalu cari proses **comz2** tersebut menggunakan `ps` dan `grep` atau `ripgrep`:

```bash
% ps aux | rg comz
thanos         8229  0.1  0.2 235496 19668 pts/2    S+   16:28   0:00 python2.7 comz.pyc
```

* Dari perintah di atas, bisa terlihat bahwa proses **comz2** memiliki **PID** `8229`. Selanjutnya jalankan `gdb` lalu _attach_ ke proses tersebut:

```bash
% gdb
Demangling of encoded C++/ObjC names when displaying symbols is on.
Demangling of C++/ObjC names in disassembly listings is on.
gdb> attach 8229
```

* Setelah proses tersebut selesai, kita akan melakukan _dump_ terhadap proses **comz2** tersebut menggunakan perintah `generate-core-file` pada `gdb`:

```bash
gdb> generate-core-file comz2.dmp
warning: target file /proc/8229/cmdline contained unexpected null characters
Saved corefile comz2.dmp
```

* Proses _dump_ selesai, kita dapat melakukan _detach_ dari proses **comz2** tersebut lalu keluar dari `gdb`:

```bash
gdb> detach
Detaching from program: /usr/bin/python2.7, process 8229
[Inferior 1 (process 8229) detached]
gdb> q
```

* Selanjutnya pada prompt **comz2** kita dapat memasukkan input `E` atau `X` untuk keluar dari aplikasi tersebut:

```bash
[!] Pilih >> E
```

* Kini kita akan menyaring _printable string_ yang terdapat pada _dump_ yang dibuat oleh `gdb` yaitu **comz2.dmp** dan disimpan ke file **comz2.str**:

```bash
% strings comz2.dmp > comz2.str
```

* Buka file **comz2.str** menggunakan teks editor, maka kita akan menemukan bagian kode yang diobfuscate berikut:

```python
exec((lambda:((()>())+(()<()))).func_code.co_lnotab).join(map(chr,[(((((((({}=={})+([]>={})+({}<[]))+(({}<[])+({} #...snip... +(((()<={})+(()==()))))]))
```

* Salin kode yang diobfuscate tersebut, lalu ganti `exec` menjadi `print` pada kode tersebut, dan jalankan. Hasilnya adalah:

```python
lx = []
for _Con_Varnames in range(len(__)):
        v.append(__[u] / _[p])
        u,p=(0,0)
for _bliz in range(len(OoO_)):
        lx.append(OoO_[u] + v[p])
        u += 1
        p += 1
```

* Sekarang bagian kode yang diobfuscate tersebut sudah dapat terbaca. Edit kembali script yang terakhir di-deobfuscate pada beberapa langkah di atas menjadi seperti ini:

```python
# Source Generated with Decompyle++
# File: 1.pyc (Python 2.7)

z = [
    98,
    104,
    67,
    125,
    203,
    #...snip...
    213,
    185,
    47,
    232,
    96]

_ = [
    66,
    22,
    77,
    65,
    7,
    #...snip...
    16,
    11,
    104,
    20,
    101]

__ = [
    594,
    594,
    594,
    594,
    594,
    #...snip...
    594,
    594,
    594,
    594,
    594]

OoO_ = [
    45,
    42,
    39,
    39,
    39,
    #...snip...
    39,
    39,
    90,
    39,
    40]

import sys
import marshal
import uncompyle6

OO = lambda _: marshal.loads(_)
u = 0
p = 0
v = []
lx = []
for _Con_Varnames in range(len(__)):
        v.append(__[u] / _[p])
        u,p=(0,0)
for _bliz in range(len(OoO_)):
        lx.append(OoO_[u] + v[p])
        u += 1
        p += 1
uncompyle6.main.decompile(2.7, OO(''.join([ chr(i) for i in lx ]).decode('hex')), sys.stdout)
```

* Jalankan script tersebut, dan simpan hasilnya pada file **3.py** yang isinya kurang lebih seperti ini:

```python
import marshal
exec marshal.loads('c\x00\x00\x00\x00 ... \t\xff\x00\xff\x00#')
```

* Edit file **3.py** tersebut menjadi seperti berikut, lalu jalankan:

```python
import sys
import marshal
import uncompyle6

uncompyle6.main.decompile(2.7, marshal.loads('c\x00\x00\x00\x00 ... \t\xff\x00\xff\x00#'),sys.stdout)
```

* Ternyata gagal dengan pesan error **Parse error at or near 'None' instruction at offset -1**

* Edit kembali file tersebut, agar kita dapat menyimpannya menjadi file _python bytecode_:

```python
e = 'c\x00\x00\x00\x00 ... \t\xff\x00\xff\x00#'
with open("4.pyc","wb") as f:
    f.write('\x03\xf3\x0d\x0a\xeb\x56\x92\x5a' + e)
```

* Jalankan script tersebut untuk menghasilkan file **4.pyc**.

```bash
% python2.7 3.py
% file 4.pyc
4.pyc: python 2.7 byte-compiled
```

* Selanjutnya, kita akan menggunakan **pycdc** untuk melakukan dekompilasi dan menyimpan hasilnya pada file `comz2-dec.py`:

```bash
% pycdc 4.pyc > comz2-dec.py
```

* Berikut ini adalah sebagian dari script **comz2** yang telah berhasil di-deobfuscate:

```python
# Source Generated with Decompyle++
# File: 4.pyc (Python 2.7)

import os
import sys
import zlib
import base64
import marshal
import binascii
import py_compile
from time import sleep as waktu
from random import randint
p = '\x1b[0m'
m = '\x1b[31m'
i = '\x1b[32m'
b = '\x1b[34m'
k = '\x1b[33;1m'
cg = '\x1b[36m'
ba = '\x1b[96;1m'
pu = '\x1b[35m'
gr = '\x1b[37m'
pb = '\x1b[47m'
cout = 0
logo = '         \x1b[34m\xe2\x95\x94\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x97\n           \x1b[31m\xe2\x8f\xa3 \x1b[32mAuthor   \x1b[37m: Sazxt\n           \x1b[31m\xe2\x8f\xa3 \x1b[32mMy Team  \x1b[37m: Black Coder Crush\n           \x1b[31m\xe2\x8f\xa3 \x1b[32mwhatsapp \x1b[37m: 083892081021\n           \x1b[31m\xe2\x8f\xa3 \x1b[32mCodeName \x1b[37m: \x1b[36mComPilez \x1b[0;1mv1.5 \x1b[33;1mupdate !\n         \x1b[34m\xe2\x95\x9a\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x90\xe2\x95\x9d\n'

def main():
    global cout, bin
    p = '\x1b[0m'
    m = '\x1b[31m'
    i = '\x1b[32m'
    b = '\x1b[34m'
    k = '\x1b[33;1m'
    cg = '\x1b[36m'
    ba = '\x1b[96;1m'
    pu = '\x1b[35m'
    gr = '\x1b[37m'
    pb = '\x1b[47m'
    os.system('clear')

#... snip...

    except KeyboardInterrupt:
        print '%s[%s!%s] %sCtrl+C not Working Pliss ctr+d !' % (b, m, b, gr)
        waktu(0.5)
        main()
    except EOFError:
        sys.exit()
    except IOError:
        print '%s[%s\xe2\x9d\x97%s] %sFile Tidak Di Temukan !' % (b, m, b, gr)
        waktu(0.5)
        main()
    except ValueError:
        print '%s[%s!%s] %sCout Berupa Angka ! ' % (b, m, b, gr)


main()
```

## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
