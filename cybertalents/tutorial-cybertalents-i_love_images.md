## [Writeup] Cyber Talents: I Love Images


Tutorial ini akan membahas solusi soal CTF [Cyber Talents: I Love Images](https://cybertalents.com/challenges/forensics/i-love-images) yang merupakan kategori _digital forensic_. Adapun deskripsi untuk soal ini adalah sebagai berikut:

```
A hacker left us something that allows us to track him in this image, can you find it?
```

Dan kita akan diberikan soal berupa file dalam format gambar PNG yang dapat [diunduh pada tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Forensics/godot.png). Unduh terlebih dahulu file tersebut lalu lanjutkan dengan mencari printable string padd file tersebut:

```bash
% strings godot.png
...
IZGECR33JZXXIX2PNZWHSX2CMFZWKNRUPU======
```

Perhatikan bahwa pada bagian akhir file tersebut terdapat sebuah _string_ yang sepintas mirip dengan encode Base64, namun dengan memperhatikan jumlah karakter **=** pada string tersebut, maka kita bisa menyimpulkan bahwa itu adalah _string_ dengan encode Base32. Kita akan menggunakan perintah berikut untuk melakukan _decode_ terhadap string tersebut:

```bash
% echo -n 'IZGECR33JZXXIX2PNZWHSX2CMFZWKNRUPU======' | base32 -d
FLAG{Not_Only_Base64}
```

Nah, flag untuk soal kali ini adalah `FLAG{Not_Only_Base64}`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
