## [Writeup] Cyber Talents: ARP Storm

Writeup kali ini akan membahas soal [Cyber Talents: ARP Storm](https://cybertalents.com/challenges/network/arp-storm) untuk kategori network. Soal ini tidak terlalu sulit. Berikut ini adalah petunjuk dari soal tersebut:

```
An attacker in the network is trying to poison the arp table
of 11.0.0.100, the admin captured this PCAP.
```

Adapun hasil _network capture_ (PCAP) dari soal tersebut dapat [diunduh pada tautan ini](https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/ARP+Storm.pcap). Untuk menyelesaikan soal ini, kita akan menggunakan aplikasi [Wireshark](https://www.wireshark.org/). Agar lebih mudah, kita akan menggunakan versi CLI dari Wireshark yaitu **tshark**. Jalankan **tshark** menggunakan perintah seperti ini:

```bash
% tshark -r ARP+Storm.pcap
    1   0.000000 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x005a
    2   0.000442 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x006d
    3   0.000745 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x0078
    4   0.000994 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x0068
    5   0.001290 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x005a
    ...
   64   0.019096 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x0077
   65   0.019337 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x0062
   66   0.019617 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x006e
   67   0.019858 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x0030
   68   0.020136 00:0c:29:1e:1d:81 → ff:ff:ff:ff:ff:ff ARP 42 Unknown ARP opcode 0x003d
```

Bisa terlihat, bahwa setiap paket menggunakan _opcode_ yang berbeda dalam format hexadesimal. Kita akan mengambil _opcode_ dari setiap paket, lalu menampilkannya dalam format ASCII, menggunakan AWK dengan perintah seperti ini:

```bash
% tshark -r ARP+Storm.pcap -Tfields -e arp.opcode | awk '{printf("%c",$1)}'
ZmxhZ3tnckB0dWl0MHVzXzBwY09kZV8xc19BbHdAeXNfQTZ1U2VkX3QwX3AwMXMwbn0=
```

Ternyata gabungan _opcode_ dari setiap paket adalah string yang diencode menggunakan Base64. Jadi kita akan menambahkan perintah untuk melakukan decode Base64 pada perintah di atas:

```bash
% tshark -r ARP+Storm.pcap -Tfields -e arp.opcode | awk '{printf("%c",$1)}' | base64 -d
flag{gr@tuit0us_0pcOde_1s_Alw@ys_A6uSed_t0_p01s0n}
```

Nah, itu dia flagnya `flag{gr@tuit0us_0pcOde_1s_Alw@ys_A6uSed_t0_p01s0n}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
