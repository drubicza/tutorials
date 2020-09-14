# Dekompilasi Script pytodz


## Pendahuluan

Jadi beberapa waktu lalu, sempat ada yang bertanya cara melakukan dekompilasi script [pytodz](https://github.com/xSODx11/pytodz). Berikut ini adalah caranya:

## Langkah-langkah

* Unduh dulu script pytodz tersebut:

```bash
% wget 'https://github.com/xSODx11/pytodz/raw/master/pytodz.py'
```

* Selanjutnya, kita periksa tipe filenya:

```bash
% file pytodz.py
pytodz.py: python 2.7 byte-compiled
```

* Ternyata file tersebut tipenya adalah python bytecode, jadi kita _rename_ saja ekstensinya:

```bash
% mv pytodz.py pytodz.pyc
```

* Lalu kita akan melakukan dekompilasi menggunakan [uncompyle6](https://pypi.org/project/uncompyle6/)

```bash
% uncompyle6 pytodz.pyc
# uncompyle6 version 3.7.3
# Python bytecode 2.7 (62211)
# Decompiled from: Python 3.8.5 (default, Jul 31 2020, 00:00:00)
# [GCC 10.2.1 20200723 (Red Hat 10.2.1-1)]
# Embedded file name: xSODx
# Compiled at: 2020-04-09 10:07:32
from marshal import loads as m_l
from zlib import decompress as z_d
f = file(__file__, 'rb')
f.seek(572)
exec (lambda _: m_l((lambda __: z_d(__))(m_l(_)[::-1])))(f.read()[::-1])
del f
del m_l
del z_d
# okay decompiling pytodz.pyc
```

* Dari informasi di atas, kita bisa melihat bahwa script tersebut akan membaca dirinya sendiri mulai dari _offset_ **572**, lalu membaca semua data mulai dari _offset_ tersebut. Setelah itu, datanya akan di-dekompresi menggunakan _zlib_. Dan hasil dekompresi tersebut akan digunakan sebagai input untuk modul _marshal_. Berikut ini adalah script sederhana yang dapat kita gunakan untuk melakukan dekompilasi terhadap script **pytodz** :

```python
#!/usr/bin/env python2
import sys
import uncompyle6
from marshal import loads as m_l
from zlib import decompress as z_d

f = file('pytodz.pyc', 'rb')
f.seek(572)
e = (lambda _: m_l((lambda __: z_d(__))(m_l(_)[::-1])))(f.read()[::-1])
uncompyle6.main.decompile(2.7, e, sys.stdout)
f.close()
```

* Simpan script di atas dengan nama `wow.py`, lalu jalankan seperti ini:

```bash
% python2.7 wow.py
# uncompyle6 version 3.7.3
# Python bytecode 2.7
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: xSODx
import os, sys, re, time, binascii, base64, marshal, zlib
from py_compile import compile as _compile
try:
    import uncompyle6
except ImportError:
    os.system('pip2 install uncompyle6')

# ...snip...

def decompile_menu():
    os.system('clear')
    print _0o0oO_.encode('cp500')
    print ('[1] {0} marshal\n[2] {0} pyc\n[3] {0} zlib\n[4] {0} base16\n[5] {0} base32\n[6] {0} base64\n[7] {0} hex\n[8] {0} bin\n[B] Back').format('Decompile')
    print '\xe2\x80\x94' * 48
    try:
        choose = raw_input('[\xe2\x80\xa2] Choose: ')
    except (EOFError, KeyboardInterrupt):
        pass

    if choose in ('1', '2', '3', '4', '5', '6', '7', '8'):
        try:
            file = raw_input('[\xe2\x80\xa2] Input File: ')
        except Exception as exception:
            main_menu(exception)

        choose = int(choose)
        if choose == 1:
            Decompile(file).marshal()
        elif choose == 2:
            Decompile(file).pyc()
        elif choose == 3:
            Decompile(file).zlib()
        elif choose == 4:
            Decompile(file).base16()
        elif choose == 5:
            Decompile(file).base32()
        elif choose == 6:
            Decompile(file).base64()
        elif choose == 7:
            Decompile(file).hex()
        elif choose == 8:
            Decompile(file).bin()
    elif choose in ('b', 'B'):
        main_menu()
    else:
        main_menu('[!] Wrong Input')


if __name__ == '__main__':
    main_menu()
```

* Bisa terlihat bahwa script tersebut berhasil didekompilasi.

## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
