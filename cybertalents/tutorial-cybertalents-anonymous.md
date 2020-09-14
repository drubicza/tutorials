## [Writeup] Cyber Talents: Anonymous


Tutorial kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Anonymous](https://cybertalents.com/challenges/forensics/anonymous) yang termasuk dalam kategori digital forensik. Adapun penjelasan untuk soal tersebut adalah sebagai berikut:

```
Can you trace the anonymous guy?
```

Kemudian kita akan diberikan tautan ke sebuah file PCAP yang dapat [diunduh di sini](https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/anonymous.pcap). Filenya cukup kecil, kurang lebih 16 KB. Selanjutnya kita akan menggunakan **tshark** yang merupakan versi CLI dari [Wireshark](https://www.wireshark.org/). Jalankan **tshark** dan gunakan file **anonymous.pcap** sebagai input seperti ini:

```bash
% tshark -r anonymous.pcap
  ...
  112  71.048288 192.168.0.200 → 192.168.0.164 FTP 95 Response: 200 Active data connection established.
  113  71.049107 192.168.0.164 → 192.168.0.200 FTP 69 Request: RETR flag.txt
  114  71.049345 192.168.0.200 → 192.168.0.164 FTP 108 Response: 125 Data connection already open. Transfer starting.
  115  71.049497 192.168.0.200 → 192.168.0.164 TCP 91 47423 → 46778 [FIN, ACK] Seq=1 Ack=1 Win=64256 Len=37
  116  71.049533 192.168.0.200 → 192.168.0.164 FTP 78 Response: 226 Transfer complete.
  ...
```

Perhatikan, bahwa pada frame nomor **113**, terdapat perintah untuk mengunduh file **flag.txt** menggunakan protokol **FTP**. Dan data transfer dimulai pada frame nomor **115** dan selesai pada frame **116**. Jadi kita akan melihat data yang ditransfer pada frame **115** dengan perintah berikut ini:

```bash
% tshark -r anonymous.pcap -Y 'frame.number==115' -x
0000  08 97 98 83 44 aa ba 78 a1 28 0c 32 08 00 45 00   ....D..x.(.2..E.
0010  00 4d 0e 20 40 00 40 06 a9 ce c0 a8 00 c8 c0 a8   .M. @.@.........
0020  00 a4 b9 3f b6 ba a7 76 6b 5a e6 60 32 b4 50 11   ...?...vkZ.`2.P.
0030  01 f6 82 fc 00 00 5a 6d 78 68 5a 33 74 68 62 6d   ......ZmxhZ3thbm
0040  39 75 65 57 31 76 64 58 4e 66 64 44 42 66 64 47   9ueW1vdXNfdDBfdG
0050  67 7a 58 32 56 75 5a 48 30 3d 0a                  gzX2VuZH0=.
```

Atau jika ingin lebih jelas, gunakan opsi **-V** seperti ini:

```bash
% tshark -r anonymous.pcap -Y 'frame.number==115' -V
Frame 115: 91 bytes on wire (728 bits), 91 bytes captured (728 bits)
    Encapsulation type: Ethernet (1)
    Arrival Time: Mar 12, 2020 02:41:59.267698000 WITA
    [Time shift for this packet: 0.000000000 seconds]
    Epoch Time: 1583952119.267698000 seconds
    [Time delta from previous captured frame: 0.000152000 seconds]
    [Time delta from previous displayed frame: 0.000000000 seconds]
    [Time since reference or first frame: 71.049497000 seconds]
    Frame Number: 115
    Frame Length: 91 bytes (728 bits)
    Capture Length: 91 bytes (728 bits)
    [Frame is marked: False]
    [Frame is ignored: False]
    [Protocols in frame: eth:ethertype:ip:tcp:data]
Ethernet II, Src: ba:78:a1:28:0c:32, Dst: 08:97:98:83:44:aa
    Destination: 08:97:98:83:44:aa
        Address: 08:97:98:83:44:aa
        .... ..0. .... .... .... .... = LG bit: Globally unique address (factory default)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
    Source: ba:78:a1:28:0c:32
        Address: ba:78:a1:28:0c:32
        .... ..1. .... .... .... .... = LG bit: Locally administered address (this is NOT the factory default)
        .... ...0 .... .... .... .... = IG bit: Individual address (unicast)
    Type: IPv4 (0x0800)
Internet Protocol Version 4, Src: 192.168.0.200, Dst: 192.168.0.164
    0100 .... = Version: 4
    .... 0101 = Header Length: 20 bytes (5)
    Differentiated Services Field: 0x00 (DSCP: CS0, ECN: Not-ECT)
        0000 00.. = Differentiated Services Codepoint: Default (0)
        .... ..00 = Explicit Congestion Notification: Not ECN-Capable Transport (0)
    Total Length: 77
    Identification: 0x0e20 (3616)
    Flags: 0x4000, Don't fragment
        0... .... .... .... = Reserved bit: Not set
        .1.. .... .... .... = Don't fragment: Set
        ..0. .... .... .... = More fragments: Not set
    Fragment offset: 0
    Time to live: 64
    Protocol: TCP (6)
    Header checksum: 0xa9ce [validation disabled]
    [Header checksum status: Unverified]
    Source: 192.168.0.200
    Destination: 192.168.0.164
Transmission Control Protocol, Src Port: 47423 (47423), Dst Port: 46778 (46778), Seq: 1, Ack: 1, Len: 37
    Source Port: 47423 (47423)
    Destination Port: 46778 (46778)
    [Stream index: 3]
    [TCP Segment Len: 37]
    Sequence number: 1    (relative sequence number)
    Sequence number (raw): 2809555802
    [Next sequence number: 39    (relative sequence number)]
    Acknowledgment number: 1    (relative ack number)
    Acknowledgment number (raw): 3865064116
    0101 .... = Header Length: 20 bytes (5)
    Flags: 0x011 (FIN, ACK)
        000. .... .... = Reserved: Not set
        ...0 .... .... = Nonce: Not set
        .... 0... .... = Congestion Window Reduced (CWR): Not set
        .... .0.. .... = ECN-Echo: Not set
        .... ..0. .... = Urgent: Not set
        .... ...1 .... = Acknowledgment: Set
        .... .... 0... = Push: Not set
        .... .... .0.. = Reset: Not set
        .... .... ..0. = Syn: Not set
        .... .... ...1 = Fin: Set
            [Expert Info (Chat/Sequence): Connection finish (FIN)]
                [Connection finish (FIN)]
                [Severity level: Chat]
                [Group: Sequence]
        [TCP Flags: ·······A···F]
    Window size value: 502
    [Calculated window size: 64256]
    [Window size scaling factor: 128]
    Checksum: 0x82fc [unverified]
    [Checksum Status: Unverified]
    Urgent pointer: 0
    [SEQ/ACK analysis]
        [iRTT: 0.000561000 seconds]
        [Bytes in flight: 37]
        [Bytes sent since last PSH flag: 37]
    [Timestamps]
        [Time since first frame in this TCP stream: 0.001867000 seconds]
        [Time since previous frame in this TCP stream: 0.001306000 seconds]
    TCP payload (37 bytes)
Data (37 bytes)

0000  5a 6d 78 68 5a 33 74 68 62 6d 39 75 65 57 31 76   ZmxhZ3thbm9ueW1v
0010  64 58 4e 66 64 44 42 66 64 47 67 7a 58 32 56 75   dXNfdDBfdGgzX2Vu
0020  5a 48 30 3d 0a                                    ZH0=.
    Data: 5a6d78685a337468626d39756557317664584e6664444266…
    Text: ZmxhZ3thbm9ueW1vdXNfdDBfdGgzX2VuZH0=\n
    [Length: 37]

```

Bisa terlihat bahwa isi dari file **flag.txt** yang diunduh menggunakan FTP adalah teks yang diencode menggunakan Base64. Isinya dapat kita decode menggunakan perintah berikut ini:

```bash
% echo -n 'ZmxhZ3thbm9ueW1vdXNfdDBfdGgzX2VuZH0=' | base64 -d
flag{anonymous_t0_th3_end}
```

Jadi, untuk soal kali ini, flagnya adalah: `flag{anonymous_t0_th3_end}`. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
