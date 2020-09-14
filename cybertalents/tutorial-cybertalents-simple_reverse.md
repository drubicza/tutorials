## [Writeup] Cyber Talents: Simple Reverse


Tutorial kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Simple Reverse](https://cybertalents.com/challenges/malware/simple-reverse) yang termasuk ke dalam kategori reverse engineering. Berikut ini adalah petunjuk untuk soal tersebut:

```
Only a plaintext password would be easier...
```

Soal ini disertai aplikasi yang harus kita unduh pada [tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Reverse/simple_reverse). Unduh terlebih dahulu soal tersebut, lalu kita periksa tipenya:

```bash
% file simple_reverse
simple_reverse: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=8a0dd1d8dbda08a213ac68dd9cd9a5f4836f95cf, not stripped
```

Selanjutnya, kita akan menggunakan [radare2](https://rada.re/n/) yang dilengkapi plugin [ghidra](https://ghidra-sre.org/) untuk melakukan analisis. Anda juga dapat menggunakan ghidra saja. Load soal tersebut pada radare2:

```bash
% r2 -A simple_reverse
[x] Analyze all flags starting with sym. and entry0 (aa)
[x] Analyze function calls (aac)
[x] Analyze len bytes of instructions for references (aar)
[x] Check for vtables
[x] Type matching analysis for all functions (aaft)
[x] Propagate noreturn information
[x] Use -AA or aaaa to perform additional experimental analysis.
[0x00400530]>
```

Selanjutnya, kita akan melakukan dekompilasi terhadap fungsi **main**:

```bash
[0x00400530]> pdg @main

undefined8 main(undefined8 argc, char **argv)
{
    int32_t iVar1;
    undefined8 uVar2;
    char **var_10h;
    undefined8 var_4h;

    if ((int32_t)argc < 2) {
        sym.imp.printf("Usage:%s <password>\nYou need to enter the password to get the flag!\n", *argv);
        uVar2 = 1;
    } else {
        iVar1 = sym.check_password(argv[1]);
        if (iVar1 == 0) {
            sym.print_flag(argv[1]);
        } else {
            sym.imp.puts("Wrong password");
        }
        uVar2 = 0;
    }
    return uVar2;
}
```

Berdasarkan hasil dekompilasi di atas, kita dapat melihat bahwa kita harus memasukkan password sebagai parameter pertama ketika menjalankan soal tersebut. Password yang kita masukkan kemudian akan dijadikan parameter ketika memanggil fungsi `sym.check_password`. Selanjutnya, kita akan melakukan dekompilasi terhadap fungsi `sym.check_password`.

```bash
[0x00400530]> pdg @sym.check_password

undefined8 sym.check_password(char *arg1)
{
    int32_t iVar1;
    undefined8 uVar2;
    int64_t in_FS_OFFSET;
    char *s;
    int32_t var_28h;
    int64_t var_20h;
    int64_t var_18h;
    int64_t var_10h;
    int64_t canary;

    canary = *(int64_t *)(in_FS_OFFSET + 0x28);
    var_20h = -0x4c0d201e200b4f12;
    var_18h = -0x4c170f161c204e1f;
    var_10h._0_2_ = 0xf2;
    iVar1 = sym.imp.strlen(arg1);
    if (iVar1 == 0x11) {
        var_28h = 0;
        while (var_28h < 0x11) {
            if (*(uint8_t *)((int64_t)&var_20h + (int64_t)var_28h) != (uint8_t)(arg1[var_28h] ^ 0x80U)) {
                uVar2 = 0xffffffff;
                goto code_r0x004006bf;
            }
            var_28h = var_28h + 1;
        }
        uVar2 = 0;
    } else {
        uVar2 = 0xffffffff;
    }
code_r0x004006bf:
    if (canary != *(int64_t *)(in_FS_OFFSET + 0x28)) {
        uVar2 = sym.imp.__stack_chk_fail();
    }
    return uVar2;
}
```

Terdapat 2 buah variabel dengan nilai yang cukup besar pada fungsi tersebut. Agar lebih jelas, kita akan membandingkan dengan disassembly fungsi tersebut:

```bash
[0x00400530]> pdf @sym.check_password
            ; CALL XREF from main @ 0x4007fe
┌ 175: sym.check_password (char *arg1);
│           ; var char *s @ rbp-0x38
│           ; var signed int64_t var_28h @ rbp-0x28
│           ; var size_t var_24h @ rbp-0x24
│           ; var int64_t var_20h @ rbp-0x20
│           ; var int64_t var_18h @ rbp-0x18
│           ; var int64_t var_10h @ rbp-0x10
│           ; var int64_t canary @ rbp-0x8
│           ; arg char *arg1 @ rdi
│           0x00400626      55             push rbp
│           0x00400627      4889e5         mov rbp, rsp
│           0x0040062a      4883ec40       sub rsp, 0x40
│           0x0040062e      48897dc8       mov qword [s], rdi          ; arg1
│           0x00400632      64488b042528.  mov rax, qword fs:[0x28]
│           0x0040063b      488945f8       mov qword [canary], rax
│           0x0040063f      31c0           xor eax, eax
│           0x00400641      48b8eeb0f4df.  movabs rax, 0xb3f2dfe1dff4b0ee
│           0x0040064b      488945e0       mov qword [var_20h], rax
│           0x0040064f      48b8e1b1dfe3.  movabs rax, 0xb3e8f0e9e3dfb1e1
│           0x00400659      488945e8       mov qword [var_18h], rax
│           0x0040065d      66c745f0f200   mov word [var_10h], 0xf2    ; 242
│           0x00400663      488b45c8       mov rax, qword [s]
│           0x00400667      4889c7         mov rdi, rax                ; const char *s
│           0x0040066a      e871feffff     call sym.imp.strlen         ; size_t strlen(const char *s)
│           0x0040066f      8945dc         mov dword [var_24h], eax
│           0x00400672      837ddc11       cmp dword [var_24h], 0x11
│       ┌─< 0x00400676      7407           je 0x40067f
│       │   0x00400678      b8ffffffff     mov eax, 0xffffffff         ; -1
│      ┌──< 0x0040067d      eb40           jmp 0x4006bf
│      ││   ; CODE XREF from sym.check_password @ 0x400676
│      │└─> 0x0040067f      c745d8000000.  mov dword [var_28h], 0
│      │┌─< 0x00400686      eb2c           jmp 0x4006b4
│      ││   ; CODE XREF from sym.check_password @ 0x4006b8
│     ┌───> 0x00400688      8b45d8         mov eax, dword [var_28h]
│     ╎││   0x0040068b      4898           cdqe
│     ╎││   0x0040068d      0fb64405e0     movzx eax, byte [rbp + rax - 0x20]
│     ╎││   0x00400692      8b55d8         mov edx, dword [var_28h]
│     ╎││   0x00400695      4863ca         movsxd rcx, edx
│     ╎││   0x00400698      488b55c8       mov rdx, qword [s]
│     ╎││   0x0040069c      4801ca         add rdx, rcx
│     ╎││   0x0040069f      0fb612         movzx edx, byte [rdx]
│     ╎││   0x004006a2      83f280         xor edx, 0xffffff80         ; 4294967168
│     ╎││   0x004006a5      38d0           cmp al, dl
│    ┌────< 0x004006a7      7407           je 0x4006b0
│    │╎││   0x004006a9      b8ffffffff     mov eax, 0xffffffff         ; -1
│   ┌─────< 0x004006ae      eb0f           jmp 0x4006bf
│   ││╎││   ; CODE XREF from sym.check_password @ 0x4006a7
│   │└────> 0x004006b0      8345d801       add dword [var_28h], 1
│   │ ╎││   ; CODE XREF from sym.check_password @ 0x400686
│   │ ╎│└─> 0x004006b4      837dd810       cmp dword [var_28h], 0x10
│   │ └───< 0x004006b8      7ece           jle 0x400688
│   │  │    0x004006ba      b800000000     mov eax, 0
│   │  │    ; CODE XREFS from sym.check_password @ 0x40067d, 0x4006ae
│   └──└──> 0x004006bf      488b75f8       mov rsi, qword [canary]
│           0x004006c3      644833342528.  xor rsi, qword fs:[0x28]
│       ┌─< 0x004006cc      7405           je 0x4006d3
│       │   0x004006ce      e81dfeffff     call sym.imp.__stack_chk_fail ; void __stack_chk_fail(void)
│       │   ; CODE XREF from sym.check_password @ 0x4006cc
│       └─> 0x004006d3      c9             leave
└           0x004006d4      c3             ret
```

Dari informasi di atas, kita bisa menarik kesimpulan sementara berikut ini:

* Password berupa karakter dengan panjang 17 byte (0x11 dalam hexadesimal).
* Setiap byte pada password akan di-XOR dengan nilai 0x80.
* Hasil XOR akan dibandingkan dengan nilai yang telah ditentukan, yaitu:
** 0xb3f2dfe1dff4b0ee
** 0xb3e8f0e9e3dfb1e1
** 0xf2
* Perlu diingat bahwa, nilai yang telah ditentukan tersebut formatnya adalah [Little Endian](https://en.wikipedia.org/wiki/Endianness)

Untuk memperoleh password yang benar, maka kita dapat membalikkan fungsi tersebut. Jika menggunakan python 3, maka akan seperti ini:

```python
>>> print("".join(chr(a ^ 0x80) for a in bytes.fromhex("eeb0f4dfe1dff2b3e1b1dfe3e9f0e8b3f2")))
n0t_a_r3a1_ciph3r
```

Bisa terlihat, bahwa passwordnya adalah `n0t_a_r3a1_ciph3r`. Selanjutnya kita akan menjalankan soal tersebut dengan memasukkan password tersebut sebagai parameter seperti ini:

```bash
% ./simple_reverse n0t_a_r3a1_ciph3r
Your flag:
flag{xor_is_pretty_simple}
```

Nah, itu dia flag untuk soal kali ini, yaitu `flag{xor_is_pretty_simple}`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
