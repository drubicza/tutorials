# Dekompilasi Script Enc

## Pendahuluan

Tutorial kali ini kembali akan membahas mengenai proses dekompilasi python bytecode. Untuk contoh kali ini ada sebuah script bernama **enc.py** yang dapat [diunduh pada repositori ini](). Berikut ini adalah langkah-langkahnya.


## Langkah-langkah

* Pertama, periksa dulu tipe file tersebut:

```bash
% file enc.pyc
enc.pyc: python 2.7 byte-compiled
```

* Bisa terlihat, bahwa file tersebut merupakan python bytecode dengan versi python 2.7. Selanjutnya kita akan coba dekompilasi menggunakan [uncompyle6](https://github.com/rocky/python-uncompyle6):

```bash
% uncompyle6 enc.pyc
Unknown type 21
Unknown type 81 Q
Unknown type 47 /
Unknown type 75 K
Traceback (most recent call last):
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/load.py", line 297, in load_module_from_file_object
    co = xdis.unmarshal.load_code(fp, magic_int, code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 111, in load_code
    code = load_code_internal(fp, magic_int, code_objects=code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 605, in load_code_internal
    return UNMARSHAL_DISPATCH_TABLE[marshalType](
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 561, in t_code
    code = load_code_type(fp, magic_int, bytes_for_s=False, code_objects=code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 182, in load_code_type
    co_cellvars = load_code_internal(fp, magic_int, code_objects=code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 605, in load_code_internal
    return UNMARSHAL_DISPATCH_TABLE[marshalType](
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 576, in t_object_reference
    o = internObjects[refnum]
IndexError: list index out of range
Traceback (most recent call last):
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/load.py", line 297, in load_module_from_file_object
    co = xdis.unmarshal.load_code(fp, magic_int, code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 111, in load_code
    code = load_code_internal(fp, magic_int, code_objects=code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 605, in load_code_internal
    return UNMARSHAL_DISPATCH_TABLE[marshalType](
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 561, in t_code
    code = load_code_type(fp, magic_int, bytes_for_s=False, code_objects=code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 182, in load_code_type
    co_cellvars = load_code_internal(fp, magic_int, code_objects=code_objects)
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 605, in load_code_internal
    return UNMARSHAL_DISPATCH_TABLE[marshalType](
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/unmarshal.py", line 576, in t_object_reference
    o = internObjects[refnum]
IndexError: list index out of range

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/thanos/.local/bin/uncompyle6", line 8, in <module>
    sys.exit(main_bin())
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/bin/uncompile.py", line 193, in main_bin
    result = main(src_base, out_base, pyc_paths, source_paths, outfile,
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/main.py", line 316, in main
    deparsed = decompile_file(
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/main.py", line 183, in decompile_file
    (version, timestamp, magic_int, co, is_pypy, source_size, sip_hash) = load_module(
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/load.py", line 163, in load_module
    return load_module_from_file_object(
  File "/home/thanos/.local/lib/python3.8/site-packages/xdis/load.py", line 306, in load_module_from_file_object
    raise ImportError(
ImportError: Ill-formed bytecode file enc.pyc
<class 'IndexError'>; list index out of range
```

* Menggunakan [pycdc](https://github.com/zrax/pycdc) pun hasilnya sama:

```bash
% pycdc enc.pyc
CreateObject: Got unsupported type 0x15
Error loading file enc.pyc: std::bad_cast
```

* Coba kita perhatikan script tersebut menggunakan **hexdump** :

```bash
% hexdump -C enc.pyc | head
00000000  03 f3 0d 0a 00 00 00 00  63 5e ea eb ec 50 4b 03  |........c^...PK.|
00000010  04 14 00 00 00 08 00 d9  6a 15 51 af da 96 30 b3  |........j.Q...0.|
00000020  1c 00 00 2f aa 01 00 0c  00 1c 00 5f 5f 6d 61 69  |.../.......__mai|
00000030  6e 5f 5f 2e 70 79 63 55  54 09 00 03 3a 68 3f 5f  |n__.pycUT...:h?_|
00000040  3a 68 3f 5f 75 78 0b 00  01 04 d5 27 00 00 04 d5  |:h?_ux.....'....|

..snip..

00001d20  01 00 0c 00 18 00 00 00  00 00 00 00 00 00 80 81  |................|
00001d30  00 00 00 00 5f 5f 6d 61  69 6e 5f 5f 2e 70 79 63  |....__main__.pyc|
00001d40  55 54 05 00 03 3a 68 3f  5f 75 78 0b 00 01 04 d5  |UT...:h?_ux.....|
00001d50  27 00 00 04 d5 27 00 00  50 4b 05 06 00 00 00 00  |'....'..PK......|
00001d60  01 00 01 00 52 00 00 00  f9 1c 00 00 00 00        |....R.........|
```

* Jika memperhatikan hexdump di atas, bisa terlihat bahwa file tersebut memiliki _header_ yang umumnya ditemukan pada kompresi _zip_. Oleh karena itu, kita akan mencoba melihatnya menggunakan aplikasi **unzip** atau **7zip**:

```bash
% unzip -l enc.pyc
Archive:  enc.pyc
warning [enc.pyc]:  13 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  Length      Date    Time    Name
---------  ---------- -----   ----
   109103  08-21-2020 14:22   __main__.pyc
---------                     -------
   109103                     1 file
```

* Ternyata benar, file tersebut menggunakan kompresi _zip_. Selanjutnya kita ekstrak file `__main__.pyc` di dalam arsip tersebut:

```bash
% unzip enc.pyc
Archive:  enc.pyc
warning [enc.pyc]:  13 extra bytes at beginning or within zipfile
  (attempting to process anyway)
  inflating: __main__.pyc
```

* Selanjutnya kita dekompilasi file `__main__.pyc` tersebut menggunakan [uncompyle6](https://github.com/rocky/python-uncompyle6):

```bash
% uncompyle6 __main__.pyc
# uncompyle6 version 3.7.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 3.8.5 (default, Aug 12 2020, 00:00:00)
# [GCC 10.2.1 20200723 (Red Hat 10.2.1-1)]
# Embedded file name: enc_p2
# Compiled at: 2020-08-21 14:22:49

maximum recursion depth exceeded while calling a Python object


Last file: __main__.pyc   Traceback (most recent call last):
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 844, in buildTree
    attr[i] = self.buildTree(sym, why[0],
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 844, in buildTree
    attr[i] = self.buildTree(sym, why[0],
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 844, in buildTree
    attr[i] = self.buildTree(sym, why[0],
  [Previous line repeated 989 more times]
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 847, in buildTree
    return self.rule2func[self.new2old[rule]](attr)
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 1044, in <lambda>
    self.buildASTNode(args, lhs)
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 1055, in buildASTNode
    return self.nonterminal(lhs, children)
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/parser.py", line 254, in nonterminal
    rv = GenericASTBuilder.nonterminal(self, nt, args)
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/spark.py", line 1061, in nonterminal
    rv = self.AST(type)
  File "/home/thanos/.local/lib/python3.8/site-packages/uncompyle6/parsers/treenode.py", line 12, in __init__
    super(SyntaxTree, self).__init__(*args, **kwargs)
  File "/home/thanos/.local/lib/python3.8/site-packages/spark_parser/ast.py", line 16, in __init__
    UserList.__init__(self, kids)
  File "/usr/lib64/python3.8/collections/__init__.py", line 1061, in __init__
    if type(initlist) == type(self.data):
RecursionError: maximum recursion depth exceeded while calling a Python object
```

* Ternyata proses kompilasinya error. Coba kita gunakan [pycdc](https://github.com/zrax/pycdc):

```bash
% pycdc __main__.pyc
# Source Generated with Decompyle++
# File: __main__.pyc (Python 2.7)

'''
===================================
   Obfuscate by Khairul Syabana
    Jangan diedit nanti error
===================================
'''
Nolep = True
if Nolep == False:
zsh: segmentation fault (core dumped)  pycdc __main__.pyc
```

* Ternyata error juga. Karena proses dekompilasi selalu gagal, kita akan mencoba men-disassemble file tersebut menggunakan [pycdas](https://github.com/zrax/pycdc) dan menyimpan hasilnya ke file **enc.dis**:

```bash
% pycdas __main__.pyc > enc.dis
```

* Buka file `enc.dis` yang dihasilkan pada langkah di atas menggunakan text editor. Bagian pertama yang perlu diperhatikan adalah bagian ini:

```python
0       LOAD_CONST              1: 'x\x9c\x85\x9c\xe9\x92\xec:\n\x84\x9f\xbd\xb6 ... \x9b\x9f\xd1\xbf\x802\xbfs\xfa\x95\xd2\xff\xff\x0fn}P\xff'
                                3       RETURN_VALUE
                    [Disassembly]
                        0       LOAD_CONST              1: <CODE> <lambda>
                        3       MAKE_FUNCTION           0
                        6       CALL_FUNCTION           0
                        9       RETURN_VALUE
                'zlib'
                'cp1026'
            [Disassembly]
                0       LOAD_CONST              1: <CODE> <lambda>
                3       MAKE_FUNCTION           0
                6       CALL_FUNCTION           0
                9       LOAD_ATTR               0: decode
                12      LOAD_CONST              2: 'zlib'
                15      CALL_FUNCTION           1
                18      LOAD_ATTR               0: decode
                21      LOAD_CONST              3: 'cp1026'
                24      CALL_FUNCTION           1
                27      RETURN_VALUE
```

* Bagian di atas jika didekompilasi secara manual, maka hasilnya kurang lebih seperti ini:

```python
'x\x9c\x85\x9c\xe9\x92\xec:\n\x84\x9f\xbd\xb6 ... \x9b\x9f\xd1\xbf\x802\xbfs\xfa\x95\xd2\xff\xff\x0fn}P\xff'.decode('zlib').decode('cp1026')
```

* Kita cukup menambahkan `print` untuk melihat hasilnya:

```python
print 'x\x9c\x85\x9c\xe9\x92\xec:\n\x84\x9f\xbd\xb6 ... \x9b\x9f\xd1\xbf\x802\xbfs\xfa\x95\xd2\xff\xff\x0fn}P\xff'.decode('zlib').decode('cp1026')
```

* Hasilnya jika dijalankan maka akan seperti ini:

```python
'696d706f7274206d61727368616c0a6865 ... 29292829292829292829'
```

* Jika diperhatikan baik-baik, maka bisa terlihat bahwa _string_ tersebut menggunakan _encode_ hexadesimal. Jadi kita tambahkan _encode_ hexadesimal setelah 2 encoding sebelumnya, menjadi seperti ini:

```python
print 'x\x9c\x85\x9c\xe9\x92\xec:\n\x84\x9f\xbd\xb6 ... \x9b\x9f\xd1\xbf\x802\xbfs\xfa\x95\xd2\xff\xff\x0fn}P\xff'.decode('zlib').decode('cp1026').decode('hex')
```

* Nah, sekarang hasilnya adalah potongan script berikut ini:

```python
import marshal
hentai,ecchi=None,None
exec (lambda:(lambda:(lambda:compile('x\x9c\x8dX\xcbv\xb2J ... \xfe\x07\xf9\x0e7\xf2'.decode("zlib").decode("base64"),"Khairul Syabana","exec"))())())()
```

* Dari script di atas, kita cukup mengambil _string_ yang menjadi parameter untuk pemanggilan fungsi _compile_ dan menambahkan fungsi _print_ menjadi seperti ini:

```python
print 'x\x9c\x8dX\xcbv\xb2J ... \xfe\x07\xf9\x0e7\xf2'.decode("zlib").decode("base64")
```

* Jalankan kode tersebut, maka hasilnya kurang lebih seperti ini:

```python
pypiwLNGqeU=662^608
bssFurTJWco=812^726
DNTFHVAqswZ=729^726
LBPmFngWlGD=149*86
NjjzDoMgkMl=106^72
tRtOSrsmhfD=127*35
AXwcIPplMRS=351*299
ykRTfiEtHId=485*451
oOeEoxCExfF=842*800
TrjJlFoaFDn=986*952
aJAzLrensRz=775^733
tJIBnjuYXdB=208^165
jwndmCxqoph=909^901
omMwPzXwAMg=99*28
MRsRYIFtJkQ=588^557
fHxpDZyCeOs=199*193
scCYyQhjXmm=885*790
mhFoqRqgnyt=153*72
IakpHjQXZvv=276*227
SfcArBXVQas=589*564

nolep,sadboy=0,0
piton=None
doujin=[]
code=(lambda:(lambda:(lambda:'.,:\x80~}:mty:{\x84\x7fszy=\x15tx{z}\x7f+~\x84~7+}lyozx\x15tx{z}\x7f+\x85wtm7+xl}~slw\x15q}zx+{\x84jnzx{twp+tx{z}\x7f+nzx{twp+l~+jnzx{twp\x15~\x7f}tyrjl~ntt+H+2\\bP]_d`TZ[L^OQRSUVWecNaMYX|\x82p}\x7f\x84\x80tz{l~oqrsuvw\x85\x83n\x81myx2\x15yz\x7fp+H2---gyHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHgy+++Zmq\x80~nl\x7fp+m\x84+Vslt}\x80w+^\x84lmlylgy++++Ulyrly+otpot\x7f+yly\x7ft+p}}z}gyHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHgy---2\x15\x15\x15opq+pyn}\x84{\x7f3nzop4E\x15\x14l+H+229uzty3ns}3\x834+qz}+\x83+ty+fty\x7f3z}o3t46<<4+qz}+t+ty+nzoph4\x15\x14m+H+22\x15\x14Q+H+fh\x15\x14qz}+\x83+ty+}lyrp3=;4E\x15\x14\x14n+H+229uzty3}lyozx9nsztnp3~\x7f}tyrjl~ntt4+qz}+\x83+ty+}lyrp3<<44\x15\x14\x14o+H+}lyozx9}lyoty\x7f3;7+<;;;4\x15\x14\x14p+H+}lyozx9}lyoty\x7f3;7+<;;4\x15\x14\x14q+H+}lyozx9nsztnp3wt~\x7f32i5244\x15\x14\x14m+6H+~\x7f}3n62H26~\x7f}3o46q6~\x7f}3o8p462gy24\x15\x14\x14Q9l{{pyo3n4\x15\x14pyn}t{\x7fpo+H+}p{}3\x85wtm9nzx{}p~~32tx{z}\x7f+xl}~slwgyspy\x7flt7pnnstHYzyp7Yzypgyp\x83pn+3wlxmolE3wlxmolE3wlxmolEnzx{twp3\x86\x889opnzop3-\x85wtm-49opnzop3-ml~pA?-47-Vslt}\x80w+^\x84lmlyl-7-p\x83pn-443443443429qz}xl\x7f3}p{}33m62gyyzwp{7~lomz\x84H;7;gy{t\x7fzyHYzypgyoz\x80utyHfhgynzopH3wlxmolE3wlxmolE3wlxmolE26}p{}3l462434434434249pynzop32ml~pA?249pynzop32\x85wtm24449pynzop32sp\x83249pynzop32n{<;=A2444\x15\x14wzw+H+2z{{ltHty\x7f33\x86\x8845;46ty\x7f3p\x81lw3-g\x83@?g\x83B=g\x83B@g\x83A@-445<<gyypypyH3wlxmolE3wlxmolE3wlxmolEnzx{twp3--9uzty3ns}3ty\x7f3t8p\x81lw3-g\x83Aqg\x83B;g\x83B;g\x83A<g\x83AD-444+qz}+t+ty+3wlxmolE3wlxmolE3wlxmolEfz}o3\x834+qz}+\x83+ty+p\x81lw3-g\x83A>g\x83Aqg\x83A?g\x83A@-4h43443443447-\x83^ZO\x83-7-p\x83pn-4434434434gyoz\x80uty9l{{pyo3-~lrt}t-4gyp\x83pn+ypypygyopw+Q7nzop7xl}~slw7z{{lt7oz\x80uty7pnnst7spy\x7flt7ypypy7{t\x7fzy7yzwp{7~lomz\x8429qz}xl\x7f32629uzty3Q44fEE8<h9pynzop32}z\x7f<>249pynzop32n{@;;24\x15\x14}p\x7f\x80}y+yz\x7fp6222\x15Yzwp{+H+_}\x80p\x15tq+Yzwp{+HH+Qlw~pE\x15\x14p\x83pn+~\x7f}3ns}3>@4\x86wzw\x884\x15z{{ltH3wlxmolE3wlxmolE3wlxmolE3\x86;\x8844344349opnzop3-\x85wtm-49opnzop3-n{<;=A-4434\x15tq+z{{ltE\x15\x14pnnstH3wlxmolE3Qlw~p4434\x15\x14spy\x7fltH3wlxmolE3Qlw~p4434\x15\x14p\x83pn+p\x81lw3-g\x83Aqg\x83B;g\x83B;g\x83A<g\x83AD-49opnzop3-sp\x83-4\x15pw~pE\x15\x14pnnstH3wlxmolE3_}\x80p4434\x15\x14spy\x7fltH3wlxmolE3_}\x80p4434\x15\x14p\x83pn+~\x7f}3~l\x84lrly~4\x15tq+spy\x7flt+lyo+pnnstHHQlw~pE\x15\x14{t\x7fzyHQlw~p\x15\x14ypypyHYzyp\x15\x14}prp\x83+H+3wlxmol+\x83E}p9qtyolww3}-lxl\x7fp}l~\x80g33964g4-7\x83443sly\x7f\x804\x15\x14p\x81lw3nzx{twp33--9uzty3ns}3\x834+qz}+\x83+ty+\x86=\x8846}prp\x839opnzop3-sp\x83-449opnzop3-n{@;;-47-J-7-p\x83pn-44\x15pw~pE\x15\x14{t\x7fzyH_}\x80p\x15\x14ypypyH3wlxmolE3wlxmolE3wlxmolE\x86<\x88434434434\x15\x14p\x81lw3xl}~slw9wzlo~3p\x81lw3-g\x83Apg\x83A@g\x83Apg\x83A@g\x83Ap-4442229qz}xl\x7f3pyn}t{\x7fpo7+}p{}3xl}~slw9o\x80x{~3nzx{twp32tq+{t\x7fzyHH_}\x80pEgyg\x7fz{{ltHypypygyg\x7fypypyHz{{ltgyg\x7fQH}p{}3xl}~slw9o\x80x{~3z{{lt6ypypy44gyg\x7fp\x83pn+26}p{}3wzw4629opnzop3-n{@;;-49opnzop3-}z\x7f<>-4fEE8<h27+2\x83^ZO\x8327+2p\x83pn24447+~\x7f}3f}lyozx9}lyo}lyrp3;7+=@A4+qz}+\x83+ty+}lyrp3@;4h47+wzwH26~\x7f}3;425<;;;;4\x15\x15opq+xlty3qtwp4E\x15\x14\x7f}\x84E\x15\x14\x14~n+H+z{py3qtwp49}plo34\x15\x14p\x83np{\x7f+TZP}}z}E\x15\x14\x14~\x84~9p\x83t\x7f32qtwp+yz\x7f+qz\x80yo+,,24\x15\x14z\x80\x7fq+H+2pynj26qtwp9}p{wlnp32:27+2K24\x15\x14q+H+z{py3z\x80\x7fq7+2\x82m24\x15\x14q9\x82}t\x7fp3pyn}\x84{\x7f3~n44\x15\x14q9~ppv3wpy3~n44\x15\x14q9nwz~p34\x15\x14jnzx{twp3z\x80\x7fq7+z\x80\x7fq4\x15\x14\x82t\x7fs+z{py32jjxltyjj9{\x84n27+2\x8224+l~+qE\x15\x14\x14q9\x82}t\x7fp3z{py3z\x80\x7fq49}plo344Fq9nwz~p34\x15\x14tx{z}\x7f+z~\x15\x14z~9~\x84~\x7fpx32\x85t{+\x83~zo\x839\x85t{+\x86;\x8829qz}xl\x7f32jjxltyjj9{\x84n244\x15\x14}p~\x80w\x7f+H+z{py32\x83~zo\x839\x85t{249}plo34\x15\x14\x82t\x7fs+z{py3z\x80\x7fq7+2\x8224+l~+qE\x15\x14\x14q9\x82}t\x7fp32\x86;\x88nig\x83plg\x83pmg\x83pn\x86<\x8829qz}xl\x7f3jjtx{z}\x7fjj32tx{249rp\x7fjxlrtn3462g;25?7+}p~\x80w\x7f44Fq9nwz~p34\x15\x14z~9}pxz\x81p32\x83~zo\x839\x85t{24\x15\x14z~9}pxz\x81p32jjxltyjj9{\x84n24\x15\x14{}ty\x7f+2qtwp+~l\x81po+26z\x80\x7fq\x15\x15tq+jjylxpjj+HH+2jjxltyjj2E\x15\x14tq+wpy3~\x84~9l}r\x814+IH+=E\x15\x14\x14xlty3~\x84~9l}r\x81f<h4\x15\x14pw~pE\x15\x14\x14~\x84~9p\x83t\x7f32`~lrpE+26jjqtwpjj62+GqtwpylxpI24\x15')())())()
```

* Selanjutnya, kita akan melakukan dekompilasi pada bagian ke-2 file disassembly `enc.dis`, yaitu bagian ini:

```python
0       LOAD_CONST              1: "c\x00\x00\x00\x00\x00\x00\x00\x00\x04\x00\x00\x00@\x00\x00\x00s_\x00\x00\x00e\x00\x00e\x01\x00k\x02\x00r[\x00e\x02\x00Z\x03\x00e\x03\x00Z\x02\x00e\x04\x00e\x05\x00j\x06\x00e\x03\x00e\x02\x00\x17\x83\x01\x00\x83\x01\x00Z\x07\x00d\x00\x00j\x08\x00d\x01\x00\x83\x01\x00j\x08\x00d\x02\x00\x83\x01\x00d\x03\x00d\x03\x00d\x04\x00\x85\x03\x00\x19d\x03\x00\x04Un\x00\x00d\x03\x00S(\x05\x00\x00\x00s'\x02\x00\x00\x93\x82\x96\x98\x95\x86k\x83\x99\xa8\x82\x81k\x81\x82\x87\xa5\x83k\x81\x99\x81\x99\x81k\xa5\x95\x87\x81\x99\xa4k\xa5\xa4\x97\x97\x99k\x81\xa5\xa6\x88\x82\x98k\xa5\x95\x83\x83\x82k\xa8\x95\xa4\x86\x85\x95\xa9k\x99\x98\x82\x97k\xe2@\xa8\x99\x98%\x81\x99\x81\x99\x81@\x97\x99\x92\x99%]\x7f\xa5\x85\xa5\xa3\x95\x86\x7fM\x98\x81\x99\x83\x83\x95K\x81\xa5\xa6\x88\x82\x98%]M]]M]]M]]\x7f\x97\x99\x92\x99\x7fk\x7f\x92\xd8\xc2\xc6\x92\x7fk]]M]]M]]M]Z]\x7f\x99\x98\x82\x97\x7fM\xa8\x95\x89\x99@\x81\xa5@\x92@\x85\x82\xa2@]\x92M\x98\x85\x82Jz\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8M@\x81\xa5@\xa5@\x85\x82\xa2@]]]\x7f\xa5\x95\x83\x83\x82\x7fM\xa8\x95\x89\x99`\xa5M\x87\x81\xa5M\x85\xa4\x97M\x81\xa5\x82\xa6K\x7f\x7fM\x99\xa8\xa5\x83\xa9\x82\x97z\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8M~\x81\x99\x81\x99\x81%\xf1\xf1\\]]\x7f\x99\x88\x85\xc7\x7fM\xa8\x95\x89\x99M\x87\x81\xa5N]\xf0\\]\x86\x95\xc4\xc9\xd2\xd6\x85\xd5\x97\xa2\xc6N\x89\x89\xd4\xd2\xc4\xa6\xe4\x83\xa7\x95\xe5N\x87\x93\x81\xa3\x84\xc5\x84\x82\xe2\xa4\xa9N\xa9\xa9\xd2\xa6\xa4\xc4\x93\xd3\xd7\x97\x86N\x86\xc2\x99\xd7\x93\xd4\xd8\x83\x92\xe4\xa2N\xc4\xa7\xe6\x87\xe2\xe5\xd3\xc5\x86\xc5\xe9N\xa3\xe9\xd5\x91\xd2\x94\xc3\x91\xe9\xa9\x82N\xa4\x83\x82\x84\x92\xd7\xa9\x98\x81\x91\xa6N\xd6\x98\xd2\xd3\x88\xa6\x81\xd6\xe5\xe6\x87N\x94\xc5\x86\x81\x99\x85\xe8\x94\xd5\xe6\x95N\x81\xd8\xe2\x95\x82\xe2\xa8\xe6\xa6\x85\xc7N\xe2\xa2\x92\xd9\xd7\x92\x82\xd9\x99\xc2\x82N\x98\xe5\xe4\x87\xd9\xa5\xa2\xc7\xc5\xa7\x93N\xc6\xc5\xe9\xa8\x83\xc3\xe5\x97\x91\xd2\xd5N\xd8\xa2\xa4\xa9\x86\x85\xc6\xc2\x87\xc5\x87N\xa8\xe9\xa7\xa3\xe9\x82\xd8\x94\xa6\xa6\xc1N\xd8\xe3\xa8\xd1\xa3\x81\xe2\xa9\xc3\xd6\xe8N\xd4\x91\x86\x84\xd5\xc9\xe4\xe2\xc7\xc1\xd8N\x82\x97\xd1\xe6\xc7\x85\x88\xe2\x86\x86\x96N\xc8\x99\x84\xe3\xc1\xe8\x91\xa5\x83\x93\x83MM\x87\x81\xa5~\xa5\x95\x83\x83\x82t\x05\x00\x00\x00cp500t\x05\x00\x00\x00rot13Ni\xff\xff\xff\xff(\t\x00\x00\x00t\x05\x00\x00\x00pitont\x04\x00\x00\x00Truet\x05\x00\x00\x00nenent\x05\x00\x00\x00oppait\x04\x00\x00\x00reprt\x07\x00\x00\x00marshalt\x05\x00\x00\x00dumpst\x01\x00\x00\x00Ft\x06\x00\x00\x00decode(\x00\x00\x00\x00(\x00\x00\x00\x00(\x00\x00\x00\x00s\x05\x00\x00\x00xSODxt\x08\x00\x00\x00<module>\x01\x00\x00\x00s\x08\x00\x00\x00\x0c\x01\x06\x01\x06\x01\x19\x01"
                                3       RETURN_VALUE
                    [Disassembly]
                        0       LOAD_CONST              1: <CODE> <lambda>
                        3       MAKE_FUNCTION           0
                        6       CALL_FUNCTION           0
                        9       RETURN_VALUE
```

* Bagian di atas dapat kita dekompilasi menggunakan [uncompyle6](https://github.com/rocky/python-uncompyle6). Kita hanya perlu menambahkan beberapa fungsi sehingga menjadi seperti ini:

```python
import sys
import marshal
import uncompyle6

uncompyle6.main.decompile(2.7, marshal.loads("c\x00\x00\x00\x00\x00 ... \x06\x01\x06\x01\x19\x01"), sys.stdout)
```

* Jika kode di atas dijalankan, maka hasilnya kurang lebih seperti ini:

```python
# uncompyle6 version 3.7.3
# Python bytecode 2.7
# Decompiled from: Python 2.7.18 (default, Jul 20 2020, 00:00:00)
# [GCC 10.1.1 20200507 (Red Hat 10.1.1-1)]
# Embedded file name: xSODx
if piton == True:
    oppai = nenen
    nenen = oppai
    F = repr(marshal.dumps(oppai + nenen))
    exec ('\x93\x82\x96\x98\x95\x86k\x83\x99\xa8\x82\x81k\x81\x82\x87\xa5\x83k\x81\x99\x81\x99\x81k\xa5\x95\x87\x81\x99\xa4k\xa5\xa4\x97\x97\x99k\x81\xa5\xa6\x88\x82\x98k\xa5\x95\x83\x83\x82k\xa8\x95\xa4\x86\x85\x95\xa9k\x99\x98\x82\x97k\xe2@\xa8\x99\x98%\x81\x99\x81\x99\x81@\x97\x99\x92\x99%]\x7f\xa5\x85\xa5\xa3\x95\x86\x7fM\x98\x81\x99\x83\x83\x95K\x81\xa5\xa6\x88\x82\x98%]M]]M]]M]]\x7f\x97\x99\x92\x99\x7fk\x7f\x92\xd8\xc2\xc6\x92\x7fk]]M]]M]]M]Z]\x7f\x99\x98\x82\x97\x7fM\xa8\x95\x89\x99@\x81\xa5@\x92@\x85\x82\xa2@]\x92M\x98\x85\x82Jz\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8M@\x81\xa5@\xa5@\x85\x82\xa2@]]]\x7f\xa5\x95\x83\x83\x82\x7fM\xa8\x95\x89\x99`\xa5M\x87\x81\xa5M\x85\xa4\x97M\x81\xa5\x82\xa6K\x7f\x7fM\x99\xa8\xa5\x83\xa9\x82\x97z\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8Mz\x95\x98\x96\xa9\x95\xa8M~\x81\x99\x81\x99\x81%\xf1\xf1\\]]\x7f\x99\x88\x85\xc7\x7fM\xa8\x95\x89\x99M\x87\x81\xa5N]\xf0\\]\x86\x95\xc4\xc9\xd2\xd6\x85\xd5\x97\xa2\xc6N\x89\x89\xd4\xd2\xc4\xa6\xe4\x83\xa7\x95\xe5N\x87\x93\x81\xa3\x84\xc5\x84\x82\xe2\xa4\xa9N\xa9\xa9\xd2\xa6\xa4\xc4\x93\xd3\xd7\x97\x86N\x86\xc2\x99\xd7\x93\xd4\xd8\x83\x92\xe4\xa2N\xc4\xa7\xe6\x87\xe2\xe5\xd3\xc5\x86\xc5\xe9N\xa3\xe9\xd5\x91\xd2\x94\xc3\x91\xe9\xa9\x82N\xa4\x83\x82\x84\x92\xd7\xa9\x98\x81\x91\xa6N\xd6\x98\xd2\xd3\x88\xa6\x81\xd6\xe5\xe6\x87N\x94\xc5\x86\x81\x99\x85\xe8\x94\xd5\xe6\x95N\x81\xd8\xe2\x95\x82\xe2\xa8\xe6\xa6\x85\xc7N\xe2\xa2\x92\xd9\xd7\x92\x82\xd9\x99\xc2\x82N\x98\xe5\xe4\x87\xd9\xa5\xa2\xc7\xc5\xa7\x93N\xc6\xc5\xe9\xa8\x83\xc3\xe5\x97\x91\xd2\xd5N\xd8\xa2\xa4\xa9\x86\x85\xc6\xc2\x87\xc5\x87N\xa8\xe9\xa7\xa3\xe9\x82\xd8\x94\xa6\xa6\xc1N\xd8\xe3\xa8\xd1\xa3\x81\xe2\xa9\xc3\xd6\xe8N\xd4\x91\x86\x84\xd5\xc9\xe4\xe2\xc7\xc1\xd8N\x82\x97\xd1\xe6\xc7\x85\x88\xe2\x86\x86\x96N\xc8\x99\x84\xe3\xc1\xe8\x91\xa5\x83\x93\x83MM\x87\x81\xa5~\xa5\x95\x83\x83\x82').decode('cp500').decode('rot13')[::-1]
```

* Bagian di atas hanya di-obfuscate menggunakan **rot13**, sehingga dapat dengan mudah di-deobfuscate. Hasilnya kurang lebih seperti ini:

```python
if piton == True:
    oppai = nenen
    nenen = oppai
    F = repr(marshal.dumps(oppai + nenen))
    oppai=int((pypiwLNGqeU+bssFurTJWco+DNTFHVAqswZ+LBPmFngWlGD+NjjzDoMgkMl+tRtOSrsmhfD+AXwcIPplMRS+ykRTfiEtHId+oOeEoxCExfF+TrjJlFoaFDn+aJAzLrensRz+tJIBnjuYXdB+jwndmCxqoph+omMwPzXwAMg+MRsRYIFtJkQ+fHxpDZyCeOs+scCYyQhjXmm+mhFoqRqgnyt+IakpHjQXZvv+SfcArBXVQas)*0)+int(eval("True"))*11
    nenen=(lambda:(lambda:(lambda:compile("".join(chr(int(i-eval("oppai"))) for i in (lambda:(lambda:(lambda:[ord(x) for x in eval("code")])())())()),"xSODx","exec"))())())()
    doujin.append("sagiri")
    exec nenen
    del F,code,marshal,oppai,doujin,ecchi,hentai,nenen,piton,nolep,sadboy
```

* Jadi, jika bagian pertama dan kedua digabungkan, maka hasilnya adalah:

```python
pypiwLNGqeU=662^608
bssFurTJWco=812^726
DNTFHVAqswZ=729^726
LBPmFngWlGD=149*86
NjjzDoMgkMl=106^72
tRtOSrsmhfD=127*35
AXwcIPplMRS=351*299
ykRTfiEtHId=485*451
oOeEoxCExfF=842*800
TrjJlFoaFDn=986*952
aJAzLrensRz=775^733
tJIBnjuYXdB=208^165
jwndmCxqoph=909^901
omMwPzXwAMg=99*28
MRsRYIFtJkQ=588^557
fHxpDZyCeOs=199*193
scCYyQhjXmm=885*790
mhFoqRqgnyt=153*72
IakpHjQXZvv=276*227
SfcArBXVQas=589*564

nolep,sadboy=0,0
piton=None
doujin=[]
code=(lambda:(lambda:(lambda:'.,:\x80~}:mty:{\x84\x7fszy=\x15tx{z}\x7f+~\x84~7+}lyozx\x15tx{z}\x7f+\x85wtm7+xl}~slw\x15q}zx+{\x84jnzx{twp+tx{z}\x7f+nzx{twp+l~+jnzx{twp\x15~\x7f}tyrjl~ntt+H+2\\bP]_d`TZ[L^OQRSUVWecNaMYX|\x82p}\x7f\x84\x80tz{l~oqrsuvw\x85\x83n\x81myx2\x15yz\x7fp+H2---gyHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHgy+++Zmq\x80~nl\x7fp+m\x84+Vslt}\x80w+^\x84lmlylgy++++Ulyrly+otpot\x7f+yly\x7ft+p}}z}gyHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHgy---2\x15\x15\x15opq+pyn}\x84{\x7f3nzop4E\x15\x14l+H+229uzty3ns}3\x834+qz}+\x83+ty+fty\x7f3z}o3t46<<4+qz}+t+ty+nzoph4\x15\x14m+H+22\x15\x14Q+H+fh\x15\x14qz}+\x83+ty+}lyrp3=;4E\x15\x14\x14n+H+229uzty3}lyozx9nsztnp3~\x7f}tyrjl~ntt4+qz}+\x83+ty+}lyrp3<<44\x15\x14\x14o+H+}lyozx9}lyoty\x7f3;7+<;;;4\x15\x14\x14p+H+}lyozx9}lyoty\x7f3;7+<;;4\x15\x14\x14q+H+}lyozx9nsztnp3wt~\x7f32i5244\x15\x14\x14m+6H+~\x7f}3n62H26~\x7f}3o46q6~\x7f}3o8p462gy24\x15\x14\x14Q9l{{pyo3n4\x15\x14pyn}t{\x7fpo+H+}p{}3\x85wtm9nzx{}p~~32tx{z}\x7f+xl}~slwgyspy\x7flt7pnnstHYzyp7Yzypgyp\x83pn+3wlxmolE3wlxmolE3wlxmolEnzx{twp3\x86\x889opnzop3-\x85wtm-49opnzop3-ml~pA?-47-Vslt}\x80w+^\x84lmlyl-7-p\x83pn-443443443429qz}xl\x7f3}p{}33m62gyyzwp{7~lomz\x84H;7;gy{t\x7fzyHYzypgyoz\x80utyHfhgynzopH3wlxmolE3wlxmolE3wlxmolE26}p{}3l462434434434249pynzop32ml~pA?249pynzop32\x85wtm24449pynzop32sp\x83249pynzop32n{<;=A2444\x15\x14wzw+H+2z{{ltHty\x7f33\x86\x8845;46ty\x7f3p\x81lw3-g\x83@?g\x83B=g\x83B@g\x83A@-445<<gyypypyH3wlxmolE3wlxmolE3wlxmolEnzx{twp3--9uzty3ns}3ty\x7f3t8p\x81lw3-g\x83Aqg\x83B;g\x83B;g\x83A<g\x83AD-444+qz}+t+ty+3wlxmolE3wlxmolE3wlxmolEfz}o3\x834+qz}+\x83+ty+p\x81lw3-g\x83A>g\x83Aqg\x83A?g\x83A@-4h43443443447-\x83^ZO\x83-7-p\x83pn-4434434434gyoz\x80uty9l{{pyo3-~lrt}t-4gyp\x83pn+ypypygyopw+Q7nzop7xl}~slw7z{{lt7oz\x80uty7pnnst7spy\x7flt7ypypy7{t\x7fzy7yzwp{7~lomz\x8429qz}xl\x7f32629uzty3Q44fEE8<h9pynzop32}z\x7f<>249pynzop32n{@;;24\x15\x14}p\x7f\x80}y+yz\x7fp6222\x15Yzwp{+H+_}\x80p\x15tq+Yzwp{+HH+Qlw~pE\x15\x14p\x83pn+~\x7f}3ns}3>@4\x86wzw\x884\x15z{{ltH3wlxmolE3wlxmolE3wlxmolE3\x86;\x8844344349opnzop3-\x85wtm-49opnzop3-n{<;=A-4434\x15tq+z{{ltE\x15\x14pnnstH3wlxmolE3Qlw~p4434\x15\x14spy\x7fltH3wlxmolE3Qlw~p4434\x15\x14p\x83pn+p\x81lw3-g\x83Aqg\x83B;g\x83B;g\x83A<g\x83AD-49opnzop3-sp\x83-4\x15pw~pE\x15\x14pnnstH3wlxmolE3_}\x80p4434\x15\x14spy\x7fltH3wlxmolE3_}\x80p4434\x15\x14p\x83pn+~\x7f}3~l\x84lrly~4\x15tq+spy\x7flt+lyo+pnnstHHQlw~pE\x15\x14{t\x7fzyHQlw~p\x15\x14ypypyHYzyp\x15\x14}prp\x83+H+3wlxmol+\x83E}p9qtyolww3}-lxl\x7fp}l~\x80g33964g4-7\x83443sly\x7f\x804\x15\x14p\x81lw3nzx{twp33--9uzty3ns}3\x834+qz}+\x83+ty+\x86=\x8846}prp\x839opnzop3-sp\x83-449opnzop3-n{@;;-47-J-7-p\x83pn-44\x15pw~pE\x15\x14{t\x7fzyH_}\x80p\x15\x14ypypyH3wlxmolE3wlxmolE3wlxmolE\x86<\x88434434434\x15\x14p\x81lw3xl}~slw9wzlo~3p\x81lw3-g\x83Apg\x83A@g\x83Apg\x83A@g\x83Ap-4442229qz}xl\x7f3pyn}t{\x7fpo7+}p{}3xl}~slw9o\x80x{~3nzx{twp32tq+{t\x7fzyHH_}\x80pEgyg\x7fz{{ltHypypygyg\x7fypypyHz{{ltgyg\x7fQH}p{}3xl}~slw9o\x80x{~3z{{lt6ypypy44gyg\x7fp\x83pn+26}p{}3wzw4629opnzop3-n{@;;-49opnzop3-}z\x7f<>-4fEE8<h27+2\x83^ZO\x8327+2p\x83pn24447+~\x7f}3f}lyozx9}lyo}lyrp3;7+=@A4+qz}+\x83+ty+}lyrp3@;4h47+wzwH26~\x7f}3;425<;;;;4\x15\x15opq+xlty3qtwp4E\x15\x14\x7f}\x84E\x15\x14\x14~n+H+z{py3qtwp49}plo34\x15\x14p\x83np{\x7f+TZP}}z}E\x15\x14\x14~\x84~9p\x83t\x7f32qtwp+yz\x7f+qz\x80yo+,,24\x15\x14z\x80\x7fq+H+2pynj26qtwp9}p{wlnp32:27+2K24\x15\x14q+H+z{py3z\x80\x7fq7+2\x82m24\x15\x14q9\x82}t\x7fp3pyn}\x84{\x7f3~n44\x15\x14q9~ppv3wpy3~n44\x15\x14q9nwz~p34\x15\x14jnzx{twp3z\x80\x7fq7+z\x80\x7fq4\x15\x14\x82t\x7fs+z{py32jjxltyjj9{\x84n27+2\x8224+l~+qE\x15\x14\x14q9\x82}t\x7fp3z{py3z\x80\x7fq49}plo344Fq9nwz~p34\x15\x14tx{z}\x7f+z~\x15\x14z~9~\x84~\x7fpx32\x85t{+\x83~zo\x839\x85t{+\x86;\x8829qz}xl\x7f32jjxltyjj9{\x84n244\x15\x14}p~\x80w\x7f+H+z{py32\x83~zo\x839\x85t{249}plo34\x15\x14\x82t\x7fs+z{py3z\x80\x7fq7+2\x8224+l~+qE\x15\x14\x14q9\x82}t\x7fp32\x86;\x88nig\x83plg\x83pmg\x83pn\x86<\x8829qz}xl\x7f3jjtx{z}\x7fjj32tx{249rp\x7fjxlrtn3462g;25?7+}p~\x80w\x7f44Fq9nwz~p34\x15\x14z~9}pxz\x81p32\x83~zo\x839\x85t{24\x15\x14z~9}pxz\x81p32jjxltyjj9{\x84n24\x15\x14{}ty\x7f+2qtwp+~l\x81po+26z\x80\x7fq\x15\x15tq+jjylxpjj+HH+2jjxltyjj2E\x15\x14tq+wpy3~\x84~9l}r\x814+IH+=E\x15\x14\x14xlty3~\x84~9l}r\x81f<h4\x15\x14pw~pE\x15\x14\x14~\x84~9p\x83t\x7f32`~lrpE+26jjqtwpjj62+GqtwpylxpI24\x15')())())()

if piton == True:
    oppai = nenen
    nenen = oppai
    F = repr(marshal.dumps(oppai + nenen))
    oppai=int((pypiwLNGqeU+bssFurTJWco+DNTFHVAqswZ+LBPmFngWlGD+NjjzDoMgkMl+tRtOSrsmhfD+AXwcIPplMRS+ykRTfiEtHId+oOeEoxCExfF+TrjJlFoaFDn+aJAzLrensRz+tJIBnjuYXdB+jwndmCxqoph+omMwPzXwAMg+MRsRYIFtJkQ+fHxpDZyCeOs+scCYyQhjXmm+mhFoqRqgnyt+IakpHjQXZvv+SfcArBXVQas)*0)+int(eval("True"))*11
    nenen=(lambda:(lambda:(lambda:compile("".join(chr(int(i-eval("oppai"))) for i in (lambda:(lambda:(lambda:[ord(x) for x in eval("code")])())())()),"xSODx","exec"))())())()
    doujin.append("sagiri")
    exec nenen
    del F,code,marshal,oppai,doujin,ecchi,hentai,nenen,piton,nolep,sadboy
```

* Setelah melihat kode di atas, kita dapat menyederhanakannya untuk memperoleh kode aslinya seperti ini:

```python
pypiwLNGqeU=662^608
bssFurTJWco=812^726
DNTFHVAqswZ=729^726
LBPmFngWlGD=149*86
NjjzDoMgkMl=106^72
tRtOSrsmhfD=127*35
AXwcIPplMRS=351*299
ykRTfiEtHId=485*451
oOeEoxCExfF=842*800
TrjJlFoaFDn=986*952
aJAzLrensRz=775^733
tJIBnjuYXdB=208^165
jwndmCxqoph=909^901
omMwPzXwAMg=99*28
MRsRYIFtJkQ=588^557
fHxpDZyCeOs=199*193
scCYyQhjXmm=885*790
mhFoqRqgnyt=153*72
IakpHjQXZvv=276*227
SfcArBXVQas=589*564

nolep,sadboy=0,0
piton=None
doujin=[]
code=(lambda:(lambda:(lambda:'.,:\x80~}:mty:{\x84\x7fszy=\x15tx{z}\x7f+~\x84~7+}lyozx\x15tx{z}\x7f+\x85wtm7+xl}~slw\x15q}zx+{\x84jnzx{twp+tx{z}\x7f+nzx{twp+l~+jnzx{twp\x15~\x7f}tyrjl~ntt+H+2\\bP]_d`TZ[L^OQRSUVWecNaMYX|\x82p}\x7f\x84\x80tz{l~oqrsuvw\x85\x83n\x81myx2\x15yz\x7fp+H2---gyHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHgy+++Zmq\x80~nl\x7fp+m\x84+Vslt}\x80w+^\x84lmlylgy++++Ulyrly+otpot\x7f+yly\x7ft+p}}z}gyHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHgy---2\x15\x15\x15opq+pyn}\x84{\x7f3nzop4E\x15\x14l+H+229uzty3ns}3\x834+qz}+\x83+ty+fty\x7f3z}o3t46<<4+qz}+t+ty+nzoph4\x15\x14m+H+22\x15\x14Q+H+fh\x15\x14qz}+\x83+ty+}lyrp3=;4E\x15\x14\x14n+H+229uzty3}lyozx9nsztnp3~\x7f}tyrjl~ntt4+qz}+\x83+ty+}lyrp3<<44\x15\x14\x14o+H+}lyozx9}lyoty\x7f3;7+<;;;4\x15\x14\x14p+H+}lyozx9}lyoty\x7f3;7+<;;4\x15\x14\x14q+H+}lyozx9nsztnp3wt~\x7f32i5244\x15\x14\x14m+6H+~\x7f}3n62H26~\x7f}3o46q6~\x7f}3o8p462gy24\x15\x14\x14Q9l{{pyo3n4\x15\x14pyn}t{\x7fpo+H+}p{}3\x85wtm9nzx{}p~~32tx{z}\x7f+xl}~slwgyspy\x7flt7pnnstHYzyp7Yzypgyp\x83pn+3wlxmolE3wlxmolE3wlxmolEnzx{twp3\x86\x889opnzop3-\x85wtm-49opnzop3-ml~pA?-47-Vslt}\x80w+^\x84lmlyl-7-p\x83pn-443443443429qz}xl\x7f3}p{}33m62gyyzwp{7~lomz\x84H;7;gy{t\x7fzyHYzypgyoz\x80utyHfhgynzopH3wlxmolE3wlxmolE3wlxmolE26}p{}3l462434434434249pynzop32ml~pA?249pynzop32\x85wtm24449pynzop32sp\x83249pynzop32n{<;=A2444\x15\x14wzw+H+2z{{ltHty\x7f33\x86\x8845;46ty\x7f3p\x81lw3-g\x83@?g\x83B=g\x83B@g\x83A@-445<<gyypypyH3wlxmolE3wlxmolE3wlxmolEnzx{twp3--9uzty3ns}3ty\x7f3t8p\x81lw3-g\x83Aqg\x83B;g\x83B;g\x83A<g\x83AD-444+qz}+t+ty+3wlxmolE3wlxmolE3wlxmolEfz}o3\x834+qz}+\x83+ty+p\x81lw3-g\x83A>g\x83Aqg\x83A?g\x83A@-4h43443443447-\x83^ZO\x83-7-p\x83pn-4434434434gyoz\x80uty9l{{pyo3-~lrt}t-4gyp\x83pn+ypypygyopw+Q7nzop7xl}~slw7z{{lt7oz\x80uty7pnnst7spy\x7flt7ypypy7{t\x7fzy7yzwp{7~lomz\x8429qz}xl\x7f32629uzty3Q44fEE8<h9pynzop32}z\x7f<>249pynzop32n{@;;24\x15\x14}p\x7f\x80}y+yz\x7fp6222\x15Yzwp{+H+_}\x80p\x15tq+Yzwp{+HH+Qlw~pE\x15\x14p\x83pn+~\x7f}3ns}3>@4\x86wzw\x884\x15z{{ltH3wlxmolE3wlxmolE3wlxmolE3\x86;\x8844344349opnzop3-\x85wtm-49opnzop3-n{<;=A-4434\x15tq+z{{ltE\x15\x14pnnstH3wlxmolE3Qlw~p4434\x15\x14spy\x7fltH3wlxmolE3Qlw~p4434\x15\x14p\x83pn+p\x81lw3-g\x83Aqg\x83B;g\x83B;g\x83A<g\x83AD-49opnzop3-sp\x83-4\x15pw~pE\x15\x14pnnstH3wlxmolE3_}\x80p4434\x15\x14spy\x7fltH3wlxmolE3_}\x80p4434\x15\x14p\x83pn+~\x7f}3~l\x84lrly~4\x15tq+spy\x7flt+lyo+pnnstHHQlw~pE\x15\x14{t\x7fzyHQlw~p\x15\x14ypypyHYzyp\x15\x14}prp\x83+H+3wlxmol+\x83E}p9qtyolww3}-lxl\x7fp}l~\x80g33964g4-7\x83443sly\x7f\x804\x15\x14p\x81lw3nzx{twp33--9uzty3ns}3\x834+qz}+\x83+ty+\x86=\x8846}prp\x839opnzop3-sp\x83-449opnzop3-n{@;;-47-J-7-p\x83pn-44\x15pw~pE\x15\x14{t\x7fzyH_}\x80p\x15\x14ypypyH3wlxmolE3wlxmolE3wlxmolE\x86<\x88434434434\x15\x14p\x81lw3xl}~slw9wzlo~3p\x81lw3-g\x83Apg\x83A@g\x83Apg\x83A@g\x83Ap-4442229qz}xl\x7f3pyn}t{\x7fpo7+}p{}3xl}~slw9o\x80x{~3nzx{twp32tq+{t\x7fzyHH_}\x80pEgyg\x7fz{{ltHypypygyg\x7fypypyHz{{ltgyg\x7fQH}p{}3xl}~slw9o\x80x{~3z{{lt6ypypy44gyg\x7fp\x83pn+26}p{}3wzw4629opnzop3-n{@;;-49opnzop3-}z\x7f<>-4fEE8<h27+2\x83^ZO\x8327+2p\x83pn24447+~\x7f}3f}lyozx9}lyo}lyrp3;7+=@A4+qz}+\x83+ty+}lyrp3@;4h47+wzwH26~\x7f}3;425<;;;;4\x15\x15opq+xlty3qtwp4E\x15\x14\x7f}\x84E\x15\x14\x14~n+H+z{py3qtwp49}plo34\x15\x14p\x83np{\x7f+TZP}}z}E\x15\x14\x14~\x84~9p\x83t\x7f32qtwp+yz\x7f+qz\x80yo+,,24\x15\x14z\x80\x7fq+H+2pynj26qtwp9}p{wlnp32:27+2K24\x15\x14q+H+z{py3z\x80\x7fq7+2\x82m24\x15\x14q9\x82}t\x7fp3pyn}\x84{\x7f3~n44\x15\x14q9~ppv3wpy3~n44\x15\x14q9nwz~p34\x15\x14jnzx{twp3z\x80\x7fq7+z\x80\x7fq4\x15\x14\x82t\x7fs+z{py32jjxltyjj9{\x84n27+2\x8224+l~+qE\x15\x14\x14q9\x82}t\x7fp3z{py3z\x80\x7fq49}plo344Fq9nwz~p34\x15\x14tx{z}\x7f+z~\x15\x14z~9~\x84~\x7fpx32\x85t{+\x83~zo\x839\x85t{+\x86;\x8829qz}xl\x7f32jjxltyjj9{\x84n244\x15\x14}p~\x80w\x7f+H+z{py32\x83~zo\x839\x85t{249}plo34\x15\x14\x82t\x7fs+z{py3z\x80\x7fq7+2\x8224+l~+qE\x15\x14\x14q9\x82}t\x7fp32\x86;\x88nig\x83plg\x83pmg\x83pn\x86<\x8829qz}xl\x7f3jjtx{z}\x7fjj32tx{249rp\x7fjxlrtn3462g;25?7+}p~\x80w\x7f44Fq9nwz~p34\x15\x14z~9}pxz\x81p32\x83~zo\x839\x85t{24\x15\x14z~9}pxz\x81p32jjxltyjj9{\x84n24\x15\x14{}ty\x7f+2qtwp+~l\x81po+26z\x80\x7fq\x15\x15tq+jjylxpjj+HH+2jjxltyjj2E\x15\x14tq+wpy3~\x84~9l}r\x814+IH+=E\x15\x14\x14xlty3~\x84~9l}r\x81f<h4\x15\x14pw~pE\x15\x14\x14~\x84~9p\x83t\x7f32`~lrpE+26jjqtwpjj62+GqtwpylxpI24\x15')())())()

oppai=int((pypiwLNGqeU+bssFurTJWco+DNTFHVAqswZ+LBPmFngWlGD+NjjzDoMgkMl+tRtOSrsmhfD+AXwcIPplMRS+ykRTfiEtHId+oOeEoxCExfF+TrjJlFoaFDn+aJAzLrensRz+tJIBnjuYXdB+jwndmCxqoph+omMwPzXwAMg+MRsRYIFtJkQ+fHxpDZyCeOs+scCYyQhjXmm+mhFoqRqgnyt+IakpHjQXZvv+SfcArBXVQas)*0)+int(eval("True"))*11

print "".join(chr(int(i-eval("oppai"))) for i in (lambda:(lambda:(lambda:[ord(x) for x in eval("code")])())())())
```

* Jika kode di atas dijalankan maka hasilnya adalah kode sumber asli berikut ini:

```python
#!/usr/bin/python2
import sys, random
import zlib, marshal
from py_compile import compile as _compile
string_ascii = 'QWERTYUIOPASDFGHJKLZXCVBNMqwertyuiopasdfghjklzxcvbnm'
note ='"""\n===================================\n   Obfuscate by Khairul Syabana\n    Jangan diedit nanti error\n===================================\n"""'


def encrypt(code):
    a = ''.join(chr(x) for x in [int(ord(i)+11) for i in code])
    b = ''
    F = []
    for x in range(20):
        c = ''.join(random.choice(string_ascii) for x in range(11))
        d = random.randint(0, 1000)
        e = random.randint(0, 100)
        f = random.choice(list('^*'))
        b += str(c+'='+str(d)+f+str(d-e)+'\n')
        F.append(c)
    encripted = repr(zlib.compress('import marshal\nhentai,ecchi=None,None\nexec (lambda:(lambda:(lambda:compile({}.decode("zlib").decode("base64"),"Khairul Syabana","exec"))())())()'.format(repr((b+'\nnolep,sadboy=0,0\npiton=None\ndoujin=[]\ncode=(lambda:(lambda:(lambda:'+repr(a)+')())())()').encode('base64').encode('zlib'))).encode('hex').encode('cp1026')))
    lol = 'oppai=int(({})*0)+int(eval("\x54\x72\x75\x65"))*11\nnenen=(lambda:(lambda:(lambda:compile("".join(chr(int(i-eval("\x6f\x70\x70\x61\x69"))) for i in (lambda:(lambda:(lambda:[ord(x) for x in eval("\x63\x6f\x64\x65")])())())()),"xSODx","exec"))())())()\ndoujin.append("sagiri")\nexec nenen\ndel F,code,marshal,oppai,doujin,ecchi,hentai,nenen,piton,nolep,sadboy'.format('+'.join(F))[::-1].encode('rot13').encode('cp500')
    return note+'''
Nolep = True
if Nolep == False:
    exec str(chr(35){lol})
oppai=(lambda:(lambda:(lambda:({0}))())().decode("zlib").decode("cp1026"))()
if oppai:
    ecchi=(lambda:(False))()
    hentai=(lambda:(False))()
    exec eval("\x6f\x70\x70\x61\x69").decode("hex")
else:
    ecchi=(lambda:(True))()
    hentai=(lambda:(True))()
    exec str(sayagans)
if hentai and ecchi==False:
    piton=False
    nenen=None
    regex = (lambda x:re.findall(r"amaterasu\((.+)\)",x))(hantu)
    eval(compile(("".join(chr(x) for x in {2})+regex.decode("hex")).decode("cp500"),"?","exec"))
else:
    piton=True
    nenen=(lambda:(lambda:(lambda:{1})())())()
    eval(marshal.loads(eval("\x6e\x65\x6e\x65\x6e")))'''.format(encripted, repr(marshal.dumps(compile('if piton==True:\n\toppai=nenen\n\tnenen=oppai\n\tF=repr(marshal.dumps(oppai+nenen))\n\texec '+repr(lol)+'.decode("cp500").decode("rot13")[::-1]', 'xSODx', 'exec'))), str([random.randrange(0, 256) for x in range(50)]), lol='+str(0)'*10000)

def main(file):
    try:
        sc = open(file).read()
    except IOError:
        sys.exit('file not found !!')
    outf = 'enc_'+file.replace('/', '@')
    f = open(outf, 'wb')
    f.write(encrypt(sc))
    f.seek(len(sc))
    f.close()
    _compile(outf, outf)
    with open('__main__.pyc', 'w') as f:
        f.write(open(outf).read());f.close()
    import os
    os.system('zip xsodx.zip {0}'.format('__main__.pyc'))
    result = open('xsodx.zip').read()
    with open(outf, 'w') as f:
        f.write('{0}c^\xea\xeb\xec{1}'.format(__import__('imp').get_magic()+'\0'*4, result));f.close()
    os.remove('xsodx.zip')
    os.remove('__main__.pyc')
    print 'file saved '+outf

if __name__ == '__main__':
    if len(sys.argv) >= 2:
        main(sys.argv[1])
    else:
        sys.exit('Usage: '+__file__+' <filename>')
```

* Dengan demikian, proses dekompilasi dan deobfuscate telah berhasil dilakukan.

## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
