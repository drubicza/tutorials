## [Writeup] Cyber Talents: Can You See Me


Ini adalah writeup soal latihan [Cyber Talents](https://cybertalents.com/challenges/malware/can-you-see-me) untuk kategori reverse engineering. Berikut ini adalah petunjuk yang diberikan:

```
The correct input is the flag,format flag{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
```

Langkah pertama yang dilakukan adalah memeriksa tipe executablenya (langkah ini sifatnya opsional):

```bash
% file canyouseeme
canyouseeme: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), \
dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.24, \
BuildID[sha1]=09b983c50bdbb08163946008d369f5aa79c09db0, not stripped
```

Jika soal tersebut dijalankan tanpa parameter, maka hasilnya seperti ini:

```bash
% ./canyouseeme
wrong number of arguments ...
```

Bisa terlihat bahwa soal tersebut memerlukan parameter. Jika kita memberikan 2 parameter, maka soal tersebut akan menampilkan pesan seperti ini:

```bash
% ./canyouseeme 0 1 2
noooooo
```

Selanjutnya, kita akan melakukan disassembly terhadap fungsi `main` pada soal tersebut. Caranya seperti ini:

```bash
% r2 -AAqc 'pdf @main' canyouseeme
...
0x080484dc      890424         mov dword [esp], eax         ; argv[3]
0x080484df      e86cfeffff     call sym.imp.strlen          ; strlen(argv[3])
0x080484e4      83f816         cmp eax, 0x16                ; 22
0x080484e7      7416           je 0x80484ff                 ;
0x080484e9      c70424b38c04.  mov dword [esp], str.noooooo ; "noooooo"
...
```

Potongan disassembly di atas bertugas untuk melakukan pengecekan terhadap parameter/argumen ke-3 yang diberikan. Jika ukurannya 22 byte (0x16), maka eksekusi akan dilanjutkan ke alamat `0x80484ff`. Jika tidak, maka akan ditampilkan pesan __noooooo__. Setelah mengetahui hal tersebut, kita kembali akan mencoba menggunakan urutan karakter sehingga berjumlah 22 sebagai parameter ke-3:

```bash
% ./canyouseeme 0 1 0123456789abcdefghijkl
not gooood enough :(
```

Kali ini, pesan yang muncul adalah __not gooood enough :(__. Kembali perhatikan disassembly dari fungsi `main` pada soal tersebut:

```C
0x08048552      8b048540a004.  mov eax, dword [eax*4 + obj.xflag]
0x08048559      890424         mov dword [esp], eax             ; const char *s
0x0804855c      e8effdffff     call sym.imp.strlen              ; size_t strlen(const char *s)
0x08048561      39c3           cmp ebx, eax                     ; ebx == eax?
0x08048563      7413           je 0x8048578                     ; jika ya, lompat ke 0x8048578 -.
0x08048565      c70424bb8c04.  mov dword [esp], str.not_gooood_enough_; "not gooood enough :("  |
0x0804856c      e8bffdffff     call sym.imp.puts                ; int puts(const char *s)       |
0x08048571      b8ffffffff     mov eax, 0xffffffff              ; -1                            |
0x08048576      eb20           jmp 0x8048598                    ;                               |
0x08048578      8344241401     add dword [local_14h], 1         ; [local_14h]++ <---------------'
0x0804857d      817c24146d9a.  cmp dword [local_14h], 0x219a6d  ; [local_14h] == 0x219a6d?
0x08048585      7e87           jle 0x804850e                    ; jika <=, lompat ke 0x804850e
0x08048587      c70424d08c04.  mov dword [esp], str.Good_J0b    ; "Good J0b!!!"
0x0804858e      e89dfdffff     call sym.imp.puts                ; puts("Good J0b!!!");
```

Berikut ini adalah bagian yang penting pada potongan disassembly di atas:

```C
0x08048561      39c3           cmp ebx, eax                     ; ebx == eax?
```

Pada baris tersebut, terdapat instruksi `CMP` yang akan membandingkan isi dari register `EBX` dengan isi register `EAX`. Kita akan menggunakan _gdb_ untuk melakukan debugging terhadap soal tersebut dan memasang _breakpoint_ pada alamat `0x08048561`.

```bash
% gdb ./canyouseeme
Demangling of encoded C++/ObjC names when displaying symbols is on.
Demangling of C++/ObjC names in disassembly listings is on.
Reading symbols from ./canyouseeme...(no debugging symbols found)...done.
gdb> b *0x08048561
Breakpoint 1 at 0x8048561
```

Lanjutkan dengan menjalankan soal tersebut dengan parameter yang telah digunakan pada beberapa langkah di atas:

```bash
gdb> r 0 1 0123456789abcdefghijkl
Starting program: /tmp/canyouseeme 0 1 0123456789abcdefghijkl
Missing separate debuginfos, use: dnf debuginfo-install glibc-2.22-18.fc23.i686

Breakpoint 1, 0x08048561 in main ()
```

Eksekusi akan berhenti karena _breakpoint_. Selanjutnya, kita lihat isi dari register `EAX` dan `EBX`:

```bash
gdb> i r eax ebx
eax            0x64     100
ebx            0x38     56
```

Untuk mempermudah proses melihat isi register `EAX` dan `EBX` tersebut, kita akan menggunakan gdb script berikut ini:

```bash
b *0x08048561
r 0 1 0123456789abcdefghijkl
set $i = 0
while $i < 22
  printf "%c -> %c",$ebx,$eax
  set $ebx=$eax
  set $i = $i + 1
  c
end
q
```

Simpan script tersebut sebagai `cariflag.gdb`, lalu jalankan seperti ini:

```bash
% gdb -x cariflag.gdb ./canyouseeme
Demangling of encoded C++/ObjC names when displaying symbols is on.
Demangling of C++/ObjC names in disassembly listings is on.
Reading symbols from ./canyouseeme...(no debugging symbols found)...done.
Breakpoint 1 at 0x8048561

Breakpoint 1, 0x08048561 in main ()
8 -> d
Breakpoint 1, 0x08048561 in main ()
f -> s
Breakpoint 1, 0x08048561 in main ()
0 -> f
Breakpoint 1, 0x08048561 in main ()
7 -> d
Breakpoint 1, 0x08048561 in main ()
e -> _
Breakpoint 1, 0x08048561 in main ()
l -> }
Breakpoint 1, 0x08048561 in main ()
6 -> i
Breakpoint 1, 0x08048561 in main ()
d -> n
Breakpoint 1, 0x08048561 in main ()
k -> s
Breakpoint 1, 0x08048561 in main ()
5 -> h
Breakpoint 1, 0x08048561 in main ()
c -> i
Breakpoint 1, 0x08048561 in main ()
j -> e
Breakpoint 1, 0x08048561 in main ()
4 -> {
Breakpoint 1, 0x08048561 in main ()
b -> _
Breakpoint 1, 0x08048561 in main ()
i -> c
Breakpoint 1, 0x08048561 in main ()
3 -> g
Breakpoint 1, 0x08048561 in main ()
a -> n
Breakpoint 1, 0x08048561 in main ()
h -> a
Breakpoint 1, 0x08048561 in main ()
2 -> a
Breakpoint 1, 0x08048561 in main ()
9 -> e
Breakpoint 1, 0x08048561 in main ()
g -> p
Breakpoint 1, 0x08048561 in main ()
1 -> lGood J0b!!!
[Inferior 1 (process 12078) exited normally]
```

Bisa terlihat bahwa setiap karakter yang dimasukkan sebagai parameter ke-3 dibandingkan dengan setiap karakter pada flag. Kita tinggal mengurutkan setiap karakter tersebut seperti ini:

```bash
0 -> f
1 -> l
2 -> a
3 -> g
4 -> {
5 -> h
6 -> i
7 -> d
8 -> d
9 -> e
a -> n
b -> _
c -> i
d -> n
e -> _
f -> s
g -> p
h -> a
i -> c
j -> e
k -> s
l -> }
```

Bisa terlihat bahwa flag yang benar adalah `flag{hidden_in_spaces}`. Sekian tutorial ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
