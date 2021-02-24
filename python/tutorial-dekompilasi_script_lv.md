# Dekompilasi Script Auto Leave Channel/Group


Tutorial singkat ini akan menjelaskan cara melakukan dekompilasi script auto leave channel/group yang dibuat menggunakan python 3.8. Berikut ini adalah langkah-langkahnya:

Pertama, kita cari tahu dulu tipe dari script tersebut:

```bash
% file lv.py
lv.py: python 3.8 byte-compiled
```

Setelah itu kita ubah ekstensinya menjadi `pyc` agar dapat didekompilasi menggunakan uncompyle6:

```bash
% mv lv.py lv.pyc
```

Gunakan uncompyle6 untuk melakukan dekompilasi:

```bash
% uncompyle6 lv.pyc
Error: uncompyle6 requires Python 2.6-3.8
```

Wah, error. Itu terjadi karena pada komputer yang digunakan untuk melakukan dekompilasi telah menggunakan python terbaru yaitu python 3.9. Selanjutnya kita akan melihat _printable string_ pada script tersebut.

```bash
% strings lv.pyc
Nsl'
I0NvbXBpbGUgQmVybGFwaXM ... XHgwMScpKQ==z
utf-8
ignore)
base64
exec
        b64decode
decode
<script>
<module>
marshal
exec
loads
<module>
```

Ternyata script tersebut menggunakan teknik yang cukup sederhana, yaitu menggunakan _base64_ dan _marshal_ . Kita dapat menyalin bagian yang di-encode menggunakan _base64_ pada script tersebut dan gunakan perintah di terminal untuk melakukan decode seperti ini:

```bash
% echo -n "I0NvbXBpbGUgQm ... XHgwMScpKQ==" | base64 -d > 0.py
```

Hasilnya akan disimpan pada file **0.py** yang isinya seperti ini:

```python
#Compile Berlapis
#By XUZUT

import marshal
exec(marshal.loads(b'\xe3\x00\x00 ... \x01\x12\x01"\x01'))
```

Edit file tersebut sehingga menjadi seperti ini:

```python
#Compile Berlapis
#By XUZUT

import sys
import marshal
import uncompyle6

uncompyle6.main.decompile(3.8,marshal.loads(b'\xe3\x00\x00\x00 ... \x01\x12\x01"\x01'),sys.stdout)
```

Jalankan script tersebut dan simpan hasilnya pada file **lv.py** :

```bash
% python 0.py > lv.py
```

Hasil akhirnya adalah script yang telah didekompilasi berikut ini:

