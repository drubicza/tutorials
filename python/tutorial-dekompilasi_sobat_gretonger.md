# Tutorial Dekompilasi Script Sobat Gretonger


## Pendahuluan

Tutorial kali ini akan membahas cara melakukan _deofuscate_ dan dekompilasi terhadap script [sobat gretongan](https://www.sobatgretongan.online/). Adapun versi script yang menjadi target pada tutorial kali ini adalah versi 2.5.7 yang dapat [diunduh pada tautan ini](https://sfile.mobi/8ujxDFe4NOf). Berikut ini adalah langkah-langkahnya.


## Langkah-Langkah

* Pertama, unduh dulu script tersebut. Lalu kita periksa tipenya:

```bash
% file script-v2.5.7.py
script-v2.5.7.py: python 2.7 byte-compiled
```

* Ternyata file tersebut tipenya adalah _python bytecode_ . Selanjutnya kita perlu mengubah ekstensi file tersebut menjadi **pyc**:

```bash
% mv script-v2.5.7.py script-v2.5.7.pyc
```

* Lalu kita akan mencoba melakukan dekompilasi menggunakan [uncompyle6](https://github.com/rocky/python-uncompyle6/):

```bash
% uncompyle6 script-v2.5.7.pyc
# uncompyle6 version 3.7.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 3.8.5 (default, Aug 12 2020, 00:00:00)
# [GCC 10.2.1 20200723 (Red Hat 10.2.1-1)]
# Embedded file name: zasu
# Compiled at: 2020-04-09 10:07:32
Traceback (most recent call last):
  File "/home/thanos/.local/bin/uncompyle6", line 8, in <module>
    sys.exit(main_bin())
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/bin/uncompile.py", line 193, in main_bin
    result = main(src_base, out_base, pyc_paths, source_paths, outfile,
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/main.py", line 316, in main
    deparsed = decompile_file(
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/main.py", line 208, in decompile_file
    decompile(
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/main.py", line 140, in decompile
    deparsed = deparse_fn(
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/semantics/pysource.py", line 2572, in code_deparse
    tokens, customize = scanner.ingest(
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/scanners/scanner2.py", line 407, in ingest
    j = self.offset2inst_index[offset]
KeyError: 322444
```

* Ternyata proses dekompilasi menggunakan uncompyle6 gagal. Selanjutnya, kita akan menggunakan [pycdc](https://github.com/zrax/pycdc) dan menyimpan hasilnya pada file **0.py** :

```bash
% pycdc script-v2.5.7.pyc > 0.py
```

* Berikut ini adalah potongan hasil dekompilasi menggunakan pycdc:

```python
# Source Generated with Decompyle++
# File: script-v2.5.7.pyc (Python 2.7)

import base64
import marshal
DIFENT_STACK = []
(lambda ___: sot.sort())(globals().update({
    (lambda : uc[i][p] ^ c[0][o] ** 5 != '').__code__.co_consts[3].join([ chr(i) for i in (lambda _______, _, ___: [
((___ >= _______) + (___ >= ___) + (_ >= _) + (___ > _) + (_______ <= _) + (_ <= ___) + ((___ >= _) - (_______ > ___)) << (___ > _) + (_ <= ___) + (_ >= _______) + (___ >= ___)) + (___ >= _______) + (_______ != _) + (___ <= ___),
((___ >= ___) + (___ >= ___) + (_______ != _) + (_______ != ___) + (_______ == _______) + (___ != _______) + (_ >= _______) * (___ >= _______) << (_______ != _) + (___ != _) + (___ != _) + (_______ <= _______)) - (___ >= ___) * (_ != ___),
((___ > _______) + (_ >= _______) + (___ >= ___) + (_ < ___) + (_______ < _) + (___ != _) + (_______ >= _______) * (___ != _) << (_______ < ___) + (_______ < ___) + (_______ != ___) + (___ != _______)) + ((_ == _) - (_______ == _) << (___ != _) + (_ < ___))])((8595258 == 8595258) + (9 != 484955) + (8595258 != 9), ((9 >= 8595258) + (484955 < 8595258) << (8595258 <= 8595258) + (9 >= 9) + (484955 != 8595258)) + (484955 != 8595258) + (9 > 9) << (9 > 9) + (9 <= 9), ((8595258 == 8595258) + (9 == 9) + (9 <= 8595258) + (8595258 >= 8595258) + (9 < 8595258) + (484955 != 9) + ((9 <= 484955) - (8595258 < 9)) << (9 <= 484955) + (8595258 <= 8595258)) - (9 != 484955) * (484955 <= 8595258)) ]): (lambda : [ {
int(c.split('$')[0]) + int(c.split('$')[2]): c.split('$')[1] } for c in (lambda ___: ___.split('@'))('-2926$i$6532@-3122$S$7333@-

#... snip ...

while MAX_LENGTH < int(key[2]):
    DIFENT_STACK.append(___[MAX_LENGTH])
    MAX_LENGTH += 1
exec marshal.loads((lambda ____: base64.b64decode(____[1]))(key) + (lambda : uc[i][p] ^ c[0][o] ** 5 != '').__code__.co_consts[3].join([ chr(i) for i in DIFENT_STACK ]))
del DIFENT_STACK
del MAX_LENGTH
del ___
del key
del sot
```

* Bisa terlihat bahwa, pada bagian akhir dekompilasi tersebut, terdapat baris berikut ini:

```python
exec marshal.loads((lambda ____: base64.b64decode(____[1]))(key) + (lambda : uc[i][p] ^ c[0][o] ** 5 != '').__code__.co_consts[3].join([ chr(i) for i in DIFENT_STACK ]))
```

* Kita akan mengubah baris tersebut menjadi:

```python
import sys
import uncompyle6
uncompyle6.main.decompile(2.7, marshal.loads((lambda ____: base64.b64decode(____[1]))(key) + (lambda : uc[i][p] ^ c[0][o] ** 5 != '').__code__.co_consts[3].join([ chr(i) for i in DIFENT_STACK ])),sys.stdout)
```

* Simpan lalu jalankan script tersebut. Ternyata muncul pesan error berikut ini:

```bash
% python2.7 0.py
  File "0.py", line 15
    (lambda : uc[i][p] ^ c[0][o] ** 5 != '').__code__.co_consts[3].join([ chr(i) for None in (lambda ______, __, ____: [
SyntaxError: cannot assign to None
```

* Kemungkinan pesan kesalahan tersebut muncul karena adalah kekurangan ketika proses dekompilasi menggunakan pycdc. Dengan sedikit _googling_ , kita bisa menemukan bahwa kode tersebut adalah obfuscator [zasu](https://github.com/Sazxt/zasu). Dan dengan memperhatikan kode sumber obfuscator yang digunakan, maka kita dapat memperbaiki kesalahan dekompilasi oleh pycdc. Perhatikan bagian ini yang terdapat pada obfuscator zasu:

```python
        return 'import base64,marshal\nDIFENT_STACK = []\n(lambda ___ : sot.sort())(globals().update({(lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([chr(i) for i in (lambda _______, _, ___ : [(((((___>=_______)+(___>=___)+(_>=_))+((___>_)+(_______<=_)+(_<=___))+((___>=_)-(_______>___)))<<(((___>_)+(_<=___))+((_>=_______)+(___>=___))))+(((___>=_______)+(_______!=_)+(___<=___)))),(((((___>=___)+(___>=___)+(_______!=_))+((_______!=___)+(_______==_______)+(___!=_______))+((_>=_______)*(___>=_______)))<<(((_______!=_)+(___!=_))+((___!=_)+(_______<=_______))))-(((___>=___)*(_!=___)))),(((((___>_______)+(_>=_______)+(___>=___))+((_<___)+(_______<_)+(___!=_))+((_______>=_______)*(___!=_)))<<(((_______<___)+(_______<___))+((_______!=___)+(___!=_______))))+((((_==_)-(_______==_)))<<(((___!=_)+(_<___)))))])((((0x83273a==0x83273a)+(0b1001!=0o1663133)+(0x83273a!=0b1001))),((((((((((((0b1001>=0x83273a)+(0o1663133<0x83273a)))<<(((0x83273a<=0x83273a)+(0b1001>=0b1001)+(0o1663133!=0x83273a))))+(((0o1663133!=0x83273a)+(0b1001>0b1001)))))))))<<(((0b1001>0b1001)+(0b1001<=0b1001))))),(((((0x83273a==0x83273a)+(0b1001==0b1001)+(0b1001<=0x83273a))+((0x83273a>=0x83273a)+(0b1001<0x83273a)+(0o1663133!=0b1001))+((0b1001<=0o1663133)-(0x83273a<0b1001)))<<(((0b1001<=0o1663133)+(0x83273a<=0x83273a))))-(((0b1001!=0o1663133)*(0o1663133<=0x83273a)))))]):(lambda : [{int(c.split("$")[0])+int(c.split("$")[2]):c.split("$")[1]} for c in (lambda ___ : ___.split("@"))(\''+str(fo)+'\')])()}))\n(lambda : globals().update({(lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([chr(i) for i in (lambda ______, __, ____ : [((((((((____==____)+(______!=__)+(____>=__))+((____!=__)+(____>__)+(__<____))+((______>=__)*(______<=____)))<<(((__<=______)+(__==__))))-(((____>=__)*(__<____)))))<<(((__<____)+(____>______))))-(((______<=______)-(__>____)))),((((((((____>=__)+(______!=____)+(__>=__)))<<(((______!=__)+(______!=____)+(____>=______))))+(((__!=______)-(______>____)))))<<(((______<=______)+(____>__))))+(((______>=____)+(____!=__)))),((((((((____<=____)+(__!=__)))<<(((____>=______)+(______!=__))+((____>=____)+(__!=____))))-(((______!=____)+(__==____)))))<<(((__>=__)+(____<=____)+(____!=______))))+(((__==__)+(____<=__))))])((((((0o1663133!=0b1001)*(0b1001>=0b1001)))<<(((0x83273a>=0b1001)+(0x83273a!=0o1663133))+((0x83273a!=0b1001)+(0b1001!=0x83273a))))),(((((0x83273a>0o1663133)+(0o1663133<=0x83273a)+(0x83273a>0b1001))+((0x83273a==0x83273a)+(0x83273a>0b1001)+(0b1001!=0x83273a))+((0o1663133==0o1663133)+(0b1001>=0o1663133)))<<(((0b1001<0o1663133)*(0x83273a==0x83273a))))),(((((0b1001<=0o1663133)+(0o1663133<=0x83273a))+((0o1663133>0b1001)+(0x83273a<=0x83273a))+((0b1001<=0o1663133)+(0x83273a==0o1663133)))<<(((0x83273a<=0x83273a)+(0b1001!=0o1663133))))))]):(lambda cz : cz[::-1].split("&"))((lambda x : (lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([c[1][c[0]] for c in enumerate(x)]))(sot))}))()\nMAX_LENGTH = int(key[0])\n___ = '+ str(co["depth"]) +'\nwhile MAX_LENGTH<int(key[2]):\n    DIFENT_STACK.append(___[MAX_LENGTH])\n    MAX_LENGTH += 1\nexec(marshal.loads((lambda ____ : base64.b64decode(____[1]))(key)+(lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([chr(i) for i in DIFENT_STACK])))\ndel DIFENT_STACK,MAX_LENGTH,___,key,sot'
```

* Kita akan mengganti bagian yang salah pada proses dekompilasi dengan bagian yang benar tersebut. Sehingga hasilnya seperti ini:

```python
import base64
import marshal
DIFENT_STACK = []
(lambda ___ : sot.sort())(globals().update({(lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([chr(i) for i in (lambda _______, _, ___ : [(((((___>=_______)+(___>=___)+(_>=_))+((___>_)+(_______<=_)+(_<=___))+((___>=_)-(_______>___)))<<(((___>_)+(_<=___))+((_>=_______)+(___>=___))))+(((___>=_______)+(_______!=_)+(___<=___)))),(((((___>=___)+(___>=___)+(_______!=_))+((_______!=___)+(_______==_______)+(___!=_______))+((_>=_______)*(___>=_______)))<<(((_______!=_)+(___!=_))+((___!=_)+(_______<=_______))))-(((___>=___)*(_!=___)))),(((((___>_______)+(_>=_______)+(___>=___))+((_<___)+(_______<_)+(___!=_))+((_______>=_______)*(___!=_)))<<(((_______<___)+(_______<___))+((_______!=___)+(___!=_______))))+((((_==_)-(_______==_)))<<(((___!=_)+(_<___)))))])((((0x83273a==0x83273a)+(0b1001!=0o1663133)+(0x83273a!=0b1001))),((((((((((((0b1001>=0x83273a)+(0o1663133<0x83273a)))<<(((0x83273a<=0x83273a)+(0b1001>=0b1001)+(0o1663133!=0x83273a))))+(((0o1663133!=0x83273a)+(0b1001>0b1001)))))))))<<(((0b1001>0b1001)+(0b1001<=0b1001))))),(((((0x83273a==0x83273a)+(0b1001==0b1001)+(0b1001<=0x83273a))+((0x83273a>=0x83273a)+(0b1001<0x83273a)+(0o1663133!=0b1001))+((0b1001<=0o1663133)-(0x83273a<0b1001)))<<(((0b1001<=0o1663133)+(0x83273a<=0x83273a))))-(((0b1001!=0o1663133)*(0o1663133<=0x83273a)))))]):(lambda : [{int(c.split("$")[0])+int(c.split("$")[2]):c.split("$")[1]} for c in (lambda ___ : ___.split("@"))('-2926$i$6532@-3122$S$7333@

    #... snip ...

    1128$0$2686@-2729$A$7743@-3584$T$16005')])()}))

(lambda : globals().update({(lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([chr(i) for i in (lambda ______, __, ____ : [((((((((____==____)+(______!=__)+(____>=__))+((____!=__)+(____>__)+(__<____))+((______>=__)*(______<=____)))<<(((__<=______)+(__==__))))-(((____>=__)*(__<____)))))<<(((__<____)+(____>______))))-(((______<=______)-(__>____)))),((((((((____>=__)+(______!=____)+(__>=__)))<<(((______!=__)+(______!=____)+(____>=______))))+(((__!=______)-(______>____)))))<<(((______<=______)+(____>__))))+(((______>=____)+(____!=__)))),((((((((____<=____)+(__!=__)))<<(((____>=______)+(______!=__))+((____>=____)+(__!=____))))-(((______!=____)+(__==____)))))<<(((__>=__)+(____<=____)+(____!=______))))+(((__==__)+(____<=__))))])((((((0o1663133!=0b1001)*(0b1001>=0b1001)))<<(((0x83273a>=0b1001)+(0x83273a!=0o1663133))+((0x83273a!=0b1001)+(0b1001!=0x83273a))))),(((((0x83273a>0o1663133)+(0o1663133<=0x83273a)+(0x83273a>0b1001))+((0x83273a==0x83273a)+(0x83273a>0b1001)+(0b1001!=0x83273a))+((0o1663133==0o1663133)+(0b1001>=0o1663133)))<<(((0b1001<0o1663133)*(0x83273a==0x83273a))))),(((((0b1001<=0o1663133)+(0o1663133<=0x83273a))+((0o1663133>0b1001)+(0x83273a<=0x83273a))+((0b1001<=0o1663133)+(0x83273a==0o1663133)))<<(((0x83273a<=0x83273a)+(0b1001!=0o1663133))))))]):(lambda cz : cz[::-1].split("&"))((lambda x : (lambda : uc[i][p] ^ c[0][o] ** 5 != "").__code__.co_consts[3].join([c[1][c[0]] for c in enumerate(x)]))(sot))}))()

MAX_LENGTH = int(key[0])
___ = [
    48305,
    48312,
    48345,

    #... snip ...

    32,
    41,
    15,
    47]

while MAX_LENGTH < int(key[2]):
    DIFENT_STACK.append(___[MAX_LENGTH])
    MAX_LENGTH += 1

import sys
import uncompyle6
uncompyle6.main.decompile(2.7, marshal.loads((lambda ____: base64.b64decode(____[1]))(key) + (lambda : uc[i][p] ^ c[0][o] ** 5 != '').__code__.co_consts[3].join([ chr(i) for i in DIFENT_STACK ])),sys.stdout)

del DIFENT_STACK
del MAX_LENGTH
del ___
del key
del sot
```

* Kembali jalankan script tersebut menggunakan python2.7 dan simpan hasilnya sebagai **script_2.5.7.py**:

```bash
% python2.7 0.py > script-v2.5.7.py
```

* Setelah menjalankan perintah di atas, maka hasilnya akan tersimpan pada file **script_2.5.7.py**. Berikut ini adalah potongan kode hasil dekompilasi script tersebut:

```python
# uncompyle6 version 3.7.3
# Python bytecode 2.7
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: zasu
import md5, itertools, urllib2, os, sys, time, random, socket, select, datetime, threading
from os import system, name
os.system('clear')
version = '2.5.7       '
expired = ['07', '10', '2020']
buffer_size = 32768
host = '0.0.0.0'
port = '8080'
logo = "\t_______             _______\n\t|@|@|@|@|           |@|@|@|@|\n\t|@|@|@|@|   _____   |@|@|@|@|\n\t|@|@|@|@| /\\_T_T_/\\ |@|@|@|@|\n\t|@|@|@|@||/\\ T T /\\||@|@|@|@|\n\t ~/T~~T~||~\\/~T~\\/~||~T~~T\\~\n\t  \\|__|_| \\(-(O)-)/ |_|__|/\n\t  _| _|    \\8_8//    |_ |_\n\t|(@)]   /~~[_____]~~\\   [(@)|\n\t  ~    (  |       |  )    ~\n\t      [~` ]       [ '~]\n\t      |~~|         |~~|\n\t      |  |         |  |\n\t     _<\\/>_       _<\\/>_\n\t    /_====_\\     /_====_\\"
Tsel_Ruangguru = [
 '118.98.93.6:443', 'video-cdn.quipper.com:443', '118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Ilmupedia = ['118.98.104.36:443', '118.98.95.106:443']
Tsel_Gamesmax = ['118.98.95.91:443', '118.98.95.106:443']
Tsel_Musicmax = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Videomax = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Gigamax = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Maxtream = ['118.98.93.65:443', '118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Whatsapp = ['118.98.95.106:443', '118.98.93.91:443', '118.98.95.91:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Omg = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Tiktok = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Youtube = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Instagram = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Facebook = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_Conference = ['118.98.95.106:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Tsel_0p0k = ['118.97.159.51:80']
Tsel_KB = ['118.98.95.91:443', '118.98.95.106:443']
Isat_Edukasi = ['video-cdn.quipper.com:443']
Axis_Ruangguru = ['rubel-video-cdn.ruangguru.com.akamaized.net:443', 'video-cdn.quipper.com:443', '23.219.184.41:443']
Axis_Gaming = ['www.pubgmobile.com:443']
Axis_Gaming_V2 = ['112.215.101.83:443']
Axis_Viu = ['rubel-video-cdn.ruangguru.com.akamaized.net', 'iflix-videocdn-p2.akamaized.net', '23.219.184.41', '23.219.184.24']
Axis_Kwai = ['rubel-video-cdn.ruangguru.com.akamaized.net', 'iflix-videocdn-p2.akamaized.net', '23.219.184.41', '23.219.184.24']
Axis_Sosmed = ['23.45.116.48:443', '23.219.184.41:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Axis_Iflix = ['iflix-videocdn-p1.akamaized.net', 'iflix-videocdn-p2.akamaized.net', 'iflix-videocdn-p3.akamaized.net', 'iflix-videocdn-p6.akamaized.net', 'iflix-videocdn-p7.akamaized.net', 'iflix-videocdn-p8.akamaized.net', 'rubel-video-cdn.ruangguru.com.akamaized.net:443', 'video.iflix.com:443', '23.219.184.41:443']
Axis_Tiktok = ['rubel-video-cdn.ruangguru.com.akamaized.net', 'iflix-videocdn-p2.akamaized.net', '23.219.184.41', '23.219.184.24']
Xl_Ruangguru = ['rubel-video-cdn.ruangguru.com.akamaized.net:443', 'video-cdn.quipper.com:443']
Xl_Iflix = ['iflix-videocdn-p1.akamaized.net', 'iflix-videocdn-p2.akamaized.net', 'iflix-videocdn-p3.akamaized.net', 'iflix-videocdn-p6.akamaized.net', 'iflix-videocdn-p7.akamaized.net', 'iflix-videocdn-p8.akamaized.net', 'video.iflix.com:443', 'rubel-video-cdn.ruangguru.com.akamaized.net:443']
Xl_Joox = ['ak-quic.stream.music.joox.com.edgesuite.net', 'ak-hk.stream.music.joox.com.edgesuite.net', 'ak-ng.stream.music.joox.com.edgesuite.net', 'ak-quic.app.joox.com.edgesuite.net', 'ak-ng.app.joox.com.edgesuite.net', 'e5121.b.akamaiedge.net']
Xl_Tiktok = ['rubel-video-cdn.ruangguru.com.akamaized.net:443', 'iflix-videocdn-p1.akamaized.net', 'iflix-videocdn-p2.akamaized.net', 'iflix-videocdn-p3.akamaized.net', 'iflix-videocdn-p6.akamaized.net', 'iflix-videocdn-p7.akamaized.net', 'iflix-videocdn-p8.akamaized.net', 'video.iflix.com:443']
Xl_Freefire = ['filecdn-igamecj.akamaized.net:443']
Xl_Pubg = ['www.pubgmobile.com:443']
Smart_Ruangguru = ['rubel-video-cdn.ruangguru.com.akamaized.net:443']
Smart_Tiktok = ['rubel-video-cdn.ruangguru.com.akamaized.net:443', 'iflix-videocdn-p1.akamaized.net', 'iflix-videocdn-p2.akamaized.net', 'iflix-videocdn-p3.akamaized.net', 'iflix-videocdn-p6.akamaized.net', 'iflix-videocdn-p7.akamaized.net', 'iflix-videocdn-p8.akamaized.net', 'video.iflix.com:443']
Three_Chatting = ['202.67.41.120:443', '202.67.41.80:443']


#... snip ...


    def run(self):
        try:
            self.proxy_host_port = random.choice(self.frontend_domains).split(':')
            self.proxy_host = self.proxy_host_port[0]
            self.proxy_port = self.proxy_host_port[1] if len(self.proxy_host_port) >= 2 and self.proxy_host_port[1] else '443'
            connecting()
            self.socket_tunnel.connect((str(self.proxy_host), int(self.proxy_port)))
            self.socket_client.sendall('HTTP/1.0 200 OK\r\n\r\n')
            self.handler(self.socket_tunnel, self.socket_client, self.buffer_size)
            self.socket_client.close()
            self.socket_tunnel.close()
            connected()
        except:
            connection_error()


if __name__ == '__main__':
    try:
        login()
    except KeyboardInterrupt:
        sys.exit()
```

* Dengan demikian, proses dekompilasi script sobat gretonger v2.5.7 selesai.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
