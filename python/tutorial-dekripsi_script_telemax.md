# Tutorial Dekripsi Script Telemax


## Pendahuluan

Pada tutorial kali ini, kita akan melakukan dekripsi dan deobfuscate terhadap script telemax v7.2. 


## Langkah-Langkah

* Pertama, kita buka file **telemax7.2.py** menggunakan teks editor. Isinya kurang lebih seperti ini:

```python
import base64, codecs
magic = 'IyMjIyAgIC ... VTXNOMk'
love = '1CnT1Kr ... fZSSPMSqG'
god = 'REYwWkpO ... FdTREYwWk'
destiny = 'gfZ2SCGy ...n2I5XDb='
joy = '\x72\x6f\x74\x31\x33'
trust = eval('\x6d\x61\x67\x69\x63') + eval('\x63\x6f\x64\x65\x63\x73\x2e\x64\x65\x63\x6f\x64\x65\x28\x6c\x6f\x76\x65\x2c\x20\x6a\x6f\x79\x29') + eval('\x67\x6f\x64') + eval('\x63\x6f\x64\x65\x63\x73\x2e\x64\x65\x63\x6f\x64\x65\x28\x64\x65\x73\x74\x69\x6e\x79\x2c\x20\x6a\x6f\x79\x29')
eval(compile(base64.b64decode(eval('\x74\x72\x75\x73\x74')),'<string>','exec'))
```

* Ganti baris terakhir pada file tersebut:

```python
## ganti dari
eval(compile(base64.b64decode(eval('\x74\x72\x75\x73\x74')),'<string>','exec'))

## menjadi
print base64.b64decode(eval('\x74\x72\x75\x73\x74'))
```

* Setelah itu, jalankan file tersebut dan simpan hasilnya pada file sementara yang kemudian akan menimpa file **telemax7.2.py** :

```bash
% python2 telemaxv7.2.py > tmp && mv -f tmp telemaxv7.2.py
```

* Bisa terlihat bahwa file **telemax7.2.py** kini berubah dan isinya menjadi kurang lebih seperti ini:

```python
####        ####
###            ####
import getpass,hashlib,base64
import requests, sys, os
def hasher(text,length,key):

# ... snip ...

def unlock(key):
    exec (decrypt("3a9f549d73 ... 273b515e",key),globals())

if "__main__" == __name__:
   c = requests.Session()
   key = c.get("https://pastebin.com/raw/HxGFSMKs").text
   unlock(key)
```

* Ada 2 hal yang perlu diperhatikan pada langkah di atas, yaitu _key_ untuk dekripsi yang terdapat pada tautan di [pastebin](https://pastebin.com/raw/HxGFSMKs) dan pemanggilan `exec` pada fungsi `unlock`. Kita cukup mengubah script di atas, yaitu mengubah pemanggilan fungsi `exec` menjadi fungsi `print` seperti ini:

```python
####        ####
###            ####
import getpass,hashlib,base64
import requests, sys, os
def hasher(text,length,key):

# ... snip ...

def unlock(key):
    print (decrypt("3a9f549d73 ... 273b515e",key),globals())

if "__main__" == __name__:
   c = requests.Session()
   key = c.get("https://pastebin.com/raw/HxGFSMKs").text
   unlock(key)
```

* Setelah itu, jalankan script tersebut:

```bash
% python telemaxv7.2.py > tmp && mv -f tmp telemaxv7.2.py
```

* Perintah di atas akan menimpa file telemax7.2.py dengan hasil _unlock_ , dan hasilnya seperti ini:

```python
("import base64\nexec(base64.b64decode('=kCK0NWZu52b ... '[::-1]))", {'sys': ... '__doc__': None})
```

* Edit file tersebut untuk memperbaiki formatnya dan ganti fungsi `exec` menjadi `print` seperti ini:

```python
import base64
print(base64.b64decode('=kCK0NWZu52 ... 0BSbvJnZ'[::-1]))
```

* Jalankan file tersebut dan simpan hasilnya dengan menimpa file aslinya:

```bash
% python telemaxv7.2.py > tmp && mv -f tmp telemaxv7.2.py
```

* Hasilnya adalah script telemax7.2.py yang sudah berhasil di-dekripsi dan di-deobfuscate. Berikut ini adalah potongan scriptnya:

```python
from telethon import TelegramClient, sync, events, errors, connection
from telethon.tl.functions.messages import *
from telethon.tl.types import *
from telethon.tl.functions.messages import *
from telethon.tl.functions.account import *
from telethon.tl.functions.channels import UpdateUsernameRequest as chusername
from telethon.tl.functions.channels import *
from telethon.errors.rpcerrorlist import *
from telethon.tl.types import *
from telethon.tl.functions.contacts import *
from telethon.errors import *
from telethon.tl.functions import *
import traceback
from json.decoder import JSONDecodeError
from sqlite3 import *
from datetime import datetime
from time import sleep
from bs4 import BeautifulSoup
import pytz, json, re, sys, os, requests, time, random, colorama, threading, itertools, stat, string
import asyncio
import getpass
import logging
import datetime
a=time.localtime()
hr=a.tm_hour
if hr < 4 :
    ucap = "Selamat Malam!"
else:
    if hr < 11:
        ucap = "Selamat Pagi,, Selamat Beraktifitas!"
    else:
        if hr < 16 :
            ucap = "Selamat Siang!"
        else:
            if hr < 19 :
                ucap = "Selamat Sore!"
            else:
                ucap = "Selamat Malam Semuanya!"
mn=a.tm_min
sc=a.tm_sec
dy=a.tm_mday
xxtime=('{}:{}:{}'.format(hr,mn,sc))


# ... snip ...


except KeyboardInterrupt:
    print(f'\n\n\r{abu2}[{merah}{xxtime}{abu2}] {merah}Exit.....!!! \n')
    sleep(1)
    exit()
finally:
    client.disconnect()
```


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
