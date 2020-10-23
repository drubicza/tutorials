# Tutorial Dekompilasi Telebot For All Clickbot Telegram


## Pendahuluan

Tutorial kali ini akan membahas cara melakukan dekompilasi script **Telebot for All Clickbot Telegram**. Script tersebut sebenarnya cukup sederhana namun sifatnya modular, yaitu terdiri dari beberapa script yang berbeda berdasarkan fungsinya masing-masing. Hal tersebut dapat terlihat pada bagian akhir tutorial ini. Sebagai tambahan, script tersebut memiliki nama **doge.py**.


## Langkah-langkah

Berikut ini adalah potongan kode dari script tersebut:

```python
# NGAPAIN JUGA DI RECODE
# LEBIH GAMPANG BUAT SENDIRI ^^
# ENC BY EZZ-KUN
# RAYEZ_ID

exec((lambda __, _, : _(b'begin 666 <data>\nM(R! ... \n \nend\n',__))("uu_codec",__import__('codecs').decode))
```

Dari potongan kode script tersebut, bisa diketahui bahwa script tersebut menggunakan **uuencode** dan dengan melihat ukuran script tersebut, bisa disimpulkan bahwa hal tersebut dilakukan berulang kali. Selain itu, script tersebut juga di-obfuscate dengan cara berbeda. Jadi, script tersebut dapat dikatakan memiliki _2-layer obfuscation_. Namun, dapat dengan mudah untuk di-deobfuscate menggunakan script berikut ini:

