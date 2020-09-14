## [Writeup] Cyber Talents: Mailer


Tutorial kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Mailer](https://cybertalents.com/challenges/forensics/mailer) yang termasuk dalam kategori digital forensic. Penjelasan untuk soal tersebut adalah sebagai berikut:

```
we got the evidence for the phishing Email but we need to know the name of malware file
```

Untuk soal kali ini, kita akan diberikan arsip yang dapat [diunduh pada tautan ini](https://hubchallenges.s3-eu-west-1.amazonaws.com/Forensics/mail_bak.7z). Setelah mengunduh file tersebut, kita dapat mengekstraknya menggunakan p7zip dengan perintah seperti ini:

```bash
% 7z -x mail_bak.7z
```

Hasil ekstraknya berupa 2 buah file, yaitu **Inbox** dan **Sent**. Karena petunjuknya berupa nama file malware, maka kita bisa menggunakan _grep_ atau [ripgrep](https://github.com/BurntSushi/ripgrep) (pada tutorial ini digunakan _ripgrep_) untuk mencari tahu apakah ada attachment pada kedua file tersebut dengan perintah seperti ini:

```bash
% rg attach *
Inbox
2017:(See attached file: eicar.com)
2027:<i>(See attached file: eicar.com)</i><br>
2037:Content-Disposition: attachment; filename="eicar.com"
4400:(See attached file: eicar.com)
4410:<i>(See attached file: eicar.com)</i><br>
4420:Content-Disposition: attachment; filename="eicar.com"
14161:(See attached file: eicar.com)
14171:<i>(See attached file: eicar.com)</i><br>
14181:Content-Disposition: attachment; filename="eicar.com"
16793:(See attached file: eicar.com)
16803:<i>(See attached file: eicar.com)</i><br>
16813:Content-Disposition: attachment; filename="eicar.com"
17161:Content-Disposition: attachment;
18116:Content-Disposition: attachment; filename="Kumamoto-Castle-6.jpg"
21002:(See attached file: eicar.com)
21012:<i>(See attached file: eicar.com)</i><br>
21022:Content-Disposition: attachment; filename="eicar.com"

Sent
11976:                                                class="attachment-92x92
12017:                                                class="attachment-92x92
12054:                                                class="attachment-92x92
12092:                                                class="attachment-92x92
12133:                                                class="attachment-92x92
12169:                                                class="attachment-92x92
12204:                                                class="attachment-92x92
26078:                                                class="attachment-92x92
26119:                                                class="attachment-92x92
26157:                                                class="attachment-92x92
26195:                                                class="attachment-92x92
26232:                                                class="attachment-92x92
26269:                                                class="attachment-92x92
26305:                                                class="attachment-92x92
```

Bisa terlihat bahwa pada file **Inbox** terdapat attachment dengan nama **eicar.com**, namun setelah mencobanya sebagai flag, ternyata salah. Selanjutnya, kita akan menggunakan filter berupa ekstensi _executable_ yang umum pada sistem operasi Windows sebagai filter:

```bash
% rg "\.exe|\.dll|\.ocx" *
Sent
2943:> ctbank.com/Mal_strike8941934890753353453.exe
2944:> <http://ctbank.com/Mal_strike8941934890753353453.exe>
2958:>>     www.ctbank.com/Mal_strike8941934890753353453.exe
2959:>>     <http://www.ctbank.com/Mal_strike8941934890753353453.exe>
2992:            href="http://ctbank.com/Mal_strike8941934890753353453.exe">ctbank.com/Mal_strike8941934890753353453.exe</a><br>
3018:                          href="http://www.ctbank.com/Mal_strike8941934890753353453.exe"
```

Kali ini, hasilnya cukup menarik karena terdapat tautan ke situs dan nama sebuah file _executable_ dengan ekstensi **exe** dan namanya cukup mencurigakan. Coba kita gunakan sebagai flag dengan format:

```
flag{Mal_strike8941934890753353453.exe}
```

Ternyata benar, `flag{Mal_strike8941934890753353453.exe}` adalah flag yang valid. Sekian tutorial kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
