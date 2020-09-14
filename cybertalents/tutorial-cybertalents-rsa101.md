## [Writeup] Cyber Talents: RSA101


Ini adalah writeup dari soal latihan [Cyber Talents: RSA101](https://cybertalents.com/challenges/cryptography/rsa101) untuk kategori crypto. Soal ini tidak terlalu sulit. Berikut ini adalah petunjuk untuk soal tersebut:

```
we received a message from our agent but we don't know how to use our key to read the message
```

Adapun soalnya, dapat diunduh pada [tautan ini](https://hubchallenges.s3-eu-west-1.amazonaws.com/crypto/RSA101.zip). Pada arsip soal tersebut, terdapat 2 buah file yaitu: **cipher** dan **key.pem**.

Untuk menyelesaikan soal tersebut, kita akan melakukan dekripsi menggunakan OpenSSL dengan perintah seperti ini:

```bash
% openssl rsautl -decrypt -in cipher -out flag.txt -inkey key.pem
```

Setelah menjalankan perintah di atas, maka kita dapat melihat flagnya pada file **flag.txt**:

```bash
% cat flag.txt
flag{RSA_nice_try}
```

Jadi flag untuk soal RSA101 adalah `flag{RSA_nice_try}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
