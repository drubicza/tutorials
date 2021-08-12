# Solusi Soal Triple Gemastik


Beberapa waktu lalu, ada yang menanyakan tentang sebuah file ELF _executable_ kepada saya. Nama filenya adalah `triple`. Jadi langsung saja saya lakukan dekompilasi. Kali ini saya akan menggunakan [cutter](https://cutter.re/). Hasil deekompilasi fungsi `main` dari _executable_ tersebut adalah sebagai berikut:

```c
undefined8 main(void)
{
    bool bVar1;
    char *pcVar2;
    undefined8 uVar3;
    int64_t in_FS_OFFSET;
    uint32_t var_ech;
    uint32_t var_e8h;
    void *s;
    char *dest;
    int64_t canary;
    
    canary = *(int64_t *)(in_FS_OFFSET + 0x28);
    memset(&s, 0, 100);
    memset(&dest, 0, 100);
    printf("Password: ");
    __isoc99_scanf(0x200f, &s);
    bVar1 = true;
    pcVar2 = (char *)fcn.00001229((char *)&s);
    pcVar2 = (char *)fcn.00001280(pcVar2);
    uVar3 = fcn.0000157f(pcVar2);
    strcpy(&dest, uVar3);
    for (_var_e8h = 0; _var_e8h < (uint64_t)(int64_t)*(int32_t *)0x40d0; _var_e8h = _var_e8h + 1) {
        if ((int32_t)*(char *)((int64_t)&dest + _var_e8h) != *(int32_t *)(_var_e8h * 4 + 0x4020)) {
            bVar1 = false;
        }
    }
    if (bVar1) {
        puts("Benar");
    } else {
        puts("Salah");
    }
    if (canary != *(int64_t *)(in_FS_OFFSET + 0x28)) {
    // WARNING: Subroutine does not return
        __stack_chk_fail();
    }
    return 0;
}
```

Berikut ini adalah penjelasan dari fungsi `main` di atas:

* Password akan diambil dengan memanggil fungsi `scanf` pada bagian ini:

```c
printf("Password: ");
__isoc99_scanf(0x200f, &s);
```

* Password tersebut akan digunakan sebagai parameter untuk pemanggilan fungsi `fcn.00001229` pada bagian ini:

```c
pcVar2 = (char *)fcn.00001229((char *)&s);
```

* Berikut ini adalah hasil dekompilasi fungsi `fcn.00001229` :

```c
char * fcn.00001229(char *arg1)
{
    uint64_t uVar1;
    char *s;
    uint32_t var_8h;

    _var_8h = 0;
    while( true ) {
        uVar1 = strlen(arg1);
        if (uVar1 <= _var_8h) break;
        arg1[_var_8h] = arg1[_var_8h] ^ 0x20;
        _var_8h = _var_8h + 1;
    }
    return arg1;
}
```

* Penjelasan singkat untuk fungsi di atas adalah setiap karakter pada password akan di `XOR` dengan nilai `0x20`. Hasil selanjutnya akan dijadikan sebagai parameter untuk pemanggilan fungsi `fcn.00001280`. Perhatikan bagian ini pada fungsi `main`:

```c
pcVar2 = (char *)fcn.00001280(pcVar2);
```

* Fungsi `fcn.00001280` agak panjang, dan berikut ini adalah hasil dekompilasinya:

```c
int64_t fcn.00001280(char *arg1)
{
    int64_t iVar1;
    char *pcVar2;
    int64_t iVar3;
    uint64_t uVar4;
    int64_t in_FS_OFFSET;
    char *s;
    char *size;
    char *var_78h;
    void *var_70h;
    undefined8 var_68h;
    void *var_60h;
    int64_t var_58h;
    int64_t var_50h;
    int64_t var_48h;
    int64_t var_40h;
    int64_t var_38h;
    int64_t var_30h;
    int64_t var_28h;
    int64_t var_20h;
    int64_t var_18h;
    int64_t var_10h;
    void *canary;

    canary = *(void **)(in_FS_OFFSET + 0x28);
    var_50h = 0x4847464544434241;
    var_48h = 0x504f4e4d4c4b4a49;
    var_40h = 0x5857565554535251;
    var_38h = 0x6665646362615a59;
    var_30h = 0x6e6d6c6b6a696867;
    var_28h = 0x767574737271706f;
    var_20h = 0x333231307a797877;
    var_18h = 0x2f2b393837363534;
    var_10h._0_1_ = 0;
    pcVar2 = (char *)strlen(arg1);
    size = pcVar2;
    if ((uint64_t)pcVar2 % 3 != 0) {
        size = pcVar2 + (3 - (uint64_t)pcVar2 % 3);
    }
    iVar1 = ((uint64_t)size / 3) * 4;
    iVar3 = malloc(iVar1 + 1);
    *(undefined *)(iVar1 + iVar3) = 0;
    var_70h = (void *)0x0;
    for (var_78h = (char *)0x0; var_78h < pcVar2; var_78h = var_78h + 3) {
        if (var_78h + 1 < pcVar2) {
            uVar4 = (int64_t)arg1[(int64_t)(var_78h + 1)] | (int64_t)var_78h[(int64_t)arg1] << 8;
        } else {
            uVar4 = (int64_t)var_78h[(int64_t)arg1] << 8;
        }
        if (var_78h + 2 < pcVar2) {
            uVar4 = (int64_t)arg1[(int64_t)(var_78h + 2)] | uVar4 << 8;
        } else {
            uVar4 = uVar4 << 8;
        }
        *(undefined *)((int64_t)var_70h + iVar3) =
             *(undefined *)((int64_t)&var_50h + (uint64_t)((uint32_t)(uVar4 >> 0x12) & 0x3f));
        *(undefined *)((int64_t)var_70h + iVar3 + 1) =
             *(undefined *)((int64_t)&var_50h + (uint64_t)((uint32_t)(uVar4 >> 0xc) & 0x3f));
        if (var_78h + 1 < pcVar2) {
            *(undefined *)((int64_t)var_70h + iVar3 + 2) =
                 *(undefined *)((int64_t)&var_50h + (uint64_t)((uint32_t)(uVar4 >> 6) & 0x3f));
        } else {
            *(undefined *)((int64_t)var_70h + iVar3 + 2) = 0x3d;
        }
        if (var_78h + 2 < pcVar2) {
            *(undefined *)((int64_t)var_70h + iVar3 + 3) =
                 *(undefined *)((int64_t)&var_50h + (uint64_t)((uint32_t)uVar4 & 0x3f));
        } else {
            *(undefined *)((int64_t)var_70h + iVar3 + 3) = 0x3d;
        }
        var_70h = (void *)((int64_t)var_70h + 4);
    }
    if (canary != *(void **)(in_FS_OFFSET + 0x28)) {
    // WARNING: Subroutine does not return
        __stack_chk_fail();
    }
    return iVar3;
}
```

* Pada dekompilasi fungsi di atas, perhatikan `var_50h` sampai `var_18h` adalah karakter _printable_ dan jika diubah menjadi ASCII _string_ maka hasilnya seperti ini:

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
```

* Dari _string_ tersebut, kita bisa mengetahui bahwa fungsi tersebut akan melakukan _encode_ base64 terhadap hasil dari fungsi pertama. Selanjutnya ada fungsi ke-3, yaitu:

```c
char * fcn.0000157f(char *arg1)
{
    uint64_t uVar1;
    char *s;
    uint32_t var_8h;

    _var_8h = 0;
    while( true ) {
        uVar1 = strlen(arg1);
        if (uVar1 <= _var_8h) break;
        arg1[_var_8h] = arg1[_var_8h] + -0x80;
        _var_8h = _var_8h + 1;
    }
    return arg1;
}
```

* Fungsi `fcn.0000157f` di atas akan mengurangi setiap (byte) karakter dengan nilai `0x80`.

* Kembali ke fungsi `main`. Pada baris berikut ini akan dilakukan perbandingan antara hasil perhitungan password setelah melalui ke-3 fungsi di atas dengan sebuah _array_ yang tiap elemennya berupa _dword_ pada alamat `0x4020`. Ukuran _array_ tersebut tersimpan pada alamat `0x40d0` yaitu ``0x2c`` atau 44 desimal.

```c
strcpy(&dest, uVar3);
for (_var_e8h = 0; _var_e8h < (uint64_t)(int64_t)*(int32_t *)0x40d0; _var_e8h = _var_e8h + 1) {
    if ((int32_t)*(char *)((int64_t)&dest + _var_e8h) != *(int32_t *)(_var_e8h * 4 + 0x4020)) {
        bVar1 = false;
    }
}
```

* Berikut ini adalah _array_ yang jumlah elemennya `0x2c` (44 desimal) pada alamat `0x40d0`:

```c
[0xd9, 0xb2, 0xb9, 0xf4, 0xe3, 0xc7, 0xda, 0xec, 0xe3, 0xb3, 0xd1,
 0xd2, 0xc5, 0xb1, 0xf4, 0xd9, 0xd4, 0xb1, 0xc9, 0xd4, 0xc5, 0xee,
 0xb9, 0xc3, 0xd1, 0xd6, 0xce, 0xc6, 0xc6, 0xe8, 0xd2, 0xaf, 0xd5,
 0xb0, 0xe8, 0xca, 0xd2, 0xec, 0xd1, 0xd2, 0xc5, 0xe8, 0xe8, 0xe4]
```

Setelah mengetahui cara kerja _executable_ tersebut, maka kita dapat membuat _script_ untuk menyelesaikannya. Berikut ini adalah langkah-langkahnya:

* Masukkan _array_ pada alamat `0x40d0` ke dalam sebuah variabel
* Lakukan operasi pengurangan sebanyak `0x80` untuk setiap elemen pada array tersebut.
* Decode hasil langkah di atas menggunakan Base64.
* Setiap karakter yang dihasilkan dari langkah di atas di XOR dengan `0x20`.

Jika ke-4 langkah di atas dituangkan ke dalam sebuah _script_ python, maka hasilnya kurang lebih seperti ini:

```python
#!/usr/bin/env python3
import base64

dword_4020  = [0xd9, 0xb2, 0xb9, 0xf4, 0xe3, 0xc7, 0xda, 0xec, 0xe3, 0xb3, 0xd1,
               0xd2, 0xc5, 0xb1, 0xf4, 0xd9, 0xd4, 0xb1, 0xc9, 0xd4, 0xc5, 0xee,
               0xb9, 0xc3, 0xd1, 0xd6, 0xce, 0xc6, 0xc6, 0xe8, 0xd2, 0xaf, 0xd5,
               0xb0, 0xe8, 0xca, 0xd2, 0xec, 0xd1, 0xd2, 0xc5, 0xe8, 0xe8, 0xe4]

print(''.join([chr(int(c) ^ 0x20) for c in base64.b64decode(''.join([chr(a - 0x80) for a in dword_4020]))]))
```

Jalankan _script_ di atas, maka hasilnya seperti ini:

```bash
% ./getflag.py
COMPFEST13{xor32_base64_shift128}
```

Jadi flag yang benar adalah `COMPFEST13{xor32_base64_shift128}`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