![deobfu.sh](https://raw.githubusercontent.com/drubicza/misc/master/others/ss-telebot_deobfush.png)

Jalankan script tersebut pada direktori dimana script **doge.py** berada, dan script tersebut akan men-deobfuscate script **doge.py**, namun tidak sampai akhir. Hal tersebut terjadi, karena ketika tutorial ini ditulis, uncompyle6 gagal melakukan dekompilasi bagian akhir script tersebut. Berikut ini adalah bagian tersebut:

```python
exec(eval((lambda ____, __, _: ____.join([_(___) for ___ in __]))('', [95, 95, 105, 109, 112, 111, 114, 116, 95, 95, 40, 39, 109, 97, 114, 115, 104, 97, 108, 39, 41, 46, 108, 111, 97, 100, 115], chr))(eval((lambda ____, __, _: ____.join([_(___) for ___ in __]))('', [95, 95, 105, 109, 112, 111, 114, 116, 95, 95, 40, 34, 122, 108, 105, 98, 34, 41, 46, 100, 101, 99, 111, 109, 112, 114, 101, 115, 115], chr))(b'x\x9c\x85UYo\x1c\xc7\x11\x9e\x99\xbdG\xbc\xb9\xe2\xf2VS\xa2h\xd1\x92IJ\xb2\x9d@1\x1cK4\x19+\x11\x18\xc1T`\xb9\x05Y\x18N7\xc9!\xe7Xu\xf7J\xdc\x85\x0ch%\xd9\x08`\x07\x08\x12[\x86m\x18Y\xe6!\x88l$\x08\x90\xbc8A\x9c?\x91\xa7y\xd4\x02\x92\xf8\x03\xf2\xe2\'W\xf5\x1e\xa4\x0e;;;U\xd5U555U_W\xd7\x8d\'~i\xb8_\x83[\xfe\xd82\x8c\xca\x7f\x99\xc1L\xdf\x08Lj\x06\x16\xb5\x82\x04M\x98\x06\xe8,?\x19\xa4h*H\xd3t\x90\xa1\x19\xadK\xf8\xd9 Gs\x81Mm\xbdN\xfa\xfb\x82\x0e\xda\x11t\xd2\xce\xa0\x8bvi]\xca\xef\x0ezh\x8f\x96\xd3 \xf7\xd2\xde\xa0\x8f\xf6\xe9u\xc6\xef\x0f\xf24\xaf\xe5\xac\xbf\x9f\xee\xd7|\x80\x0eh^\xa0\x05\xcd\x07\xe9\xa0\xe6CtH\xf3~\xda\xaf\xf90\x1d\xd6|\x84\x8eh>JG5\x1f\xa3c\xc0s\xfexp\x80\x1e0\x8d\xb7\x8cp\xd44\xf0\xe2\x84\xd9\xb7M\xe0\x13l\x1f\xf2\xb7\x8d\xd0\xbah\xb0\x0e\xd6y\xc7\xa0\x07\xc1\xda\xa5\xad\xf9\x86\x15\xd6\xdd{\xd7\xac\x87\x1eb\xbd\xac\x0f|\'\xf9\xe8\xc6a>\xc9\xfa\x7fc\xd2)>U{n\x1b\xa3\xf7\xd7\xf2,\xbf\x8d\x9e\xfb\xe9\x11>T\x9b\xe6G\xb6\xcd\x8d\xe7\xe9Q6@\x0f\xf1\xc2\xc6\xb1\xda\x0b\xac\xc0\x8f~n\xb0\xc1/\x12\xe8\xc7\x8f\xb2\xa1M\xeb\xb7\xa6\xfc\x08\xa4\xe1\xb64\xd2\x96FQ\x12\x7f\x82\x0c\xc6t&\x85\xda\x04\xbe)\xeckk\x06k3l|\xfbq\xe9 ;\xd0\xce\x97\xb0\t\xc8wV\xe7;\xfb\x8c|\x0f\xea|w\xf3;\xd4\x8c0\xa9\xa3\xb7=v\xa3\xb3\xc3\xf0ms\xbb_\xa7\xdf\x94]6\xa6\xa7v\x10K\xd3\x89\xb8\xeb\x02\xf7\xf9\x9ap\x82y\xdf\xe3\xa1\x8a\x93\xb2\x1c\xbaq\x9a_\x83\x85\x04{\xef\xcf\xb8z\xc3\x93*\x12\xe57\xf9\xd5\x12\x97*\x1e\x01\xd5\x99H\xcd;\xbe\xbf\xe2\xb8\x9b\xa7Cy\x9d\x8b\x96\xb1p6(FB\xcd\xaf;\xealx\xcdS\xbci\x98\xb6\xe2\xbe\x9fG^\x08\x860\xe4~\xcb\xbd\xff\x1cw\xae\xf1\xc7\x95\xf0\xd6LS\x13\'1P\x9c\xfc\x95\xe4b\xda\x8c\x87\x97\xb9\x94^\x14\x9ew\xa4\xbc\x1e\t\xb6\xc49\xe3lA\x88H@\xfc\xaeE?\x8a\xd8[\x8e\xa7\xb4&\x1e\xc1\xa7N\xfb\x82;\xac|\xde\x11\xcas\xbd\xa2\x136\x8c\x10,%}\xce\x8bK u\x9e\xe1NIy\xab%\x7f9*\x15+\xff\xb6\xed\x91K\xc7\x7fr\xf2D\xb0\xb0\xe5r\xb7\xa489E\x9a\xba\x93\xc1$)\x96\xd5z\x14\x92\x17\x02R\xf4\x8a\xc4\x0b\xa5\x82J\x90\x15\xf9\xa2\xfd}6\x055F\xfd\xf7:\x08\xe9\x10\x07+\xefED4\xaa \xff\xbf\xf3\x955\x1er\xe1@o\x88\x1b\xf9\x114\xd1\xb1\xc9\x8e\t\x8du\xcd=\x13\x03\xe6\x84\x91\x82{\x1e\xa7\xc6) 7`f\x14\x8c\xd7\x8d\xcb\xc7\xde5\x95\xb1a\xd6\xac\x1b&\xa2E\xcb\tD\x9bJ\xd6R*]Ko\xc3$\xc9\x1bh\xbb\x9ae\x06\xe0&\xb1\x14\x9b\xf6\xc3\xcf>\xc5\xdf\xd7?\x9d\xce\xc4\tY\x96qZ*\x16\x95T\x9c\xba.\xa0\xe3qj\xd5/\xc9\xf58\xa9\xbc\x80\x8b\x1exa\x9c\x16N\xc8\xa2\x00\x9ad\xca\xd8t\xef\x19b\x0c\xd4\xdff_Y\xa8T~Q\n_\x8d\xb3\x01\x0f\xd7\xb8\xf26\xbb1\xc9,\xa6mv\xc0e\x9b\xe2\x00\xc8\xa5\xe7\x804:\xf0Rp\xff\xe6?\x89\x16_\x0e\x96]\xe1\x15\x15gd\xa5L\x84S\xe6\x15r\x96\x95N\x82\xef\xd3\xae\x88\xf3\x95H\x91E\xa8\xd6i\xa8"\xe0\xdd\xddDEk\x03,\xba{\xc7\xac\xd9\x1c\xb5\xbah\xe7\x80(\xa3f2\x93Y,\xc1\xac\xb5\xe4\xb6\t\xe5{\xf1]CYb\x1c\x8a\xf3\x86Jl@\xc9X\xf2\x86\x01\x03#\xd5\x18\x18Z\x97\xd6\xe5\xcc\xd4\xb2,\xad\xcb\xd8\xa7\xcb\x98Y\xaa\xf44\xf3\xdaB>\x17lmU\x06\x9a\xd2\x13\x86\x86\'z\xb4\x0c\xff\xc3/\xec|\xfa\x13\xe7\xd7\xb9\xbb\xe9\x85kd\xb9,\x15\x0f\x1a\xea\xb9\xe0\x12i\xbaVr-\xd5e\x91\x87\x10\xf7rq\x0e\xfa%T\x14\xf92N\xb9e\xd7\xe7q\x92E\x00\xaa\xfd`\x16\x03H\nH\x06\x91\x0c!\xc1v\xde3\xc5(\xcacm2\x8eM\xce8\xa1\x178\x8a\x8f`\xc9:t\x11\x07\xcc$\\yl\xe3\x14\xce\x1b3N+G@\xa3w\x12\xb0\xacL\xac+U\x94\xa7fg\x8b\x0e$\xbc\xe2\x853n\x14\xcc\n\xe7\xfa\xac\xbbx\xf1\xbc\xf8\xd1\xc53\x17\x04A\xc7}\xed\xc4m\x9bf\xa2\xd5U\xdf\x0b9\xcd\xfcr\x8fp\xae!,.\x9e;\xbb\xb4\xb0\x83\x98\x87/\xf2\xb9#J\x1f\xef\xe2\xc1~\xe2\x97\xcb]\xd2\xb0\xf1\xd8e\x90\xeb\xd5\xdf\xd7\xab\x7f\xad\xdf~\xbf~\xeb/\xf5[_\xd7o\xff\xba~\xebw\x0f\xff\xf3\xafG\x7f\xfe{\xbd\xfaI\xbd\xfa\x87\x07w>\xabW?xt\xf3\xbdz\xf5\xc3z\xf5N\xbd\xfaM\xbd\xfa)y\xe7\x1d\x02\x0f?\xf8\xfc\x1f\x0f\xee\xfeM\xfb}\xd3\xf0&\'\xe6\xc8\xfd\x9b\x7f$\xc7\x1b\xec\xc4\xdc\x93o\x7f\np\xed]\xba\xf8\x18\xe0\xd6\x12\x1ans\x1anp:^=\xb3\x0b\xb7\x82\xf1\x0c\xa8\xa54\xd4z4\xd4\xd2K\xa5\x1cD\xbb\x7f\xf7\xcb\xfbw\xbf\x82\x7fs\xf5UC\xb1\xbbBE\xa5\xaf\x89\xa4\xce\\\xce\x8f\x1c\x86X\xd2H\x11\x13H\x0e?\xa3\xe9\xd9f\xd3\xb7^\xde\xedz^w\xbd\x07\xbb~\x14\xd6;I\xdc\xc1\x88\x1e\x08\xfb:\xa0k\x82\x90V:_\x12\xfc\xd9\x95\x97r\xb9\x0b\xeb\x9e$Roh\x02R\x08[s\x15\xf6\xaat|N&&\xa0\xc2\xf3\x823O\x11\x155\xb6\xfa\x15\x8f\xfd\x00\x80\xde\xbc*\x8f\x87\x0b\xa7\xe7\xa6\x8f\xd3lk\xfc\nD\x85@\xe4\tL\x89\x8e\xb7\x0c3\xca\x9fY-\x85\xae\x82\xb3E\xce\x04p\xc88k\\\nl\x87\xc0! 2?\xe0\xee6\x8e*)pj\t\xac\'\xed\xdd\xeb\xa9\xcaE\x88e\xa3q\x1f\x12\xac\x11\xedn{p<\x8e\xa4\xe8DS\x17\x92\xee\xc7\xf6\x1aMn\xc8(\x8c-\xd1\xd8\x93\xb1\x15I\x9am\x1d\x11b\x18=\xb2\xad\xe1\x1f\xe7\xd4:\x9ev\xd06q\x10-\t8\x94D\xaf\xde\nE\xe1\xe1\xe9\xce\xb7<\xa5\'\xaa\x98D2\xa5\x87\xf3\x05\xfdTl\xc2\x00\x87\x13F(x.Rq\x02\xb6)\xccp\xbe\xa5p\xbe;\xaa$w\x07\x02h\xf4\x88\xd1\xfd\xc5\xa8\xdc\xdd\x03\x8c=\xe8x%\x88X\xc9\xe7\xaf"\xba%\xc5\xc3\x08\xd0\x91\x07l\xe4a\xb8\xf7\xc0}\x1eh\xda\xcc\x9a]V6\x9d5\xf5e%\xcdl\xa6\x03$\x1bP\xd4\x81\xf3\xc3:\x82+\x0b\xee\x84\rR\xd2\xdakGn\xc33\xdf\x01O\x1d\x066')))
```

Jadi untuk langkah terakhir, kita akan menggunakan modul **dis** bawaan python. Ubah script tersebut menjadi seperti ini:

```python
import dis
dis.dis(eval((lambda ____, __, _: ____.join([_(___) for ___ in __]))('', [95, 95, 105, 109, 112, 111, 114, 116, 95, 95, 40, 39, 109, 97, 114, 115, 104, 97, 108, 39, 41, 46, 108, 111, 97, 100, 115], chr))(eval((lambda ____, __, _: ____.join([_(___) for ___ in __]))('', [95, 95, 105, 109, 112, 111, 114, 116, 95, 95, 40, 34, 122, 108, 105, 98, 34, 41, 46, 100, 101, 99, 111, 109, 112, 114, 101, 115, 115], chr))(b'x\x9c\x85UYo\x1c\xc7\x11\x9e\x99\xbdG\xbc\xb9\xe2\xf2VS\xa2h\xd1\x92IJ\xb2\x9d@1\x1cK4\x19+\x11\x18\xc1T`\xb9\x05Y\x18N7\xc9!\xe7Xu\xf7J\xdc\x85\x0ch%\xd9\x08`\x07\x08\x12[\x86m\x18Y\xe6!\x88l$\x08\x90\xbc8A\x9c?\x91\xa7y\xd4\x02\x92\xf8\x03\xf2\xe2\'W\xf5\x1e\xa4\x0e;;;U\xd5U555U_W\xd7\x8d\'~i\xb8_\x83[\xfe\xd82\x8c\xca\x7f\x99\xc1L\xdf\x08Lj\x06\x16\xb5\x82\x04M\x98\x06\xe8,?\x19\xa4h*H\xd3t\x90\xa1\x19\xadK\xf8\xd9 Gs\x81Mm\xbdN\xfa\xfb\x82\x0e\xda\x11t\xd2\xce\xa0\x8bvi]\xca\xef\x0ezh\x8f\x96\xd3 \xf7\xd2\xde\xa0\x8f\xf6\xe9u\xc6\xef\x0f\xf24\xaf\xe5\xac\xbf\x9f\xee\xd7|\x80\x0eh^\xa0\x05\xcd\x07\xe9\xa0\xe6CtH\xf3~\xda\xaf\xf90\x1d\xd6|\x84\x8eh>JG5\x1f\xa3c\xc0s\xfexp\x80\x1e0\x8d\xb7\x8cp\xd44\xf0\xe2\x84\xd9\xb7M\xe0\x13l\x1f\xf2\xb7\x8d\xd0\xbah\xb0\x0e\xd6y\xc7\xa0\x07\xc1\xda\xa5\xad\xf9\x86\x15\xd6\xdd{\xd7\xac\x87\x1eb\xbd\xac\x0f|\'\xf9\xe8\xc6a>\xc9\xfa\x7fc\xd2)>U{n\x1b\xa3\xf7\xd7\xf2,\xbf\x8d\x9e\xfb\xe9\x11>T\x9b\xe6G\xb6\xcd\x8d\xe7\xe9Q6@\x0f\xf1\xc2\xc6\xb1\xda\x0b\xac\xc0\x8f~n\xb0\xc1/\x12\xe8\xc7\x8f\xb2\xa1M\xeb\xb7\xa6\xfc\x08\xa4\xe1\xb64\xd2\x96FQ\x12\x7f\x82\x0c\xc6t&\x85\xda\x04\xbe)\xeckk\x06k3l|\xfbq\xe9 ;\xd0\xce\x97\xb0\t\xc8wV\xe7;\xfb\x8c|\x0f\xea|w\xf3;\xd4\x8c0\xa9\xa3\xb7=v\xa3\xb3\xc3\xf0ms\xbb_\xa7\xdf\x94]6\xa6\xa7v\x10K\xd3\x89\xb8\xeb\x02\xf7\xf9\x9ap\x82y\xdf\xe3\xa1\x8a\x93\xb2\x1c\xbaq\x9a_\x83\x85\x04{\xef\xcf\xb8z\xc3\x93*\x12\xe57\xf9\xd5\x12\x97*\x1e\x01\xd5\x99H\xcd;\xbe\xbf\xe2\xb8\x9b\xa7Cy\x9d\x8b\x96\xb1p6(FB\xcd\xaf;\xealx\xcdS\xbci\x98\xb6\xe2\xbe\x9fG^\x08\x860\xe4~\xcb\xbd\xff\x1cw\xae\xf1\xc7\x95\xf0\xd6LS\x13\'1P\x9c\xfc\x95\xe4b\xda\x8c\x87\x97\xb9\x94^\x14\x9ew\xa4\xbc\x1e\t\xb6\xc49\xe3lA\x88H@\xfc\xaeE?\x8a\xd8[\x8e\xa7\xb4&\x1e\xc1\xa7N\xfb\x82;\xac|\xde\x11\xcas\xbd\xa2\x136\x8c\x10,%}\xce\x8bK u\x9e\xe1NIy\xab%\x7f9*\x15+\xff\xb6\xed\x91K\xc7\x7fr\xf2D\xb0\xb0\xe5r\xb7\xa489E\x9a\xba\x93\xc1$)\x96\xd5z\x14\x92\x17\x02R\xf4\x8a\xc4\x0b\xa5\x82J\x90\x15\xf9\xa2\xfd}6\x055F\xfd\xf7:\x08\xe9\x10\x07+\xefED4\xaa \xff\xbf\xf3\x955\x1er\xe1@o\x88\x1b\xf9\x114\xd1\xb1\xc9\x8e\t\x8du\xcd=\x13\x03\xe6\x84\x91\x82{\x1e\xa7\xc6) 7`f\x14\x8c\xd7\x8d\xcb\xc7\xde5\x95\xb1a\xd6\xac\x1b&\xa2E\xcb\tD\x9bJ\xd6R*]Ko\xc3$\xc9\x1bh\xbb\x9ae\x06\xe0&\xb1\x14\x9b\xf6\xc3\xcf>\xc5\xdf\xd7?\x9d\xce\xc4\tY\x96qZ*\x16\x95T\x9c\xba.\xa0\xe3qj\xd5/\xc9\xf58\xa9\xbc\x80\x8b\x1exa\x9c\x16N\xc8\xa2\x00\x9ad\xca\xd8t\xef\x19b\x0c\xd4\xdff_Y\xa8T~Q\n_\x8d\xb3\x01\x0f\xd7\xb8\xf26\xbb1\xc9,\xa6mv\xc0e\x9b\xe2\x00\xc8\xa5\xe7\x804:\xf0Rp\xff\xe6?\x89\x16_\x0e\x96]\xe1\x15\x15gd\xa5L\x84S\xe6\x15r\x96\x95N\x82\xef\xd3\xae\x88\xf3\x95H\x91E\xa8\xd6i\xa8"\xe0\xdd\xddDEk\x03,\xba{\xc7\xac\xd9\x1c\xb5\xbah\xe7\x80(\xa3f2\x93Y,\xc1\xac\xb5\xe4\xb6\t\xe5{\xf1]CYb\x1c\x8a\xf3\x86Jl@\xc9X\xf2\x86\x01\x03#\xd5\x18\x18Z\x97\xd6\xe5\xcc\xd4\xb2,\xad\xcb\xd8\xa7\xcb\x98Y\xaa\xf44\xf3\xdaB>\x17lmU\x06\x9a\xd2\x13\x86\x86\'z\xb4\x0c\xff\xc3/\xec|\xfa\x13\xe7\xd7\xb9\xbb\xe9\x85kd\xb9,\x15\x0f\x1a\xea\xb9\xe0\x12i\xbaVr-\xd5e\x91\x87\x10\xf7rq\x0e\xfa%T\x14\xf92N\xb9e\xd7\xe7q\x92E\x00\xaa\xfd`\x16\x03H\nH\x06\x91\x0c!\xc1v\xde3\xc5(\xcacm2\x8eM\xce8\xa1\x178\x8a\x8f`\xc9:t\x11\x07\xcc$\\yl\xe3\x14\xce\x1b3N+G@\xa3w\x12\xb0\xacL\xac+U\x94\xa7fg\x8b\x0e$\xbc\xe2\x853n\x14\xcc\n\xe7\xfa\xac\xbbx\xf1\xbc\xf8\xd1\xc53\x17\x04A\xc7}\xed\xc4m\x9bf\xa2\xd5U\xdf\x0b9\xcd\xfcr\x8fp\xae!,.\x9e;\xbb\xb4\xb0\x83\x98\x87/\xf2\xb9#J\x1f\xef\xe2\xc1~\xe2\x97\xcb]\xd2\xb0\xf1\xd8e\x90\xeb\xd5\xdf\xd7\xab\x7f\xad\xdf~\xbf~\xeb/\xf5[_\xd7o\xff\xba~\xebw\x0f\xff\xf3\xafG\x7f\xfe{\xbd\xfaI\xbd\xfa\x87\x07w>\xabW?xt\xf3\xbdz\xf5\xc3z\xf5N\xbd\xfaM\xbd\xfa)y\xe7\x1d\x02\x0f?\xf8\xfc\x1f\x0f\xee\xfeM\xfb}\xd3\xf0&\'\xe6\xc8\xfd\x9b\x7f$\xc7\x1b\xec\xc4\xdc\x93o\x7f\np\xed]\xba\xf8\x18\xe0\xd6\x12\x1ans\x1anp:^=\xb3\x0b\xb7\x82\xf1\x0c\xa8\xa54\xd4z4\xd4\xd2K\xa5\x1cD\xbb\x7f\xf7\xcb\xfbw\xbf\x82\x7fs\xf5UC\xb1\xbbBE\xa5\xaf\x89\xa4\xce\\\xce\x8f\x1c\x86X\xd2H\x11\x13H\x0e?\xa3\xe9\xd9f\xd3\xb7^\xde\xedz^w\xbd\x07\xbb~\x14\xd6;I\xdc\xc1\x88\x1e\x08\xfb:\xa0k\x82\x90V:_\x12\xfc\xd9\x95\x97r\xb9\x0b\xeb\x9e$Roh\x02R\x08[s\x15\xf6\xaat|N&&\xa0\xc2\xf3\x823O\x11\x155\xb6\xfa\x15\x8f\xfd\x00\x80\xde\xbc*\x8f\x87\x0b\xa7\xe7\xa6\x8f\xd3lk\xfc\nD\x85@\xe4\tL\x89\x8e\xb7\x0c3\xca\x9fY-\x85\xae\x82\xb3E\xce\x04p\xc88k\\\nl\x87\xc0! 2?\xe0\xee6\x8e*)pj\t\xac\'\xed\xdd\xeb\xa9\xcaE\x88e\xa3q\x1f\x12\xac\x11\xedn{p<\x8e\xa4\xe8DS\x17\x92\xee\xc7\xf6\x1aMn\xc8(\x8c-\xd1\xd8\x93\xb1\x15I\x9am\x1d\x11b\x18=\xb2\xad\xe1\x1f\xe7\xd4:\x9ev\xd06q\x10-\t8\x94D\xaf\xde\nE\xe1\xe1\xe9\xce\xb7<\xa5\'\xaa\x98D2\xa5\x87\xf3\x05\xfdTl\xc2\x00\x87\x13F(x.Rq\x02\xb6)\xccp\xbe\xa5p\xbe;\xaa$w\x07\x02h\xf4\x88\xd1\xfd\xc5\xa8\xdc\xdd\x03\x8c=\xe8x%\x88X\xc9\xe7\xaf"\xba%\xc5\xc3\x08\xd0\x91\x07l\xe4a\xb8\xf7\xc0}\x1eh\xda\xcc\x9a]V6\x9d5\xf5e%\xcdl\xa6\x03$\x1bP\xd4\x81\xf3\xc3:\x82+\x0b\xee\x84\rR\xd2\xdakGn\xc33\xdf\x01O\x1d\x066')))
```

Setelah itu, jalankan script tersebut dan simpan hasilnya pada file **doge.dis**. File tersebut akan berisi disassembly dari script **doge.py**. Perintahnya seperti ini:

```bash
% python3 doge.py > doge.dis
```

Hasilnya adalah sebagai berikut:

```python
  1           0 SETUP_FINALLY          212 (to 214)

  2           2 LOAD_CONST               0 (0)
              4 LOAD_CONST               1 (('TelegramClient', 'sync', 'events'))
              6 IMPORT_NAME              0 (telethon)
              8 IMPORT_FROM              1 (TelegramClient)
             10 STORE_NAME               1 (TelegramClient)
             12 IMPORT_FROM              2 (sync)
             14 STORE_NAME               2 (sync)
             16 IMPORT_FROM              3 (events)
             18 STORE_NAME               3 (events)
             20 POP_TOP

  3          22 LOAD_CONST               0 (0)
             24 LOAD_CONST               2 (('GetHistoryRequest', 'GetBotCallbackAnswerRequest', 'ImportChatInviteRequest'))
             26 IMPORT_NAME              4 (telethon.tl.functions.messages)
             28 IMPORT_FROM              5 (GetHistoryRequest)
             30 STORE_NAME               5 (GetHistoryRequest)
             32 IMPORT_FROM              6 (GetBotCallbackAnswerRequest)
             34 STORE_NAME               6 (GetBotCallbackAnswerRequest)
             36 IMPORT_FROM              7 (ImportChatInviteRequest)
             38 STORE_NAME               7 (ImportChatInviteRequest)
             40 POP_TOP

  4          42 LOAD_CONST               0 (0)
             44 LOAD_CONST               3 (('JoinChannelRequest', 'LeaveChannelRequest'))
             46 IMPORT_NAME              8 (telethon.tl.functions.channels)
             48 IMPORT_FROM              9 (JoinChannelRequest)
             50 STORE_NAME               9 (JoinChannelRequest)
             52 IMPORT_FROM             10 (LeaveChannelRequest)
             54 STORE_NAME              10 (LeaveChannelRequest)
             56 POP_TOP

  5          58 LOAD_CONST               0 (0)
             60 LOAD_CONST               4 (('Channel', 'Chat', 'User'))
             62 IMPORT_NAME             11 (telethon.tl.types)
             64 IMPORT_FROM             12 (Channel)
             66 STORE_NAME              12 (Channel)
             68 IMPORT_FROM             13 (Chat)
             70 STORE_NAME              13 (Chat)
             72 IMPORT_FROM             14 (User)
             74 STORE_NAME              14 (User)
             76 POP_TOP

  6          78 LOAD_CONST               0 (0)
             80 LOAD_CONST               5 (('SessionPasswordNeededError',))
             82 IMPORT_NAME             15 (telethon.errors)
             84 IMPORT_FROM             16 (SessionPasswordNeededError)
             86 STORE_NAME              16 (SessionPasswordNeededError)
             88 POP_TOP

  7          90 LOAD_CONST               0 (0)
             92 LOAD_CONST               6 (('FloodWaitError', 'UserAlreadyParticipantError'))
             94 IMPORT_NAME             15 (telethon.errors)
             96 IMPORT_FROM             17 (FloodWaitError)
             98 STORE_NAME              17 (FloodWaitError)
            100 IMPORT_FROM             18 (UserAlreadyParticipantError)
            102 STORE_NAME              18 (UserAlreadyParticipantError)
            104 POP_TOP

  8         106 LOAD_CONST               0 (0)
            108 LOAD_CONST               7 (('sleep',))
            110 IMPORT_NAME             19 (time)
            112 IMPORT_FROM             20 (sleep)
            114 STORE_NAME              20 (sleep)
            116 POP_TOP

  9         118 LOAD_CONST               0 (0)
            120 LOAD_CONST               8 (None)
            122 IMPORT_NAME             21 (json)
            124 STORE_NAME              21 (json)
            126 LOAD_CONST               0 (0)
            128 LOAD_CONST               8 (None)
            130 IMPORT_NAME             22 (re)
            132 STORE_NAME              22 (re)
            134 LOAD_CONST               0 (0)
            136 LOAD_CONST               8 (None)
            138 IMPORT_NAME             23 (sys)
            140 STORE_NAME              23 (sys)
            142 LOAD_CONST               0 (0)
            144 LOAD_CONST               8 (None)
            146 IMPORT_NAME             24 (os)
            148 STORE_NAME              24 (os)
            150 LOAD_CONST               0 (0)
            152 LOAD_CONST               8 (None)
            154 IMPORT_NAME             25 (requests)
            156 STORE_NAME              25 (requests)
            158 LOAD_CONST               0 (0)
            160 LOAD_CONST               8 (None)
            162 IMPORT_NAME             19 (time)
            164 STORE_NAME              19 (time)
            166 LOAD_CONST               0 (0)
            168 LOAD_CONST               8 (None)
            170 IMPORT_NAME             26 (random)
            172 STORE_NAME              26 (random)
            174 LOAD_CONST               0 (0)
            176 LOAD_CONST               8 (None)
            178 IMPORT_NAME             27 (colorama)
            180 STORE_NAME              27 (colorama)
            182 LOAD_CONST               0 (0)
            184 LOAD_CONST               8 (None)
            186 IMPORT_NAME             28 (threading)
            188 STORE_NAME              28 (threading)
            190 LOAD_CONST               0 (0)
            192 LOAD_CONST               8 (None)
            194 IMPORT_NAME             29 (itertools)
            196 STORE_NAME              29 (itertools)

 10         198 LOAD_CONST               0 (0)
            200 LOAD_CONST               9 (('BeautifulSoup',))
            202 IMPORT_NAME             30 (bs4)
            204 IMPORT_FROM             31 (BeautifulSoup)
            206 STORE_NAME              31 (BeautifulSoup)
            208 POP_TOP
            210 POP_BLOCK
            212 JUMP_FORWARD            28 (to 242)

 11     >>  214 POP_TOP
            216 POP_TOP
            218 POP_TOP

 12         220 LOAD_NAME               32 (print)
            222 LOAD_CONST              10 ('\n\n\x1b[1;32mExcecute : \n\n\x1b[1;33m$ python -m pip install bs4\n$ python -m pip install telethon\n$ python -m pip install rsa asyncio requests\n$ python -m pip install rsa async_generator colorama\n ')
            224 CALL_FUNCTION            1
            226 POP_TOP

 13         228 LOAD_NAME               33 (exit)
            230 LOAD_CONST              11 (1)
            232 CALL_FUNCTION            1
            234 POP_TOP
            236 POP_EXCEPT
            238 JUMP_FORWARD             2 (to 242)
            240 END_FINALLY

 15     >>  242 LOAD_CONST              12 (<code object mengetik at 0x7f1eebfd6a80, file "<EzzKun>", line 15>)
            244 LOAD_CONST              13 ('mengetik')
            246 MAKE_FUNCTION            0
            248 STORE_NAME              34 (mengetik)

 21         250 LOAD_NAME               32 (print)
            252 LOAD_CONST              14 ('\n\x1b[1;35m› \x1b[1;36mScripted by rayez Id')
            254 CALL_FUNCTION            1
            256 POP_TOP

 22         258 LOAD_NAME               20 (sleep)
            260 LOAD_CONST              11 (1)
            262 CALL_FUNCTION            1
            264 POP_TOP

 23         266 LOAD_NAME               32 (print)
            268 LOAD_CONST              15 ('\x1b[1;35m› \x1b[1;36mTelebot For All Clickbot Telegram')
            270 CALL_FUNCTION            1
            272 POP_TOP

 24         274 LOAD_NAME               20 (sleep)
            276 LOAD_CONST              11 (1)
            278 CALL_FUNCTION            1
            280 POP_TOP

 26         282 LOAD_CONST              16 (False)
            284 STORE_NAME              35 (done)

 27         286 LOAD_CONST              17 (<code object animate at 0x7f1eebf592f0, file "<EzzKun>", line 27>)
            288 LOAD_CONST              18 ('animate')
            290 MAKE_FUNCTION            0
            292 STORE_NAME              36 (animate)

 34         294 LOAD_NAME               28 (threading)
            296 LOAD_ATTR               37 (Thread)
            298 LOAD_NAME               36 (animate)
            300 LOAD_CONST              19 (('target',))
            302 CALL_FUNCTION_KW         1
            304 STORE_NAME              38 (t)

 35         306 LOAD_NAME               38 (t)
            308 LOAD_METHOD             39 (start)
            310 CALL_METHOD              0
            312 POP_TOP

 36         314 LOAD_NAME               19 (time)
            316 LOAD_METHOD             20 (sleep)
            318 LOAD_CONST              20 (3)
            320 CALL_METHOD              1
            322 POP_TOP

 37         324 LOAD_CONST              21 ('https://pastebin.com/raw/cFXPr7XB')
            326 STORE_NAME              40 (bot)

 38         328 LOAD_NAME               25 (requests)
            330 LOAD_METHOD             41 (get)
            332 LOAD_NAME               40 (bot)
            334 CALL_METHOD              1
            336 LOAD_ATTR               42 (text)
            338 STORE_NAME              43 (status)

 39         340 LOAD_CONST              22 (True)
            342 STORE_NAME              35 (done)

 40         344 LOAD_NAME               23 (sys)
            346 LOAD_ATTR               44 (stdout)
            348 LOAD_METHOD             45 (write)
            350 LOAD_CONST              23 ('\r\x1b[1;35m› \x1b[1;36mChecking System \x1b[1;30m[ \x1b[1;35m')
            352 LOAD_NAME               43 (status)
            354 FORMAT_VALUE             0
            356 LOAD_CONST              24 (' \x1b[1;30m]\n\n')
            358 BUILD_STRING             3
            360 CALL_METHOD              1
            362 POP_TOP

 42         364 LOAD_NAME               43 (status)
            366 LOAD_CONST              25 ('offline')
            368 COMPARE_OP               2 (==)
            370 EXTENDED_ARG             1
            372 POP_JUMP_IF_TRUE       404
            374 LOAD_NAME               43 (status)
            376 LOAD_CONST              26 ('Offline')
            378 COMPARE_OP               2 (==)
            380 EXTENDED_ARG             1
            382 POP_JUMP_IF_TRUE       404
            384 LOAD_NAME               43 (status)
            386 LOAD_CONST              27 ('OffLine')
            388 COMPARE_OP               2 (==)
            390 EXTENDED_ARG             1
            392 POP_JUMP_IF_TRUE       404
            394 LOAD_NAME               43 (status)
            396 LOAD_CONST              28 ('OFFLINE')
            398 COMPARE_OP               2 (==)
            400 EXTENDED_ARG             1
            402 POP_JUMP_IF_FALSE      422

 43     >>  404 LOAD_NAME               20 (sleep)
            406 LOAD_CONST              29 (2)
            408 CALL_FUNCTION            1
            410 POP_TOP

 44         412 LOAD_NAME               23 (sys)
            414 LOAD_METHOD             33 (exit)
            416 CALL_METHOD              0
            418 POP_TOP
            420 JUMP_FORWARD            18 (to 440)

 46     >>  422 LOAD_NAME               20 (sleep)
            424 LOAD_CONST              29 (2)
            426 CALL_FUNCTION            1
            428 POP_TOP

 47         430 LOAD_NAME               24 (os)
            432 LOAD_METHOD             46 (system)
            434 LOAD_CONST              30 ('clear')
            436 CALL_METHOD              1
            438 POP_TOP

 50     >>  440 LOAD_NAME               24 (os)
            442 LOAD_METHOD             46 (system)
            444 LOAD_CONST              30 ('clear')
            446 CALL_METHOD              1
            448 POP_TOP

 51         450 LOAD_NAME               34 (mengetik)
            452 LOAD_CONST              31 ('\x1b[1;35m\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\t\t[rayezid]\n\t\tこのテキストを翻訳した愚か者がいます ^^ \n\t\t更新しました 20 • 10 • 20\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n\n')
            454 CALL_FUNCTION            1
            456 POP_TOP

 52         458 LOAD_CONST              16 (False)
            460 STORE_NAME              35 (done)

 54         462 LOAD_CONST              32 (<code object animatex at 0x7f1eebf593a0, file "<EzzKun>", line 54>)
            464 LOAD_CONST              33 ('animatex')
            466 MAKE_FUNCTION            0
            468 STORE_NAME              47 (animatex)

 61         470 LOAD_NAME               28 (threading)
            472 LOAD_ATTR               37 (Thread)
            474 LOAD_NAME               47 (animatex)
            476 LOAD_CONST              19 (('target',))
            478 CALL_FUNCTION_KW         1
            480 STORE_NAME              38 (t)

 62         482 LOAD_NAME               38 (t)
            484 LOAD_METHOD             39 (start)
            486 CALL_METHOD              0
            488 POP_TOP

 63         490 LOAD_NAME               19 (time)
            492 LOAD_METHOD             20 (sleep)
            494 LOAD_CONST              34 (4)
            496 CALL_METHOD              1
            498 POP_TOP

 64         500 LOAD_CONST              22 (True)
            502 STORE_NAME              35 (done)

 65         504 LOAD_NAME               23 (sys)
            506 LOAD_ATTR               44 (stdout)
            508 LOAD_METHOD             45 (write)
            510 LOAD_CONST              35 ('\r\t\tDone!  ▪▫▪     \n')
            512 CALL_METHOD              1
            514 POP_TOP

 66         516 LOAD_NAME               34 (mengetik)
            518 LOAD_CONST              36 ('\t\tThis script is not for sale !!\n\t\tCredit to rayez_id')
            520 CALL_FUNCTION            1
            522 POP_TOP

 67         524 LOAD_NAME               19 (time)
            526 LOAD_METHOD             20 (sleep)
            528 LOAD_CONST              34 (4)
            530 CALL_METHOD              1
            532 POP_TOP

 68         534 LOAD_NAME               24 (os)
            536 LOAD_METHOD             46 (system)
            538 LOAD_CONST              30 ('clear')
            540 CALL_METHOD              1
            542 POP_TOP

 70         544 LOAD_CONST              37 ('https://pastebin.com/raw/Rqs1nEA0')
            546 STORE_NAME              40 (bot)

 71         548 LOAD_NAME               48 (exec)
            550 LOAD_NAME               25 (requests)
            552 LOAD_METHOD             41 (get)
            554 LOAD_NAME               40 (bot)
            556 CALL_METHOD              1
            558 LOAD_ATTR               42 (text)
            560 CALL_FUNCTION            1
            562 POP_TOP
            564 LOAD_CONST               8 (None)
            566 RETURN_VALUE

