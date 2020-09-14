## [Writeup] Cyber Talents: Postbase


Ini adalah writeup soal latihan [Cyber Talents](https://cybertalents.com/challenges/cryptography/postbase) untuk kategori cryptography dengan level mudah (_easy_). Berikut ini adalah petunjuk pada soal tersebut:

```
We got this letters and numbers and don&#039;t understand them. Can you? R[corrupted]BR3tCNDUzXzYxWDdZXzRSfQ==
```

Bisa terlihat bahwa soal tersebut berupa string yang di-encode menggunakan Base64, namun tidak lengkap (_corrupted_). Selanjutnya, kita akan mencoba mencari berapa karakter yang tidak ada dari string tersebut. Salah satu caranya adalah menggunakan perintah/aplikasi `base64` pada terminal. Atau bisa juga menggunakan cara lain, misalnya menggunakan modul yang terdapat pada scripting seperti php, python dan lain-lain. Jika menggunakan perintah `base64` pada terminal, lakukan seperti ini:

```bash
$ echo -n 'RBR3tCNDUzXzYxWDdZXzRSfQ==' | base64 -d
Dw#CS5cuE'base64: invalid input
```

Bisa terlihat, jika bagian `[corrupted]` dihilangkan, maka hasilnya akan invalid. Lanjutkan dengan mengganti bagian `[corrupted]` tersebut dengan karakter yang valid untuk encode base64, misalnya huruf `A`.

```bash
$ echo -n 'RABR3tCNDUzXzYxWDdZXzRSfQ==' | base64 -d
Wbase64: invalid input
```

Hasilnya masih invalid. Tambahkan lagi karakter, sehingga menjadi `AA`:

```bash
$ echo -n 'RAABR3tCNDUzXzYxWDdZXzRSfQ==' | base64 -d
DG{B453_61X7Y_4R}
```

Nah, sekarang sudah bisa terlihat. Kemungkinan flagnya adalah `{B453_61X7Y_4R}`. Namun agar lebih jelas, kita akan membuat skrip untuk mencari flag yang benar. Berikut ini adalah scriptnya:

```python
#!/usr/bin/env python
import sys
import base64

str_base64 = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"

for a in str_base64:
    for b in str_base64:
        try:
            str_flag = base64.b64decode("R" + a + b + "BR3tCNDUzXzYxWDdZXzRSfQ==")
            if ("flag" in str_flag) or ("FLAG" in str_flag):
                print "Found: {}".format(str_flag)
                sys.exit(0)
        except:
            continue
```

Simpan potongan kode di atas sebagai `cek.py` atau beri nama sesuai keinginan. Set sebagai _executable_ dengan perintah `chmod +x cek.py` lalu jalankan atau bisa juga dijalankan dengan perintah `python cek.py` seperti ini:

```bash
$ chmod +x cek.py
$ ./cek.py
Found: FLAG{B453_61X7Y_4R}

$ python cek.py
Found: FLAG{B453_61X7Y_4R}
```

Bisa terlihat bahwa flag yang benar adalah `FLAG{B453_61X7Y_4R}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
