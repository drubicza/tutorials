## [Writeup] Cyber Talents: Deleted


Tutorial kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Deleted](https://cybertalents.com/challenges/forensics/deleted) yang masuk dalam kategori digital forensic. Berikut ini adalah petunjuk pada soal tersebut:

```
hi john i need your assistance to recover my drive
```

Selain petunjuk tersebut, kita diminta untuk mengunduh file yang terdapat pada [tautan ini](https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/deleted.E01). Setelah selesai mengunduh, kita lanjutkan dengan mencari tahu format file tersebut dengan perintah seperti ini:

```bash
% file deleted.E01
deleted.E01: EWF/Expert Witness/EnCase image file format
```

Ternyata formatnya adalah **EWF**, dan berdasarkan petunjuk yang terdapat pada situs [andreafortuna](https://www.andreafortuna.org/2018/04/11/how-to-mount-an-ewf-image-file-e01-on-linux/), kita dapat menggunakan **ewftools** untuk melakukan forensik terhadap file tersebut. Jika menggunakan sistem operasi Linux distro Fedora, kita dapat melakukan instalasi **ewftools** dengan perintah:

```bash
% sudo dnf install ewftools
```

Setelah proses instalasi selesai, kita cukup mengikuti petunjuk pada situs [andreafortuna](https://www.andreafortuna.org/2018/04/11/how-to-mount-an-ewf-image-file-e01-on-linux/) tersebut. Jalankan perintah berikut ini sebagai root:

* Buat direktori untuk image dan mountpoint:

```bash
# mkdir {rawimage,mountpoint}

```

* Mount file **deleted.E01** menggunakan **ewfmount**:

```bash
# ewfmount deleted.E01 rawimage/
ewfmount 20140608
```

* Mount image **ewf1** pada subdirektori **rawimage**:

```bash
# mount rawimage/ewf1 mountpoint/ -o ro,loop,show_sys_files,streams_interface=windows
```

* Lihat isi dari sub direktori **mountpoint**:

```bash
# ls -l mountpoint/
total 2217
-rwxrwxrwx. 1 root root    2560 Mar 21 21:00 '$AttrDef'
-rwxrwxrwx. 1 root root       0 Mar 21 21:00 '$BadClus'
-rwxrwxrwx. 1 root root   26104 Mar 21 21:00 '$Bitmap'
-rwxrwxrwx. 1 root root    8192 Mar 21 21:00 '$Boot'
drwxrwxrwx. 1 root root       0 Mar 21 21:00 '$Extend'
-rwxrwxrwx. 1 root root 2097152 Mar 21 21:00 '$LogFile'
-rwxrwxrwx. 1 root root    4096 Mar 21 21:00 '$MFTMirr'
----------. 1 root root       0 Mar 21 21:00 '$Secure'
-rwxrwxrwx. 1 root root  131072 Mar 21 21:00 '$UpCase'
-rwxrwxrwx. 1 root root       0 Mar 21 21:00 '$Volume'
-rwxrwxrwx. 1 root root      10 Mar 21 21:03  holland.txt
-rwxrwxrwx. 1 root root       0 Mar 21 21:02  nano.txt
-rwxrwxrwx. 1 root root       0 Mar 21 21:02  peta.doc
drwxrwxrwx. 1 root root       0 Mar 21 21:05  RECYCLER
drwxrwxrwx. 1 root root       0 Mar 21 21:02 'System Volume Information'
```

* Terdapat file **holland.txt**, coba kita lihat isinya:

```bash
# cat mountpoint/holland.txt
not here
```

* Ternyata bukan flag. Kita lanjutkan dengan melihat isi dari _Recycle Bin_:

```bash
# ls -l mountpoint/RECYCLER/S-1-5-21-789336058-1500820517-682003330-1003/
total 2
drwxrwxrwx. 1 root root   0 Mar 21 21:04 De1
-rwxrwxrwx. 1 root root  65 Mar 21 21:05 desktop.ini
-rwxrwxrwx. 1 root root 820 Mar 21 21:05 INFO2
```

* Terdapat file **INFO2**, coba kita periksa isinya:

```bash
# cat mountpoint/RECYCLER/S-1-5-21-789336058-1500820517-682003330-1003/INFO2
 E:\My BR0%aE:\My BR
```

* Ternyata bukan flag juga. Kita periksa sub direktori **Del**:

```bash
# ls -l mountpoint/RECYCLER/S-1-5-21-789336058-1500820517-682003330-1003/De1/
total 2
-rwxrwxrwx. 2 root root 254 Mar 21 21:04 'Briefcase Database'
-rwxrwxrwx. 1 root root  82 Mar 21 21:02  desktop.ini
-rwxrwxrwx. 1 root root  25 Mar 21 21:04  flag.txt
```

* Ternyata terdapat file **flag.txt**, coba kita periksa isinya:

```bash
# cat mountpoint/RECYCLER/S-1-5-21-789336058-1500820517-682003330-1003/De1/flag.txt
flag{d3l3t3dbuty0ukn0wit}
```

Ternyata flagnya adalah `flag{d3l3t3dbuty0ukn0wit}`. Selanjutnya, setelah menemukan flagnya, kita perlu melakukan unmount menggunakan perintah berikut ini:

```bash
# umount mountpoint/
# umount rawimage/
# rm -rf mountpoint/ rawimage/
```

Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