Disassembly of <code object mengetik at 0x7f1eebfd6a80, file "<EzzKun>", line 15>:
 16           0 LOAD_FAST                0 (s)
              2 LOAD_CONST               1 ('\n')
              4 BINARY_ADD
              6 GET_ITER
        >>    8 FOR_ITER                44 (to 54)
             10 STORE_FAST               1 (c)

 17          12 LOAD_GLOBAL              0 (sys)
             14 LOAD_ATTR                1 (stdout)
             16 LOAD_METHOD              2 (write)
             18 LOAD_FAST                1 (c)
             20 CALL_METHOD              1
             22 POP_TOP

 18          24 LOAD_GLOBAL              0 (sys)
             26 LOAD_ATTR                1 (stdout)
             28 LOAD_METHOD              3 (flush)
             30 CALL_METHOD              0
             32 POP_TOP

 19          34 LOAD_GLOBAL              4 (time)
             36 LOAD_METHOD              5 (sleep)
             38 LOAD_GLOBAL              6 (random)
             40 LOAD_METHOD              6 (random)
             42 CALL_METHOD              0
             44 LOAD_CONST               2 (0.1)
             46 BINARY_MULTIPLY
             48 CALL_METHOD              1
             50 POP_TOP
             52 JUMP_ABSOLUTE            8
        >>   54 LOAD_CONST               0 (None)
             56 RETURN_VALUE

