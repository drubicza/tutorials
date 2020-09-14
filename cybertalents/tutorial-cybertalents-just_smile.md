## [Writeup] Cyber Talents: Just Smile


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: Just Smile](https://cybertalents.com/challenges/forensics/just-smile) yang merupakan soal dalam kategori _digital forensic_. Berikut ini adalah penjelasan untuk soal tersebut:

```
Can you find the flag hidden in the image?
```

Dan soalnya berupa sebuah gambar format PNG yang dapat [diunduh pada tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Forensics/smile.png). Jadi hal pertama yang perlu dilakukan adalah mengunduh file tersebut, lalu kita periksa menggunakan perintah seperti ini:

```bash
% file smile.png
smile.png: PNG image data, 480 x 480, 8-bit/color RGBA, non-interlaced
```

Ternyata file tersebut benar merupakan file gambar dengan format PNG. Selanjutnya kita periksa menggunakan _strings_ :

```bash
% strings smile.png
IHDR
bKGD
        pHYs
tIME
+iTXtComment
This_Is_Not_the_Flag_but_Useful

...

IEND
/lib64/ld-linux-x86-64.so.2
libc.so.6
__isoc99_scanf
__stack_chk_fail
printf
__cxa_finalize
strcmp
__libc_start_main
GLIBC_2.7
GLIBC_2.2.5
GLIBC_2.4
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
=y
=9
52
ABCDEFGHH
IJKLMNOPH
QRSTUVWXH
YZ{}f
This_Is_H
Not_the_H
Flag_butH
_Useful
AWAVI
AUATL
[]A\A]A^A_
Say the password:
;*3$"
GCC: (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0
```

Dari analisis _string_ yang terdapat pada file gambar tersebut, terlihat bahwa file tersebut juga berisi _executable_ ELF. Kita akan mencoba mengekstraknya menggunakan [binwalk](https://github.com/ReFirmLabs/binwalk):

```bash
% binwalk -re smile.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 480 x 480, 8-bit/color RGBA, non-interlaced
154           0x9A            Zlib compressed data, best compression
79757         0x1378D         ELF, 64-bit LSB shared object, AMD x86-64, version 1 (SYSV)
```

Namun, setelah melihat sub direktori hasil ekstraksi, tampaknya binwalk gagal mengekstrak file tersebut. Jadi kita akan mengekstrak secara manual _executable_ tersebut menggunakan informasi dari binwalk. Offset _executable_ tersebut adalah **79757** dan kita akan menggunakan **dd** untuk proses tersebut dengan perintah berikut ini:

```bash
% dd if=smile.png of=out.elf bs=1 skip=79757
8448+0 records in
8448+0 records out
8448 bytes (8.4 kB, 8.2 KiB) copied, 0.0405314 s, 208 kB/s
```

Coba kita periksa apakah hasil ekstraknya berhasil:

```bash
% file out.elf
out.elf: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=b69c1cf0ad3c9bc837a7e23139f321fc81238045, not stripped
```

Ternyata hasilnya benar. Selanjutnya kita akan menggunakan **gdb** untuk melakukan debugging terhadap _executable_ tersebut:

```bash
% gdb ./out.elf
Demangling of encoded C++/ObjC names when displaying symbols is on.
Demangling of C++/ObjC names in disassembly listings is on.
Reading symbols from ./out.elf...
(No debugging symbols found in ./out.elf)
```

Masih pada _prompt_ **gdb**, kita akan melakukan _disassembly_ terhadap fungsi **main** dengan perintah seperti ini:

```bash
gdb> disass main
Dump of assembler code for function main:
   0x000000000000076a <+0>:     push   rbp
   0x000000000000076b <+1>:     mov    rbp,rsp
   0x000000000000076e <+4>:     sub    rsp,0x90
   0x0000000000000775 <+11>:    mov    rax,QWORD PTR fs:0x28
   0x000000000000077e <+20>:    mov    QWORD PTR [rbp-0x8],rax
   0x0000000000000782 <+24>:    xor    eax,eax
   0x0000000000000784 <+26>:    movabs rax,0x4847464544434241
   0x000000000000078e <+36>:    movabs rdx,0x504f4e4d4c4b4a49
   0x0000000000000798 <+46>:    mov    QWORD PTR [rbp-0x90],rax
   0x000000000000079f <+53>:    mov    QWORD PTR [rbp-0x88],rdx
   0x00000000000007a6 <+60>:    movabs rax,0x5857565554535251
   0x00000000000007b0 <+70>:    mov    QWORD PTR [rbp-0x80],rax
   0x00000000000007b4 <+74>:    mov    DWORD PTR [rbp-0x78],0x7d7b5a59
   0x00000000000007bb <+81>:    mov    WORD PTR [rbp-0x74],0x215f
   0x00000000000007c1 <+87>:    mov    BYTE PTR [rbp-0x72],0x0
   0x00000000000007c5 <+91>:    lea    rdi,[rip+0x248]        # 0xa14
   0x00000000000007cc <+98>:    mov    eax,0x0
   0x00000000000007d1 <+103>:   call   0x620 <printf@plt>
   0x00000000000007d6 <+108>:   movabs rax,0x5f73495f73696854
   0x00000000000007e0 <+118>:   movabs rdx,0x5f6568745f746f4e
   0x00000000000007ea <+128>:   mov    QWORD PTR [rbp-0x70],rax
   0x00000000000007ee <+132>:   mov    QWORD PTR [rbp-0x68],rdx
   0x00000000000007f2 <+136>:   movabs rax,0x7475625f67616c46
   0x00000000000007fc <+146>:   movabs rdx,0x6c75666573555f
   0x0000000000000806 <+156>:   mov    QWORD PTR [rbp-0x60],rax
   0x000000000000080a <+160>:   mov    QWORD PTR [rbp-0x58],rdx
   0x000000000000080e <+164>:   movzx  eax,BYTE PTR [rbp-0x8b]
   0x0000000000000815 <+171>:   mov    BYTE PTR [rbp-0x50],al
   0x0000000000000818 <+174>:   movzx  eax,BYTE PTR [rbp-0x85]
   0x000000000000081f <+181>:   mov    BYTE PTR [rbp-0x4f],al
   0x0000000000000822 <+184>:   movzx  eax,BYTE PTR [rbp-0x90]
   0x0000000000000829 <+191>:   mov    BYTE PTR [rbp-0x4e],al
   0x000000000000082c <+194>:   movzx  eax,BYTE PTR [rbp-0x8a]
   0x0000000000000833 <+201>:   mov    BYTE PTR [rbp-0x4d],al
   0x0000000000000836 <+204>:   movzx  eax,BYTE PTR [rbp-0x76]
   0x000000000000083a <+208>:   mov    BYTE PTR [rbp-0x4c],al
   0x000000000000083d <+211>:   movzx  eax,BYTE PTR [rbp-0x90]
   0x0000000000000844 <+218>:   mov    BYTE PTR [rbp-0x4b],al
   0x0000000000000847 <+221>:   movzx  eax,BYTE PTR [rbp-0x81]
   0x000000000000084e <+228>:   mov    BYTE PTR [rbp-0x4a],al
   0x0000000000000851 <+231>:   movzx  eax,BYTE PTR [rbp-0x81]
   0x0000000000000858 <+238>:   mov    BYTE PTR [rbp-0x49],al
   0x000000000000085b <+241>:   movzx  eax,BYTE PTR [rbp-0x8c]
   0x0000000000000862 <+248>:   mov    BYTE PTR [rbp-0x48],al
   0x0000000000000865 <+251>:   movzx  eax,BYTE PTR [rbp-0x83]
   0x000000000000086c <+258>:   mov    BYTE PTR [rbp-0x47],al
   0x000000000000086f <+261>:   movzx  eax,BYTE PTR [rbp-0x8c]
   0x0000000000000876 <+268>:   mov    BYTE PTR [rbp-0x46],al
   0x0000000000000879 <+271>:   movzx  eax,BYTE PTR [rbp-0x8d]
   0x0000000000000880 <+278>:   mov    BYTE PTR [rbp-0x45],al
   0x0000000000000883 <+281>:   movzx  eax,BYTE PTR [rbp-0x88]
   0x000000000000088a <+288>:   mov    BYTE PTR [rbp-0x44],al
   0x000000000000088d <+291>:   movzx  eax,BYTE PTR [rbp-0x83]
   0x0000000000000894 <+298>:   mov    BYTE PTR [rbp-0x43],al
   0x0000000000000897 <+301>:   movzx  eax,BYTE PTR [rbp-0x8a]
   0x000000000000089e <+308>:   mov    BYTE PTR [rbp-0x42],al
   0x00000000000008a1 <+311>:   movzx  eax,BYTE PTR [rbp-0x74]
   0x00000000000008a5 <+315>:   mov    BYTE PTR [rbp-0x41],al
   0x00000000000008a8 <+318>:   movzx  eax,BYTE PTR [rbp-0x8b]
   0x00000000000008af <+325>:   mov    BYTE PTR [rbp-0x40],al
   0x00000000000008b2 <+328>:   movzx  eax,BYTE PTR [rbp-0x88]
   0x00000000000008b9 <+335>:   mov    BYTE PTR [rbp-0x3f],al
   0x00000000000008bc <+338>:   movzx  eax,BYTE PTR [rbp-0x85]
   0x00000000000008c3 <+345>:   mov    BYTE PTR [rbp-0x3e],al
   0x00000000000008c6 <+348>:   movzx  eax,BYTE PTR [rbp-0x8c]
   0x00000000000008cd <+355>:   mov    BYTE PTR [rbp-0x3d],al
   0x00000000000008d0 <+358>:   movzx  eax,BYTE PTR [rbp-0x7e]
   0x00000000000008d4 <+362>:   mov    BYTE PTR [rbp-0x3c],al
   0x00000000000008d7 <+365>:   movzx  eax,BYTE PTR [rbp-0x74]
   0x00000000000008db <+369>:   mov    BYTE PTR [rbp-0x3b],al
   0x00000000000008de <+372>:   movzx  eax,BYTE PTR [rbp-0x7f]
   0x00000000000008e2 <+376>:   mov    BYTE PTR [rbp-0x3a],al
   0x00000000000008e5 <+379>:   movzx  eax,BYTE PTR [rbp-0x8c]
   0x00000000000008ec <+386>:   mov    BYTE PTR [rbp-0x39],al
   0x00000000000008ef <+389>:   movzx  eax,BYTE PTR [rbp-0x90]
   0x00000000000008f6 <+396>:   mov    BYTE PTR [rbp-0x38],al
   0x00000000000008f9 <+399>:   movzx  eax,BYTE PTR [rbp-0x85]
   0x0000000000000900 <+406>:   mov    BYTE PTR [rbp-0x37],al
   0x0000000000000903 <+409>:   movzx  eax,BYTE PTR [rbp-0x85]
   0x000000000000090a <+416>:   mov    BYTE PTR [rbp-0x36],al
   0x000000000000090d <+419>:   movzx  eax,BYTE PTR [rbp-0x78]
   0x0000000000000911 <+423>:   mov    BYTE PTR [rbp-0x35],al
   0x0000000000000914 <+426>:   movzx  eax,BYTE PTR [rbp-0x73]
   0x0000000000000918 <+430>:   mov    BYTE PTR [rbp-0x34],al
   0x000000000000091b <+433>:   movzx  eax,BYTE PTR [rbp-0x73]
   0x000000000000091f <+437>:   mov    BYTE PTR [rbp-0x33],al
   0x0000000000000922 <+440>:   movzx  eax,BYTE PTR [rbp-0x75]
   0x0000000000000926 <+444>:   mov    BYTE PTR [rbp-0x32],al
   0x0000000000000929 <+447>:   mov    BYTE PTR [rbp-0x31],0x0
   0x000000000000092d <+451>:   lea    rax,[rbp-0x30]
   0x0000000000000931 <+455>:   mov    rsi,rax
   0x0000000000000934 <+458>:   lea    rdi,[rip+0xeb]        # 0xa26
   0x000000000000093b <+465>:   mov    eax,0x0
   0x0000000000000940 <+470>:   call   0x640 <__isoc99_scanf@plt>
   0x0000000000000945 <+475>:   lea    rdx,[rbp-0x30]
   0x0000000000000949 <+479>:   lea    rax,[rbp-0x70]
   0x000000000000094d <+483>:   mov    rsi,rdx
   0x0000000000000950 <+486>:   mov    rdi,rax
   0x0000000000000953 <+489>:   call   0x630 <strcmp@plt>
   0x0000000000000958 <+494>:   test   eax,eax
   0x000000000000095a <+496>:   jne    0x974 <main+522>
   0x000000000000095c <+498>:   lea    rax,[rbp-0x50]
   0x0000000000000960 <+502>:   mov    rsi,rax
   0x0000000000000963 <+505>:   lea    rdi,[rip+0xbc]        # 0xa26
   0x000000000000096a <+512>:   mov    eax,0x0
   0x000000000000096f <+517>:   call   0x620 <printf@plt>
   0x0000000000000974 <+522>:   mov    eax,0x0
   0x0000000000000979 <+527>:   mov    rcx,QWORD PTR [rbp-0x8]
   0x000000000000097d <+531>:   xor    rcx,QWORD PTR fs:0x28
   0x0000000000000986 <+540>:   je     0x98d <main+547>
   0x0000000000000988 <+542>:   call   0x610 <__stack_chk_fail@plt>
   0x000000000000098d <+547>:   leave
   0x000000000000098e <+548>:   ret
End of assembler dump.
```

Perhatikan bahwa pada _disassembly_ di atas terdapat pemanggilan terhadap fungsi **strcmp** yang digunakan untuk membandingkan 2 buah _string_ yang terdapat pada _register_ **RSI** dan **RDI**. Berikut ini adalah instruksi yang dimaksud:

```c
0x0000000000000953 <+489>:   call   0x630 <strcmp@plt>
```

Kita akan memasang _breakpoint_ pada alamat tersebut, lalu menjalankan _executable_ dan melihat _string_ yang dibandingkan:

```c
gdb> b *main+489
Breakpoint 1 at 0x953
gdb> r
Starting program: /tmp/out.elf
Missing separate debuginfos, use: dnf debuginfo-install glibc-2.31-4.fc32.x86_64
Say the password:whatever

Breakpoint 1, 0x0000555555554953 in main ()
=> 0x0000555555554953 <main+489>:       e8 d8 fc ff ff  call   0x555555554630 <strcmp@plt>
```

Bisa terlihat bahwa setelah kita memasukkan password (whatever), maka **gdb** akan berhenti pada _breakpoint_ yang telah kita pasang. Selanjutnya, kita akan memeriksa isi dari _register_ **RSI** dan **RDI**:

```c
gdb> x/s $rsi
0x7fffffffdb40: "whatever"
gdb> x/s $rdi
0x7fffffffdb00: "This_Is_Not_the_Flag_but_Useful"
```

Bisa terlihat bahwa password yang benar adalah **This_Is_Not_the_Flag_but_Useful**. Selanjutnya kita akan menonaktifkan _breakpoint_ yang telah kita pasang, dan mengulangi eksekusi program dan memasukkan _password_ yang benar.

```c
gdb> i b
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000555555554953 <main+489>
        breakpoint already hit 1 time
gdb> disable 1
gdb> r
Starting program: /tmp/out.elf
Say the password:This_Is_Not_the_Flag_but_Useful
FLAG{APPENEDING_FILES_REALLY!!}[Inferior 1 (process 6251) exited normally]
```

Bisa terlihat, setelah kita memasukkan _password_ yang benar, maka flagnya akan ditampilkan oleh program tersebut. Flag untuk soal ini adalah `FLAG{APPENEDING_FILES_REALLY!!}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
