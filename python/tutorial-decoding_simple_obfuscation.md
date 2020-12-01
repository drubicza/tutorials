Decoding Simple Obfuscation


Pada tutorial singkat kali ini, kita akan melakukan _decoding_ terhadap sebuah file python bytecompiled yang bernama **sms.py**. Langkah pertama yang harus dilakukan adalah mencari tahu tipe dari file tersebut:

```bash
% file sms.py 
sms.py: python 3.8 byte-compiled
```

Ternyata file tersebut merupakan python bytecompiled. Jika kita melihat _string_ yang terdapat pada file tersebut, maka isinya adalah kurang lebih seperti ini:

```
...
#*!*!*!*#^#*!*!*!*#^^#*! ... ^#*!*!*!*#^#*!*!*!*#)
...
base64
exec
b64decode
```

Coba kita salin string yang berupa tanda baca tersebut dan simpan ke dalam file **bot.py** dengan format seperti ini:

```python
from base64 import b64decode

print(b64decode("#*!*!*!*#^#*! ... *#^#*!*!*!*#"))
```

Setelah itu, jalankan script tersebut:

```bash
% python3 wow.py
```

Hasilnya adalah script berikut ini:

```python
b'import os,sys,requests,json,time\ndef clear():\n  ... Periksa kembali koneksi anda\\033[00m") \n     balik()\n'
```

Jika diformat ulang dan dirapikan, maka script tersebut bentuknya seperti ini:

```python
import os,sys,requests,json,time

def clear():
    os.system("clear")

def kata(s):
    for c in s + "\n":
        sys.stdout.write(c)
        sys.stdout.flush()
        time.sleep(1./140)

def load():
    for x in range(1,101):
        time.sleep(1./30)
        print(f"\r\033[1;97m[\033[1;96m!\033[1;97m] Loading...(\033[1;92m{x}\033[90m%\033[1;97m)", end="", flush=True)

def balik():
    f = input("\033[1;97m[\033[1;91m?\033[1;97m] Kirim kembali? (y/t): ")
    if f == "y":
       os.system("python sms.py")
    else:
       sys.exit("\033[1;97m[\033[1;91m!\033[1;97m]\033[1;91mExit\033[1;97m")

def baner():
    kata("""
\033[1;97m\t
\033[1;91mxe2x95x94xe2x95x90xe2x95x97\033[1;97mxe2x94x8cxe2x94xacxe2x94x90xe2x94x8cxe2x94x80xe2x94x90   \033[33;1mxe2x95x94xe2x95x90xe2x95x97\033[1;97mxe2x94xacxe2x94x80xe2x94x90xe2x94x8cxe2x94x80xe2x94x90xe2x94x8cxe2x94xacxe2x94x90xe2x94xacxe2x94x8cxe2x94x80xe2x94x90
\033[1;91mxe2x95x9axe2x95x90xe2x95x97\033[1;97mxe2x94x82xe2x94x82xe2x94x82xe2x94x94xe2x94x80xe2x94x90xe2x94x80xe2x94x80xe2x94x80\033[33;1mxe2x95x91 xe2x95xa6\033[1;97mxe2x94x9cxe2x94xacxe2x94x98xe2x94x9cxe2x94x80xe2x94xa4 xe2x94x82 xe2x94x82xe2x94x94xe2x94x80xe2x94x90
\033[1;91mxe2x95x9axe2x95x90xe2x95x9d\033[1;97mxe2x94xb4 xe2x94xb4xe2x94x94xe2x94x80xe2x94x98   \033[33;1mxe2x95x9axe2x95x90xe2x95x9d\033[1;97mxe2x94xb4xe2x94x94xe2x94x80xe2x94xb4 xe2x94xb4 xe2x94xb4 xe2x94xb4xe2x94x94xe2x94x80xe2x94x98
\x1b[00m\033[41m   SMS GRATIS ALL OPRATOR   \033[00m
\033[1;97m[\033[1;91mxe2x80xa2\033[1;97m] Author : \033[33;1mAn brush fon
\033[1;97m[\033[1;91mxe2x80xa2\033[1;97m] Github : \033[4;92mhttps://github.com/Cadot-ID\033[00m
\033[1;94m________________________________________
\033[1;92mNote:\033[1;97m Pesan minimal 15 karakter
\033[1;94m________________________________________""")

if __name__=="__main__":
     clear()
     baner()
     try:
          no = input("\033[1;97m[\033[1;92m+\033[1;97m] Nomor target: \033[33;1m")
          msg = input("\033[1;97m[\033[1;92m+\033[1;97m] Pesan: \033[1;92m")
          dat = {"number": no, "pesan": msg}
          load()
          br = requests.post("https://nuubi.herokuapp.com/api/smsgratis", data=dat).text
          if "SMS Gratis Telah Dikirim" in br:
              print(f"\n\033[1;97m[\033[1;92mxe2x9cx93\033[1;97m] SMS KE  \033[1;96m{no} \033[1;92mSuccess")
          elif "Terjadi kesalahan!" in br:
              kata("\n\033[1;97m[\033[1;91mx\033[1;97m] Terjadi Kesalahan\033[1;91m!!!\033[00m")
          else:
              print(f"\n\033[1;97m[\033[1;91mx\033[1;97m] SMS KE  \033[1;96m{no} \033[1;91mGagal\033[00m")

     except TypeError:
            print("\033[1;97m\033[1;91mxe2x80xa2\033[1;97m] Nomor salah\033[1;91m!\033[00m")

     except (KeyboardInterrupt,EOFError):
            sys.exit()

     except requests.exceptions.ConnectionError:
            print("\033[1;97m[\033[1;91m!\033[1;97m]\033[1;91mPeriksa kembali koneksi anda\033[00m")
     balik()
```

Demikan tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
