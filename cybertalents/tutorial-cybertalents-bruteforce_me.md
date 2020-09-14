## [Writeup] Cyber Talents: Bruteforce Me


Tutorial kali ini akan membahas solusi untuk soal latihan [Cyber Talents: Bruteforce Me](https://cybertalents.com/challenges/malware/bruteforce-me) yang merupakan kategori Malware Reverse Engineering. Berikut ini adalah deskripsi dari soal tersebut:

```
flag format flag{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx},only a-z,0-9,_ are allowed.
try to find the only flag that makes sense.
Note: no special hardware is required to bruteforce

https://www.youtube.com/watch?v=hyk46UmJPS4 this may help you coding the solution.
DON'T BRUTEFORCE KEY SUBMISSIONS.
```

Soalnya berupa script python yang dapat [diunduh pada tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Reverse/bruteforceme.py). Berikut ini adalah kode sumber soal tersebut:

```python
ll =[51, 54, 48, 51, 61, 57, 50, 54, 48, 52, 55, 50, 50, 57, 47, 52, 57, 47, 54, 24, 57, 58, 62]

i = raw_input()
ss= 0
try:
    for ii in range(0,46 , 2):
        temp = i[ii:ii+2]
        temp = int(temp,0x10)
        ss+=temp
        temp >>=1
        if temp != ll[ii/2]:
            print "Something is wrong"
    if ss !=2406:
            print ss/0
    print "This flag may or may not work, can you find more ?"

except:
    print "NO"
```

Berikut ini adalah penjelasan singkat untuk setiap baris pada kode sumber soal tersebut:

```python
# list berikut ini berjumlah 26, artinya flag panjangnya 26 karakter
ll =[51, 54, 48, 51, 61, 57, 50, 54, 48, 52, 55, 50, 50, 57, 47, 52, 57, 47, 54, 24, 57, 58, 62]

i = raw_input()                         # variabel i berisi string input dari user
ss= 0                                   # variabel ss nilai awalnya 0
try:
    for ii in range(0,46 , 2):          # ambil setiap 2 karakter, ulangi sampai total 46 karakter
        temp = i[ii:ii+2]               # variabel temp berisi 2 karakter dari user
        temp = int(temp,0x10)           # ubah variabel temp dari hexadesimal menjadi desimal
        ss+=temp                        # jumlahkan hasilnya ke variabel ss
        temp >>=1                       # geser ke kanan 1 bit nilai variabel temp (shift right)
        if temp != ll[ii/2]:            # jika nilai variabel temp tidak sama dengan nilai pada list ll
            print "Something is wrong"  # berarti nilainya salah
    if ss !=2406:                       # jika jumlah total karakter flag tidak sama dengan 2406
            print ss/0                  # maka flag salah
    print "This flag may or may not work, can you find more ?"

except:
    print "NO"
```

Jika disederhanakan, berikut ini adalah mekanisme script di atas:

* Input dari user berupa string hexadesimal berjumlah 46 digit (karakter)
* Setiap 2 karakter akan diubah menjadi nilai desimal dan disimpan pada variabel **temp**.
* Variabel **ss** akan digunakan untuk menyimpan jumlah nilai tersebut.
* Nilai desimal pada variabel **temp** kemudian digeser ke kanan sebanyak 1 bit.
* Jika nilai pada variabel **temp** tidak sesuai dengan nilai yang terdapat pada list **ll**, maka nilai input dari user salah.
* Jika total jumlah nilai input dari user tidak sama dengan **2406**, maka inputnya salah.
* Input dari user berupa flag, hanya boleh berupa nilai hexadesimal dari karakter **_**, **0-9** dan **a-z**.

Dengan berbekal informasi tersebut, kita dapat menyimpulkan bahwa sebenarnya script tersebut cukup sederhana, yaitu menggeser ke kanan 1 bit setiap karakter dari input yang diberikan user. Selanjutnya kita akan membuat script untuk bruteforce seperti ini:

```python
#!/usr/bin/env python3

ll = [51, 54, 48, 51, 61, 57, 50, 54, 48, 52, 55, 50, 50, 57, 47, 52, 57, 47, 54, 24, 57, 58, 62]
aa = [95,48,49,50,51,52,53,54,55,56,57,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,122,123,125] # karakter a-z,0-9,_,{ dan }

for x in ll:
    for y in aa:
        if ((y >> 1) == x):
            print(chr(y), end=",")
    print()
```

Jika dijalankan, maka _script_ tersebut akan menghasilkan _output_ berikut ini:

```bash
f,g,
l,m,
a,
f,g,
z,{,
r,s,
d,e,
l,m,
a,
h,i,
n,o,
d,e,
d,e,
r,s,
_,
h,i,
r,s,
_,
l,m,
0,1,
r,s,
t,u,
},
```

Dari hasil di atas, kita mendapatkan beberapa kandidat _string_ yang kemungkinan adalah _flag_. Namun tetap masih harus kita periksa jumlah dari string tersebut nilainya harus 2406. Berikut adalah string tersebut:

```
flag{remainder_is_m0st}
flag{remainder_is_l0st}
```

Kita dapat mengujinya menggunakan python seperti ini:

```python
% python -c 'print(sum(map(lambda c: ord(c), "flag{remainder_is_m0st}")))'
2407
% python -c 'print(sum(map(lambda c: ord(c), "flag{remainder_is_l0st}")))'
2406
```

Bisa terlihat bahwa flag yang memenuhi kriteria adalah `flag{remainder_is_l0st}`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
