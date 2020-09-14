## [Writeup] Cyber Talents: Elementary


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: Elementary](https://cybertalents.com/challenges/malware/elementary). Soal tersebut masuk ke dalam kategori _reverse engineering_. Berikut ini adalah penjelasan untuk soal tersebut:

```
Here we've prepared a simple program, crack me if you can.
```

Dan kita akan diberikan soal berupa sebuah file executable yang dapat [diunduh pada tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Reverse/elementary). Setelah, mengunduh soal tersebut, kita akan memeriksa tipenya:

```bash
% file elementary
elementary: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=d39e86cbb1ab3d21df90dda89aa7c1b27465d613, stripped
```

Cukup aneh, karena executable tersebut tipenya adalah _shared object_. Dari sini kita bisa menarik kesimpulan bahwa aplikasi ini tidak akan menggunakan alamat eksekusi yang normal. Selanjutnya, kita disassemble executable tersebut menggunakan objdump:

```bash
% objdump -d -M intel -j .text elementary

elementary:     file format elf64-x86-64


Disassembly of section .text:

0000000000000690 <.text>:
 690:   31 ed                   xor    ebp,ebp
 692:   49 89 d1                mov    r9,rdx
 695:   5e                      pop    rsi
 696:   48 89 e2                mov    rdx,rsp
 699:   48 83 e4 f0             and    rsp,0xfffffffffffffff0
 69d:   50                      push   rax
 69e:   54                      push   rsp
 69f:   4c 8d 05 ba 02 00 00    lea    r8,[rip+0x2ba]        # 960 <__cxa_finalize@plt+0x2e0>
 6a6:   48 8d 0d 43 02 00 00    lea    rcx,[rip+0x243]        # 8f0 <__cxa_finalize@plt+0x270>
 6ad:   48 8d 3d 70 01 00 00    lea    rdi,[rip+0x170]        # 824 <__cxa_finalize@plt+0x1a4>
 6b4:   ff 15 26 09 20 00       call   QWORD PTR [rip+0x200926]        # 200fe0 <__cxa_finalize@plt+0x200960>
 6ba:   f4                      hlt
 6bb:   0f 1f 44 00 00          nop    DWORD PTR [rax+rax*1+0x0]
 6c0:   48 8d 3d 51 09 20 00    lea    rdi,[rip+0x200951]        # 201018 <__cxa_finalize@plt+0x200998>
 6c7:   55                      push   rbp
 6c8:   48 8d 05 49 09 20 00    lea    rax,[rip+0x200949]        # 201018 <__cxa_finalize@plt+0x200998>
 6cf:   48 39 f8                cmp    rax,rdi
 6d2:   48 89 e5                mov    rbp,rsp
 6d5:   74 19                   je     6f0 <__cxa_finalize@plt+0x70>
 6d7:   48 8b 05 fa 08 20 00    mov    rax,QWORD PTR [rip+0x2008fa]        # 200fd8 <__cxa_finalize@plt+0x200958>
 6de:   48 85 c0                test   rax,rax
 6e1:   74 0d                   je     6f0 <__cxa_finalize@plt+0x70>
 6e3:   5d                      pop    rbp
 6e4:   ff e0                   jmp    rax
 6e6:   66 2e 0f 1f 84 00 00    nop    WORD PTR cs:[rax+rax*1+0x0]
 6ed:   00 00 00
 6f0:   5d                      pop    rbp
 6f1:   c3                      ret
 6f2:   0f 1f 40 00             nop    DWORD PTR [rax+0x0]
 6f6:   66 2e 0f 1f 84 00 00    nop    WORD PTR cs:[rax+rax*1+0x0]
 6fd:   00 00 00
 700:   48 8d 3d 11 09 20 00    lea    rdi,[rip+0x200911]        # 201018 <__cxa_finalize@plt+0x200998>
 707:   48 8d 35 0a 09 20 00    lea    rsi,[rip+0x20090a]        # 201018 <__cxa_finalize@plt+0x200998>
 70e:   55                      push   rbp
 70f:   48 29 fe                sub    rsi,rdi
 712:   48 89 e5                mov    rbp,rsp
 715:   48 c1 fe 03             sar    rsi,0x3
 719:   48 89 f0                mov    rax,rsi
 71c:   48 c1 e8 3f             shr    rax,0x3f
 720:   48 01 c6                add    rsi,rax
 723:   48 d1 fe                sar    rsi,1
 726:   74 18                   je     740 <__cxa_finalize@plt+0xc0>
 728:   48 8b 05 c1 08 20 00    mov    rax,QWORD PTR [rip+0x2008c1]        # 200ff0 <__cxa_finalize@plt+0x200970>
 72f:   48 85 c0                test   rax,rax
 732:   74 0c                   je     740 <__cxa_finalize@plt+0xc0>
 734:   5d                      pop    rbp
 735:   ff e0                   jmp    rax
 737:   66 0f 1f 84 00 00 00    nop    WORD PTR [rax+rax*1+0x0]
 73e:   00 00
 740:   5d                      pop    rbp
 741:   c3                      ret
 742:   0f 1f 40 00             nop    DWORD PTR [rax+0x0]
 746:   66 2e 0f 1f 84 00 00    nop    WORD PTR cs:[rax+rax*1+0x0]
 74d:   00 00 00
 750:   80 3d c1 08 20 00 00    cmp    BYTE PTR [rip+0x2008c1],0x0        # 201018 <__cxa_finalize@plt+0x200998>
 757:   75 2f                   jne    788 <__cxa_finalize@plt+0x108>
 759:   48 83 3d 97 08 20 00    cmp    QWORD PTR [rip+0x200897],0x0        # 200ff8 <__cxa_finalize@plt+0x200978>
 760:   00
 761:   55                      push   rbp
 762:   48 89 e5                mov    rbp,rsp
 765:   74 0c                   je     773 <__cxa_finalize@plt+0xf3>
 767:   48 8b 3d 9a 08 20 00    mov    rdi,QWORD PTR [rip+0x20089a]        # 201008 <__cxa_finalize@plt+0x200988>
 76e:   e8 0d ff ff ff          call   680 <__cxa_finalize@plt>
 773:   e8 48 ff ff ff          call   6c0 <__cxa_finalize@plt+0x40>
 778:   c6 05 99 08 20 00 01    mov    BYTE PTR [rip+0x200899],0x1        # 201018 <__cxa_finalize@plt+0x200998>
 77f:   5d                      pop    rbp
 780:   c3                      ret
 781:   0f 1f 80 00 00 00 00    nop    DWORD PTR [rax+0x0]
 788:   f3 c3                   repz ret
 78a:   66 0f 1f 44 00 00       nop    WORD PTR [rax+rax*1+0x0]
 790:   55                      push   rbp
 791:   48 89 e5                mov    rbp,rsp
 794:   5d                      pop    rbp
 795:   e9 66 ff ff ff          jmp    700 <__cxa_finalize@plt+0x80>
 79a:   55                      push   rbp
 79b:   48 89 e5                mov    rbp,rsp
 79e:   48 83 ec 30             sub    rsp,0x30
 7a2:   48 89 7d d8             mov    QWORD PTR [rbp-0x28],rdi
 7a6:   48 89 75 d0             mov    QWORD PTR [rbp-0x30],rsi
 7aa:   64 48 8b 04 25 28 00    mov    rax,QWORD PTR fs:0x28
 7b1:   00 00
 7b3:   48 89 45 f8             mov    QWORD PTR [rbp-0x8],rax
 7b7:   31 c0                   xor    eax,eax
 7b9:   50                      push   rax
 7ba:   31 c0                   xor    eax,eax
 7bc:   74 04                   je     7c2 <__cxa_finalize@plt+0x142>
 7be:   48 83 c4 04             add    rsp,0x4
 7c2:   58                      pop    rax
 7c3:   c6 45 f7 00             mov    BYTE PTR [rbp-0x9],0x0
 7c7:   48 8b 15 42 08 20 00    mov    rdx,QWORD PTR [rip+0x200842]        # 201010 <__cxa_finalize@plt+0x200990>
 7ce:   48 8b 45 d0             mov    rax,QWORD PTR [rbp-0x30]
 7d2:   48 89 d6                mov    rsi,rdx
 7d5:   48 89 c7                mov    rdi,rax
 7d8:   e8 83 fe ff ff          call   660 <strcmp@plt>
 7dd:   85 c0                   test   eax,eax
 7df:   75 07                   jne    7e8 <__cxa_finalize@plt+0x168>
 7e1:   b8 01 00 00 00          mov    eax,0x1
 7e6:   eb 05                   jmp    7ed <__cxa_finalize@plt+0x16d>
 7e8:   b8 00 00 00 00          mov    eax,0x0
 7ed:   48 8b 4d f8             mov    rcx,QWORD PTR [rbp-0x8]
 7f1:   64 48 33 0c 25 28 00    xor    rcx,QWORD PTR fs:0x28
 7f8:   00 00
 7fa:   74 05                   je     801 <__cxa_finalize@plt+0x181>
 7fc:   e8 3f fe ff ff          call   640 <__stack_chk_fail@plt>
 801:   c9                      leave
 802:   c3                      ret
 803:   55                      push   rbp
 804:   48 89 e5                mov    rbp,rsp
 807:   48 8d 3d 6f 01 00 00    lea    rdi,[rip+0x16f]        # 97d <__cxa_finalize@plt+0x2fd>
 80e:   e8 1d fe ff ff          call   630 <puts@plt>
 813:   90                      nop
 814:   5d                      pop    rbp
 815:   c3                      ret
 816:   55                      push   rbp
 817:   48 89 e5                mov    rbp,rsp
 81a:   bf 01 00 00 00          mov    edi,0x1
 81f:   e8 4c fe ff ff          call   670 <exit@plt>
 824:   55                      push   rbp
 825:   48 89 e5                mov    rbp,rsp
 828:   48 83 ec 30             sub    rsp,0x30
 82c:   89 7d dc                mov    DWORD PTR [rbp-0x24],edi
 82f:   48 89 75 d0             mov    QWORD PTR [rbp-0x30],rsi
 833:   64 48 8b 04 25 28 00    mov    rax,QWORD PTR fs:0x28
 83a:   00 00
 83c:   48 89 45 f8             mov    QWORD PTR [rbp-0x8],rax
 840:   31 c0                   xor    eax,eax
 842:   50                      push   rax
 843:   31 c0                   xor    eax,eax
 845:   74 04                   je     84b <__cxa_finalize@plt+0x1cb>
 847:   48 83 c4 04             add    rsp,0x4
 84b:   58                      pop    rax
 84c:   c6 45 ee 00             mov    BYTE PTR [rbp-0x12],0x0
 850:   c6 45 f7 00             mov    BYTE PTR [rbp-0x9],0x0
 854:   48 8d 3d 28 01 00 00    lea    rdi,[rip+0x128]        # 983 <__cxa_finalize@plt+0x303>
 85b:   e8 d0 fd ff ff          call   630 <puts@plt>
 860:   48 8d 45 e6             lea    rax,[rbp-0x1a]
 864:   ba 08 00 00 00          mov    edx,0x8
 869:   48 89 c6                mov    rsi,rax
 86c:   bf 00 00 00 00          mov    edi,0x0
 871:   e8 da fd ff ff          call   650 <read@plt>
 876:   48 8d 3d 11 01 00 00    lea    rdi,[rip+0x111]        # 98e <__cxa_finalize@plt+0x30e>
 87d:   e8 ae fd ff ff          call   630 <puts@plt>
 882:   48 8d 45 ef             lea    rax,[rbp-0x11]
 886:   ba 08 00 00 00          mov    edx,0x8
 88b:   48 89 c6                mov    rsi,rax
 88e:   bf 00 00 00 00          mov    edi,0x0
 893:   e8 b8 fd ff ff          call   650 <read@plt>
 898:   48 8d 55 ef             lea    rdx,[rbp-0x11]
 89c:   48 8d 45 e6             lea    rax,[rbp-0x1a]
 8a0:   48 89 d6                mov    rsi,rdx
 8a3:   48 89 c7                mov    rdi,rax
 8a6:   e8 ef fe ff ff          call   79a <__cxa_finalize@plt+0x11a>
 8ab:   89 45 e0                mov    DWORD PTR [rbp-0x20],eax
 8ae:   83 7d e0 00             cmp    DWORD PTR [rbp-0x20],0x0
 8b2:   74 0c                   je     8c0 <__cxa_finalize@plt+0x240>
 8b4:   b8 00 00 00 00          mov    eax,0x0
 8b9:   e8 45 ff ff ff          call   803 <__cxa_finalize@plt+0x183>
 8be:   eb 0a                   jmp    8ca <__cxa_finalize@plt+0x24a>
 8c0:   b8 00 00 00 00          mov    eax,0x0
 8c5:   e8 4c ff ff ff          call   816 <__cxa_finalize@plt+0x196>
 8ca:   b8 00 00 00 00          mov    eax,0x0
 8cf:   48 8b 4d f8             mov    rcx,QWORD PTR [rbp-0x8]
 8d3:   64 48 33 0c 25 28 00    xor    rcx,QWORD PTR fs:0x28
 8da:   00 00
 8dc:   74 05                   je     8e3 <__cxa_finalize@plt+0x263>
 8de:   e8 5d fd ff ff          call   640 <__stack_chk_fail@plt>
 8e3:   c9                      leave
 8e4:   c3                      ret
 8e5:   66 2e 0f 1f 84 00 00    nop    WORD PTR cs:[rax+rax*1+0x0]
 8ec:   00 00 00
 8ef:   90                      nop
 8f0:   41 57                   push   r15
 8f2:   41 56                   push   r14
 8f4:   49 89 d7                mov    r15,rdx
 8f7:   41 55                   push   r13
 8f9:   41 54                   push   r12
 8fb:   4c 8d 25 96 04 20 00    lea    r12,[rip+0x200496]        # 200d98 <__cxa_finalize@plt+0x200718>
 902:   55                      push   rbp
 903:   48 8d 2d 96 04 20 00    lea    rbp,[rip+0x200496]        # 200da0 <__cxa_finalize@plt+0x200720>
 90a:   53                      push   rbx
 90b:   41 89 fd                mov    r13d,edi
 90e:   49 89 f6                mov    r14,rsi
 911:   4c 29 e5                sub    rbp,r12
 914:   48 83 ec 08             sub    rsp,0x8
 918:   48 c1 fd 03             sar    rbp,0x3
 91c:   e8 e7 fc ff ff          call   608 <puts@plt-0x28>
 921:   48 85 ed                test   rbp,rbp
 924:   74 20                   je     946 <__cxa_finalize@plt+0x2c6>
 926:   31 db                   xor    ebx,ebx
 928:   0f 1f 84 00 00 00 00    nop    DWORD PTR [rax+rax*1+0x0]
 92f:   00
 930:   4c 89 fa                mov    rdx,r15
 933:   4c 89 f6                mov    rsi,r14
 936:   44 89 ef                mov    edi,r13d
 939:   41 ff 14 dc             call   QWORD PTR [r12+rbx*8]
 93d:   48 83 c3 01             add    rbx,0x1
 941:   48 39 dd                cmp    rbp,rbx
 944:   75 ea                   jne    930 <__cxa_finalize@plt+0x2b0>
 946:   48 83 c4 08             add    rsp,0x8
 94a:   5b                      pop    rbx
 94b:   5d                      pop    rbp
 94c:   41 5c                   pop    r12
 94e:   41 5d                   pop    r13
 950:   41 5e                   pop    r14
 952:   41 5f                   pop    r15
 954:   c3                      ret
 955:   90                      nop
 956:   66 2e 0f 1f 84 00 00    nop    WORD PTR cs:[rax+rax*1+0x0]
 95d:   00 00 00
 960:   f3 c3                   repz ret
```

Perhatikan baris ini:

```asm
 7ce:   48 8b 45 d0             mov    rax,QWORD PTR [rbp-0x30]
 7d2:   48 89 d6                mov    rsi,rdx
 7d5:   48 89 c7                mov    rdi,rax
 7d8:   e8 83 fe ff ff          call   660 <strcmp@plt>
```

Potongan kode di atas berisi pemanggilan fungsi `strcmp` yaitu fungsi yang terdapat pada Libc dan digunakan untuk membandingkan 2 buah _string_. Kita akan memasang _breakpoint_ pada pemanggilan fungsi tersebut. Masalahnya, kita tidak mengetahui alamat dimana fungsi tersebut dipanggil. Kita akan menggunakan _gdb_ untuk melakukkan debugging. Jalankan _gdb_ dengan parameter berupa nama aplikasi tersebut:

```bash
% gdb ./elementary
Demangling of encoded C++/ObjC names when displaying symbols is on.
Demangling of C++/ObjC names in disassembly listings is on.
Reading symbols from ./elementary...
(No debugging symbols found in ./elementary)
gdb>
```

Pada prompt _gdb_ gunakan perintah `info file` atau singkatannya `i file` untuk mengetahui informasi mengenai file elementary tersebut:

```bash
gdb> i file
Symbols from "/tmp/elementary".
Local exec file:
        `/tmp/elementary', file type elf64-x86-64.
        Entry point: 0x690
        0x0000000000000238 - 0x0000000000000254 is .interp
        0x0000000000000254 - 0x0000000000000274 is .note.ABI-tag
        0x0000000000000274 - 0x0000000000000298 is .note.gnu.build-id
        0x0000000000000298 - 0x00000000000002b4 is .gnu.hash
        0x00000000000002b8 - 0x00000000000003c0 is .dynsym
        0x00000000000003c0 - 0x000000000000046e is .dynstr
        0x000000000000046e - 0x0000000000000484 is .gnu.version
        0x0000000000000488 - 0x00000000000004b8 is .gnu.version_r
        0x00000000000004b8 - 0x0000000000000590 is .rela.dyn
        0x0000000000000590 - 0x0000000000000608 is .rela.plt
        0x0000000000000608 - 0x000000000000061f is .init
        0x0000000000000620 - 0x0000000000000680 is .plt
        0x0000000000000680 - 0x0000000000000688 is .plt.got
        0x0000000000000690 - 0x0000000000000962 is .text
        0x0000000000000964 - 0x000000000000096d is .fini
        0x0000000000000970 - 0x0000000000000999 is .rodata
        0x000000000000099c - 0x00000000000009f0 is .eh_frame_hdr
        0x00000000000009f0 - 0x0000000000000b58 is .eh_frame
        0x0000000000200d98 - 0x0000000000200da0 is .init_array
        0x0000000000200da0 - 0x0000000000200da8 is .fini_array
        0x0000000000200da8 - 0x0000000000200f98 is .dynamic
        0x0000000000200f98 - 0x0000000000201000 is .got
        0x0000000000201000 - 0x0000000000201018 is .data
        0x0000000000201018 - 0x0000000000201020 is .bss
```

Setelah mengetahui _entry point_ soal tersebut, kita akan memasang _breakpoint_ pada alamat tersebut:

```bash
gdb> b *0x690
Breakpoint 1 at 0x690
```

Jalankan soal tersebut dengan perintah `run` atau `r`:

```bash
gdb> r
Starting program: /tmp/elementary
Warning:
Cannot insert breakpoint 1.
Cannot access memory at address 0x690
```

Ternyata _breakpoint_ yang kita pasang tidak dapat diakses. Selanjutnya kita lihat kembali informasi file soal tersebut:

```bash
gdb> i file
Symbols from "/tmp/elementary".
Native process:
        Using the running image of child process 12571.
        While running this, GDB does not access memory from...
Local exec file:
        `/tmp/elementary', file type elf64-x86-64.
        Entry point: 0x555555554690
        0x0000555555554238 - 0x0000555555554254 is .interp
        0x0000555555554254 - 0x0000555555554274 is .note.ABI-tag
        0x0000555555554274 - 0x0000555555554298 is .note.gnu.build-id
        0x0000555555554298 - 0x00005555555542b4 is .gnu.hash
        0x00005555555542b8 - 0x00005555555543c0 is .dynsym
        0x00005555555543c0 - 0x000055555555446e is .dynstr
        0x000055555555446e - 0x0000555555554484 is .gnu.version
        0x0000555555554488 - 0x00005555555544b8 is .gnu.version_r
        0x00005555555544b8 - 0x0000555555554590 is .rela.dyn
        0x0000555555554590 - 0x0000555555554608 is .rela.plt
        0x0000555555554608 - 0x000055555555461f is .init
        0x0000555555554620 - 0x0000555555554680 is .plt
        0x0000555555554680 - 0x0000555555554688 is .plt.got
        0x0000555555554690 - 0x0000555555554962 is .text
        0x0000555555554964 - 0x000055555555496d is .fini
        0x0000555555554970 - 0x0000555555554999 is .rodata
        0x000055555555499c - 0x00005555555549f0 is .eh_frame_hdr
        0x00005555555549f0 - 0x0000555555554b58 is .eh_frame
        0x0000555555754d98 - 0x0000555555754da0 is .init_array
        0x0000555555754da0 - 0x0000555555754da8 is .fini_array
        0x0000555555754da8 - 0x0000555555754f98 is .dynamic
        0x0000555555754f98 - 0x0000555555755000 is .got
        0x0000555555755000 - 0x0000555555755018 is .data
        0x0000555555755018 - 0x0000555555755020 is .bss
```

Ternyata alamat _entry point_ yang benar adalah `0x555555554690`. Kita hapus saja _breakpoint_ yang telah kita pasang sebelumnya, lalu pasang _breakpoint_ pada alamat yang benar:

```bash
gdb> i b
Num     Type           Disp Enb Address            What
1       breakpoint     keep y   0x0000000000000690
gdb> delete 1
gdb> b *0x555555554690
Breakpoint 2 at 0x555555554690
```

Jalankan soal tersebut dengan perintah `run` atau `r`:

```bash
gdb> r
Starting program: /tmp/elementary
Missing separate debuginfos, use: dnf debuginfo-install glibc-2.31-4.fc32.x86_64

Breakpoint 2, 0x0000555555554690 in ?? ()
=> 0x0000555555554690:  31 ed   xor    ebp,ebp
```

Kini _breakpoint_ yang kita pasang berhasil dieksekusi, dan kita berada pada _entry point_. Sekarang kita akan melakukan disassembly terhadap soal tersebut:

```bash
gdb> x/100i $pc
   ...
   0x5555555547c3:      mov    BYTE PTR [rbp-0x9],0x0
   0x5555555547c7:      mov    rdx,QWORD PTR [rip+0x200842]        # 0x555555755010
   0x5555555547ce:      mov    rax,QWORD PTR [rbp-0x30]
   0x5555555547d2:      mov    rsi,rdx
   0x5555555547d5:      mov    rdi,rax
   0x5555555547d8:      call   0x555555554660 <strcmp@plt>
   0x5555555547dd:      test   eax,eax
   0x5555555547df:      jne    0x5555555547e8
   0x5555555547e1:      mov    eax,0x1
   0x5555555547e6:      jmp    0x5555555547ed
   0x5555555547e8:      mov    eax,0x0
   0x5555555547ed:      mov    rcx,QWORD PTR [rbp-0x8]
   0x5555555547f1:      xor    rcx,QWORD PTR fs:0x28
   0x5555555547fa:      je     0x555555554801
   0x5555555547fc:      call   0x555555554640 <__stack_chk_fail@plt>
   0x555555554801:      leave
   0x555555554802:      ret
   0x555555554803:      push   rbp
```

Perhatikan ke-3 instruksi berikut ini:

```bash
   0x5555555547d2:      mov    rsi,rdx
   0x5555555547d5:      mov    rdi,rax
   0x5555555547d8:      call   0x555555554660 <strcmp@plt>
```

Fungsi `strcmp` dipanggil pada alamat `0x5555555547d8`. Kita akan memasang _breakpoint_ pada alamat tersebut, dan menonaktifkan _breakpoint_ yang telah kita pasang sebelumnya:

```bash
gdb> i b
Num     Type           Disp Enb Address            What
2       breakpoint     keep y   0x0000555555554690
        breakpoint already hit 1 time
gdb> disable 2
gdb> b *0x5555555547d8
Breakpoint 3 at 0x5555555547d8
```

Lanjutkan eksekusi dan masukkan _user_ dan _password_ yang diminta oleh soal tersebut:

```bash
gdb> c
Continuing.
Username:
foo
Password:
bar

Breakpoint 3, 0x00005555555547d8 in ?? ()
=> 0x00005555555547d8:  e8 83 fe ff ff  call   0x555555554660 <strcmp@plt>
```

Selanjutnya, kita akan melihat isi dari register `rsi` dan `rdi` yang akan dibandingkan menggunakan fungsi `strcmp`:

```bash
gdb> x/s $rsi
0x555555554974: "N1C3Tryy"
gdb> x/s $rdi
0x7fffffffdc6f: "bar\n\377\377\177"
```

Bisa terlihat bahwa password yang benar adalah `N1C3Tryy`. Ulangi eksekusi soal tersebut, dan masukkan password yang benar:

```bash
gdb> r
Starting program: /tmp/elementary
Username:
foo
Password:
N1C3Tryy

Breakpoint 3, 0x00005555555547d8 in ?? ()
=> 0x00005555555547d8:  e8 83 fe ff ff  call   0x555555554660 <strcmp@plt>
gdb> c
Continuing.
nice!
[Inferior 1 (process 13808) exited normally]
```

Ternyata flag untuk soal ini adalah password tersebut, yaitu `N1C3Tryy`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
