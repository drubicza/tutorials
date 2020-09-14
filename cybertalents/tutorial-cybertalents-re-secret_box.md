## [Writeup] Cyber Talents: Secret Box

Ini adalah writeup soal latihan [Cyber Talents](https://cybertalents.com/challenges/malware/secret-box) untuk kategori reverse engineering. Berikut ini adalah petunjuk yang diberikan:

```
There is a secret in the box, can you read this?
```

Adapun soalnya berupa arsip `secretbox.zip` yang berisi 2 buah file yaitu `secretbox.py` dan `secret.png`. Berikut ini adalah isi dari script `secretbox.py`:

```python
import sys
from PIL import Image

def prob(s_img, msg, d_img):
        im = Image.open(s_img).convert("RGBA")
        p = im.load()
        c = 0
        msg = map(lambda x: ord(x) ^ len(d_img), msg[::-1])
        for i in range(0, len(msg)):
                enc = msg[i]
                p[c, 0] = (p[c, 0][0], p[c, 0][1], p[c, 0][2], enc)
                c += 1
        im.save(d_img)

if len(sys.argv) != 4:
        print "%s \"orignal.png\" \"secret message\" \"secret.png\"" % sys.argv[0]
        exit()

prob(sys.argv[1], sys.argv[2], sys.argv[3])
```

Script python tersebut akan melakukan hal berikut ini:
* Mengubah format gambar imput dengan menambahkan channel alpha.
* Flag akan disimpan dalam format reversed.
* Melakukan operasi XOR terhadap setiap byte pada flag dengan panjang karakter pada nama file output.
* Menyimpan setiap byte hasil operasi XOR ke dalam channel alpha pada gambar output.

Untuk menemukan flag, script tersebut akan kita modifikasi dengan membalikkan metode yang digunakan:
* Baca gambar `secret.png` dengan format RGBA (Red Green Blue Alpha).
* Ambil byte pada channel alpha (misalnya 32 byte).
* Lakukan operasi XOR untuk setiap byte tersebut: (byte ^ len('secret.png')) atau (byte ^ 10).
* Reverse string flag, untuk menghasilkan flag yang valid.

Berikut ini adalah script yang akan kita gunakan:

```python
#!/usr/bin/env python
import sys
from PIL import Image

def deprob(s_img):
    p = Image.open(s_img).convert("RGBA").load()
    print ''.join(map(chr, (p[i,0][3] ^ len(s_img) for i in range(0,32))))[::-1]

deprob("secret.png")
```

Simpan script tersebut dengan nama `getflag.py` pada direktori yang sama dengan file gambar `secret.png`, lalu jalankan:

```bash
% python getflag.py
flag{1t_is_very_light_b0x}
```

Flag untuk soal tersebut adalah `flag{1t_is_very_light_b0x}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