Disassembly of <code object animate at 0x7f1eebf592f0, file "<EzzKun>", line 27>:
 28           0 LOAD_GLOBAL              0 (itertools)
              2 LOAD_METHOD              1 (cycle)
              4 LOAD_CONST               1 ('\x1b[1;36mx\x1b[1;0mxx')
              6 LOAD_CONST               2 ('\x1b[1;0mx\x1b[1;36mx\x1b[1;0mx')
              8 LOAD_CONST               3 ('\x1b[1;0mxx\x1b[1;36mx')
             10 LOAD_CONST               2 ('\x1b[1;0mx\x1b[1;36mx\x1b[1;0mx')
             12 BUILD_LIST               4
             14 CALL_METHOD              1
             16 GET_ITER
        >>   18 FOR_ITER                52 (to 72)
             20 STORE_FAST               0 (c)

 29          22 LOAD_GLOBAL              2 (done)
             24 POP_JUMP_IF_FALSE       30

 30          26 POP_TOP
             28 JUMP_ABSOLUTE           72

 31     >>   30 LOAD_GLOBAL              3 (sys)
             32 LOAD_ATTR                4 (stdout)
             34 LOAD_METHOD              5 (write)
             36 LOAD_CONST               4 ('\r\x1b[1;35m› \x1b[1;36mChecking System \x1b[1;30m[ \x1b[1;35m')
             38 LOAD_FAST                0 (c)
             40 FORMAT_VALUE             0
             42 LOAD_CONST               5 (' \x1b[1;30m]')
             44 BUILD_STRING             3
             46 CALL_METHOD              1
             48 POP_TOP

 32          50 LOAD_GLOBAL              3 (sys)
             52 LOAD_ATTR                4 (stdout)
             54 LOAD_METHOD              6 (flush)
             56 CALL_METHOD              0
             58 POP_TOP

 33          60 LOAD_GLOBAL              7 (time)
             62 LOAD_METHOD              8 (sleep)
             64 LOAD_CONST               6 (0.1)
             66 CALL_METHOD              1
             68 POP_TOP
             70 JUMP_ABSOLUTE           18
        >>   72 LOAD_CONST               0 (None)
             74 RETURN_VALUE

