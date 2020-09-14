## [Writeup] Cyber Talents: Catch Me If You Can

Ini adalah writeup soal latihan [Cyber Talents](https://cybertalents.com/challenges/malware/catch-me) untuk kategori reverse engineering. Berikut adalah petunjuk yang diberikan:

```
Catch me if you can
```

Soal ini tingkat kesulitannya medium dan soalnya berupa file executable dengan nama `catchme.exe`. Kita akan menggunakan radare2 untuk mengerjakan soal tersebut. Jalankan radare2 dengan perintah seperti ini:

```bash
% r2 -A catchme.exe
```

Selanjutnya, kita akan mencari hal yang menarik pada soal tersebut. Gunakan perintah `f` pada radare2:

```C
[0x00411055]> f
0x00417b40 9 str.i_got_it
...
```

Nah, ada string yang menarik. Selanjutnya, kita mencari referensi yang mengacu pada string tersebut. Gunakan perintah `axt`:

```C
[0x00411055]> axt 0x00417b40
entry1 0x411751 [DATA] push str.i_got_it
```

Lakukan disassembly terhadap alamat dimana instruksi tersebut berada. Gunakan perintah `pd 50@0x411751` untuk melakukan disassembly sebanyak 50 instruksi. Hasilnya seperti ini:

```C
[0x00411055]> pd 50@0x411751
│           0x00411751      68407b4100     push str.i_got_it                               ; 0x417b40 ; "i_got_it"
│           0x00411756      8d4588         lea eax, [local_78h]
│           0x00411759      50             push eax
│           0x0041175a      e8eafaffff     call fcn.00411249
│           0x0041175f      83c408         add esp, 8
│           0x00411762      85c0           test eax, eax
│       ┌─< 0x00411764      0f8433010000   je 0x41189d
│       │   0x0041176a      c78528ffffff.  mov dword [local_d8h], 0x25                     ; '%' ; 37
│       │   0x00411774      c7852cffffff.  mov dword [local_d4h], 0xd                      ; 13
│       │   0x0041177e      c78530ffffff.  mov dword [local_d0h], 0x15                     ; 21
│       │   0x00411788      c78534ffffff.  mov dword [local_cch], 4
│       │   0x00411792      c78538ffffff.  mov dword [local_c8h], 0x13                     ; 19
│       │   0x0041179c      c7853cffffff.  mov dword [local_c4h], 0x74                     ; 't' ; 116
│       │   0x004117a6      c78540ffffff.  mov dword [local_c0h], 1
│       │   0x004117b0      c78544ffffff.  mov dword [local_bch], 0x36                     ; '6' ; 54
│       │   0x004117ba      c78548ffffff.  mov dword [local_b8h], 0x7f                     ; 127
│       │   0x004117c4      c7854cffffff.  mov dword [local_b4h], 0x78                     ; 'x' ; 120
│       │   0x004117ce      c78550ffffff.  mov dword [local_b0h], 0x35                     ; '5' ; 53
│       │   0x004117d8      c78554ffffff.  mov dword [local_ach], 0x7f                     ; 127
│       │   0x004117e2      c78558ffffff.  mov dword [local_a8h], 0x1e                     ; 30
│       │   0x004117ec      c7855cffffff.  mov dword [local_a4h], 0x5f                     ; '_' ; 95
│       │   0x004117f6      c78560ffffff.  mov dword [local_a0h], 0x45                     ; 'E' ; 69
│       │   0x00411800      c78564ffffff.  mov dword [local_9ch], 0x64                     ; 'd' ; 100
│       │   0x0041180a      c78568ffffff.  mov dword [local_98h], 0x79                     ; 'y' ; 121
│       │   0x00411814      c7856cffffff.  mov dword [local_94h], 0x48                     ; 'H' ; 72
│       │   0x0041181e      c78570ffffff.  mov dword [local_90h], 0x13                     ; 19
│       │   0x00411828      c7857cffffff.  mov dword [local_84h], 0                        ; local_84h = 0
│      ┌──< 0x00411832      eb0f           jmp 0x411843                                    ; lompat ke 0x411843
│      ││   ; CODE XREF from entry1 (0x411872)
│      ││   0x00411834      8b857cffffff   mov eax, dword [local_84h]                      ; eax = counter
│      ││   0x0041183a      83c001         add eax, 1                                      ; eax++
│      ││   0x0041183d      89857cffffff   mov dword [local_84h], eax                      ; counter = eax
│      ││   ; CODE XREF from entry1 (0x411832)
│      └──> 0x00411843      83bd7cffffff.  cmp dword [local_84h], 0x13                     ; apakah counter == 0x13?
│      ┌──< 0x0041184a      7d28           jge 0x411874                                    ; jika lebih besar atau sama dengan, maka lompat ke 0x411874
│      ││   0x0041184c      8b857cffffff   mov eax, dword [local_84h]                      ; eax = counter
│      ││   0x00411852      0fbe8800a041.  movsx ecx, byte [eax + str.Catch_Me_If_You_Can] ; ecx = "Catch Me If You Can"[counter]
│      ││   0x00411859      8b957cffffff   mov edx, dword [local_84h]                      ; edx = counter
│      ││   0x0041185f      338c9528ffff.  xor ecx, dword [ebp + edx*4 - 0xd8]             ; "C"
│      ││   0x00411866      8b857cffffff   mov eax, dword [local_84h]                      ; eax = counter
│      ││   0x0041186c      888800a04100   mov byte [eax + str.Catch_Me_If_You_Can], cl    ; "Catch Me If You Can"[counter] = cl
│      ││   0x00411872      ebc0           jmp 0x411834                                    ; ulangi sampai habis
│      ││   ; CODE XREF from entry1 (0x41184a)
│      └──> 0x00411874      8b857cffffff   mov eax, dword [local_84h]
│       │   0x0041187a      89855cfeffff   mov dword [local_1a4h], eax
│       │   0x00411880      83bd5cfeffff.  cmp dword [local_1a4h], 0x14
│       │   0x00411887      7302           jae 0x41188b
│       │   0x00411889      eb05           jmp 0x411890
│       │   ; CODE XREF from entry1 (0x411887)
│       │   0x0041188b      e8c4f8ffff     call fcn.00411154
│       │   ; CODE XREF from entry1 (0x411889)
│       │   0x00411890      8b8d5cfeffff   mov ecx, dword [local_1a4h]
│       │   0x00411896      c68100a04100.  mov byte [ecx + str.Catch_Me_If_You_Can], 0 ; section..data ; [0x41a000:1]=67 ; "Catch Me If You Can"
│       │   ; CODE XREFS from entry1 (0x411729, 0x411764)
│       └─> 0x0041189d      52             push edx
│           0x0041189e      8bcd           mov ecx, ebp
```

Secara sederhana, disassembly di atas akan melakukan operasi XOR sebanyak `0x13` kali, yaitu panjang kalimat `Catch Me If You Can`. Kalimat tersebut di-XOR dengan sejumlah byte yang disimpan pada stack. Berikut ini adalah urutan byte tersebut:

```
0x25,0x0d,0x15,0x04,0x13,0x74,0x01,0x36,0x7f,0x78,0x35,0x7f,0x1e,0x5f,0x45,0x64,0x79,0x48,0x13
```

Selanjutnya, kita dapat menggunakan python untuk operasi XOR seperti ini:

```python
>>> b = [0x25,0x0d,0x15,0x04,0x13,0x74,0x01,0x36,0x7f,0x78,0x35,0x7f,0x1e,0x5f,0x45,0x64,0x79,0x48,0x13]
>>> s = "Catch Me If You Can"
>>> print ''.join([chr(ord(s[i]) ^ b[i]) for i in range(len(s))])
flag{TLS_1S_G00D:)}
```

Nah, bisa terlihat bahwa flag untuk soal tersebut adalah `flag{TLS_1S_G00D:)}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