```python
from telethon import TelegramClient, sync, events, errors
from telethon.tl.functions.messages import GetHistoryRequest, GetBotCallbackAnswerRequest
from telethon.tl.functions.channels import JoinChannelRequest
from telethon.errors import SessionPasswordNeededError
from telethon.errors import FloodWaitError
from time import sleep
import json, re, sys, os, requests, time, random, colorama, threading, itertools
from bs4 import BeautifulSoup
import telethon.sync
from telethon import utils
from telethon import TelegramClient
from telethon.tl.types import Channel
from telethon.utils import get_display_name
from telethon.tl.functions.channels import LeaveChannelRequest
from telethon.tl.functions.messages import DeleteMessagesRequest
a = time.localtime()
hr = a.tm_hour
mn = a.tm_min
sc = a.tm_sec
xtime = '{}:{}:{}'.format(hr, mn, sc)
try:
    import colorama
    from colorama import Fore, Back, Style
    colorama.init(autoreset=True)
    hijau = Style.RESET_ALL + Style.BRIGHT + Fore.GREEN
    res = Style.RESET_ALL
    abu2 = Style.DIM + Fore.WHITE
    biru = Style.RESET_ALL + Style.BRIGHT + Fore.BLUE
    ungu2 = Style.NORMAL + Fore.MAGENTA
    ungu = Style.RESET_ALL + Style.BRIGHT + Fore.MAGENTA
    hijau2 = Style.NORMAL + Fore.GREEN
    yellow2 = Style.NORMAL + Fore.YELLOW
    yellow = Style.RESET_ALL + Style.BRIGHT + Fore.YELLOW
    red2 = Style.NORMAL + Fore.RED
    red = Style.RESET_ALL + Style.BRIGHT + Fore.RED
    cyan = Style.BRIGHT + Fore.CYAN
    cyan2 = Style.NORMAL + Fore.CYAN
    des = Style.BRIGHT + Fore.GREEN + '『🔥』'
    kur1 = Style.BRIGHT + Fore.RED + '['
    kur2 = Style.BRIGHT + Fore.RED + ']'
except:
    print('Please Install Modul Colorama!!\n\n\n')
    sys.exit()
else:
    os.system('cls' if os.name == 'nt' else 'clear')
    c = requests.session()
    banner = ' \x1b[1;33m                          /-   /      /*\n                          /"!  /"!    /*)\n                          !`"!  !!!   |*{\n                       /-  !" !  iii  |€}\n                      /!"!  !" !  !!! |\\|\n                     /`*!"!  !" !  i| [%]\n                    {    !"!  != !/π/ |~}\n                   {_#####~!"\\ /*/    ]¤[\n                            \\|.:/     @1/\n                                      \'/\n\n\x1b[1;32m ============================================================\n\x1b[1;31m [\x1b[1;36m ©CREATED BY ANTO XUZUT™ \x1b[1;31m]\n\x1b[1;32m Channel Telegram : @kuli_online_channel\n\x1b[1;32m Channel Youtube  : Andi Kacho\n\x1b[1;32m Support By       : @AndiKacho & @Rozza15\n\x1b[1;32m Donasi           : D7wnb429X7NYzmSA8Xy8WescWnj9dqG8mP(Doge)\n\x1b[1;32m ============================================================\n\x1b[1;31m [\x1b[1;36m SCRIPT AUTO LEAVE CHANNEL/GROUP \x1b[1;31m]\n\x1b[1;32m ============================================================\n'
    print(banner)
    if not os.path.exists('session'):
        os.makedirs('session')
    api_id = 974754
    api_hash = '6295657bbae725bfe8dfcca5d9e323e6'
    phone_number = sys.argv[1]
    if len(sys.argv) < 2:
        print('Usage: python bch.py phone_number')
        print('-> Input number in international format (example: +639162995600)\n')
        e = input('Press any key to exit...')
        exit(1)
    client = TelegramClient('session/' + phone_number, api_id, api_hash)
    client.connect()
    if not client.is_user_authorized():
        try:
            client.send_code_request(phone_number)
            me = client.sign_in(phone_number, input('\n\n\n\x1b[1;0mEnter Your Code : '))
        except SessionPasswordNeededError:
            passw = input('\x1b[1;0mYour 2fa Password : ')
            me = client.start(phone_number, passw)

    myself = client.get_me()
    print(f"\x1b[1;35mWELCOME         :\x1b[1;36m {myself.first_name}\n\x1b[1;35mUSERNAME        :\x1b[1;36m {myself.username}\n\x1b[1;35mYOUR NUMBER     :\x1b[1;36m {phone_number}")
    sys.stdout.write('\r')
    sys.stdout.write('                                                              ')
    sys.stdout.write('\r')
    sys.stdout.write(f"\r{abu2}[{yellow}Waiting!{abu2}]{yellow} Starting Bot\n")
    sys.stdout.flush()
    sleep(2)
    sys.stdout.write('\r')
    sys.stdout.write('                                                              ')
    sys.stdout.write('\r')
    sys.stdout.write(f"\r{abu2}[{hijau}✅{abu2}]{hijau} Working\n")
    sys.stdout.flush()
    dialogs = client.get_dialogs()
    for dialog in dialogs:
        if type(dialog.entity) is Channel:
            print(f"{abu2}[{hijau}✅{abu2}]{hijau} Leave Channel/Group {biru}" + dialog.name)
            client(LeaveChannelRequest(dialog.entity))
        print(f"\n{abu2}[{red}+{abu2}] {red}Done....Exit....!!!")
        sys.exit()
```

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