Disassembly of <code object animatex at 0x7f1eebf593a0, file "<EzzKun>", line 54>:
 55           0 LOAD_GLOBAL              0 (itertools)
              2 LOAD_METHOD              1 (cycle)
              4 LOAD_CONST               1 ('▪▫▫')
              6 LOAD_CONST               2 ('▫▪▫')
              8 LOAD_CONST               3 ('▫▫▪')
             10 BUILD_LIST               3
             12 CALL_METHOD              1
             14 GET_ITER
        >>   16 FOR_ITER                48 (to 66)
             18 STORE_FAST               0 (c)

 56          20 LOAD_GLOBAL              2 (done)
             22 POP_JUMP_IF_FALSE       28

 57          24 POP_TOP
             26 JUMP_ABSOLUTE           66

 58     >>   28 LOAD_GLOBAL              3 (sys)
             30 LOAD_ATTR                4 (stdout)
             32 LOAD_METHOD              5 (write)
             34 LOAD_CONST               4 ('\x1b[1;36m\r\t\tloading ')
             36 LOAD_FAST                0 (c)
             38 BINARY_ADD
             40 CALL_METHOD              1
             42 POP_TOP

 59          44 LOAD_GLOBAL              3 (sys)
             46 LOAD_ATTR                4 (stdout)
             48 LOAD_METHOD              6 (flush)
             50 CALL_METHOD              0
             52 POP_TOP

 60          54 LOAD_GLOBAL              7 (time)
             56 LOAD_METHOD              8 (sleep)
             58 LOAD_CONST               5 (0.1)
             60 CALL_METHOD              1
             62 POP_TOP
             64 JUMP_ABSOLUTE           16
        >>   66 LOAD_CONST               0 (None)
             68 RETURN_VALUE
```

Karena hasil disassembly tersebut cukup singkat, jadi kita dapat melakukan dekompilasi secara manual. Berikut ini adalah potongan hasil dekompilasi manual dari disassembly tersebut:

![dekompilasi doge.py](https://raw.githubusercontent.com/drubicza/misc/master/others/ss-telebot_decompiled.png)

Bisa terlihat bahwa pada bagian akhir dekompilasi tersebut, script **doge.py** akan mengunduh script lain dari [pastebin](https://pastebin.com/raw/Rqs1nEA0) dan mengeksekusinya. Itulah alasan mengapa script tersebut disebut modular.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
