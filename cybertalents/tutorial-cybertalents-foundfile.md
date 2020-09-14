## [Writeup] Cyber Talents: File Found


Tutorial ini akan membahas solusi untuk soal CTF [Cyber Talents: File Found](https://cybertalents.com/challenges/forensics/file-found) yang termasuk ke dalam kategori _digital forensic_. Petunjuk untuk soal tersebut adalah sebagai berikut:

```
We found the following file on a machine,
we know it contains a secret but we do not know
what this file is can you help us obtain the code?
```

Untuk soalnya sendiri berupa file yang dapat [diunduh di sini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Forensics/foundfile). Dan setelah mengunduh file tersebut, kita dapat mulai mencari tahu tipe dari file tersebut:

```bash
% file foundfile
foundfile: compiled Java class data, version 52.0 (Java 1.8)
```

Bisa terlihat bahwa file tersebut tipenya adalah _java class_. Agar lebih mudah, kita akan melakukan _reverse engineering_, yaitu melakukan dekompilasi terhadap file tersebut menggunakan [procyon](https://bitbucket.org/mstrobel/procyon/downloads/). Unduh terlebih dahulu procyon, lalu jalankan dengan perintah seperti ini:

```bash
% mv foundfile foundfile.class
% java -jar procyon-decompiler-0.5.36.jar foundfile.class
```

Hasil dekompilasinya adalah sebagai berikut:

```java
//
// Decompiled by Procyon v0.5.36
//

public class HelloWorld
{
    public static void main(final String[] array) {
        final String s = "SYNT{SBERAFVPF_101}";
        for (int i = 0; i < s.length(); ++i) {
            char char1 = s.charAt(i);
            if (char1 >= 'a' && char1 <= 'm') {
                char1 += '\r';
            }
            else if (char1 >= 'A' && char1 <= 'M') {
                char1 += '\r';
            }
            else if (char1 >= 'n' && char1 <= 'z') {
                char1 -= '\r';
            }
            else if (char1 >= 'N' && char1 <= 'Z') {
                char1 -= '\r';
            }
            System.out.print(char1);
        }
    }
}
```

Dari hasil dekompilasi tersebut, bisa diketahui bahwa nama asli _class_ file tersbut adalah **HelloWorld**, dan jika dieksekusi akan menampilkan flag untuk soal tersebut. Caranya adalah sebagai berikut:

```bash
% mv foundfile.class HelloWorld.class
% java HelloWorld
FLAG{FORENSICS_101}
```

Bisa terlihat bahwa flag untuk soal tersebut adalah `FLAG{FORENSICS_101}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
