# Tutorial Script Antiuncompyle6

## Pendahuluan

Beberapa waktu lalu, ada seseorang yang mengirimkan sebuah script dengan nama **AntiUncompyle6.py**. File tersebut cukup kecil. Tutorial ini akan membahas secara singkat tentang script tersebut.

## Langkah-langkah

* Pertama, kita periksa terlebih dahulu tipe dari file tersebut:

```bash
% file antiuncompyle6.py
antiuncompyle6.py: python 2.7 byte-compiled
```

* Ternyata file tersebut tipenya adalah python bytecode, jadi sebaiknya kita ubah ekstensinya menjadi **pyc**:

```bash
% mv antiuncompyle6.py antiuncompyle6.pyc
```

* Selanjutnya kita lakukan dekompilasi menggunakan [uncompyle6](https://pypi.org/project/uncompyle6/):

```bash
% uncompyle6 antiuncompyle6.pyc
```

* Hasilnya adalah script berikut ini:

```python
(lambda __print, __g, __contextlib: (
 __print('[*] anti uncompyle6 [*]'), (lambda __out: (lambda __ctx: [__ctx.__enter__(), __ctx.__exit__(None, None, None), __out[0](lambda : [ [ [ (open(outf, 'w').write(awok + '\n' + awoka), (__import__('py_compile').compile(outf, outf), None)[1])[1] for __g['awoka'] in [('exec {}.decode("zlib")').format(repr(open(file).read().encode('zlib')))] ][0] for __g['awok'] in [('exec str(chr(35){})').format('+str(0)' * 10000)] ][0] for __g['outf'] in ['enc_' + file] ][0])][2])(__contextlib.nested(type('except', (), {'__enter__': lambda self: None, '__exit__': lambda __self, __exctype, __value, __traceback: __exctype is not None and issubclass(__exctype, Exception) and [ [ True for __out[0] in [(exit(e), lambda after: after())[1]] ][0] for __g['e'] in [__value] ][0]})(), type('try', (), {'__enter__': lambda self: None, '__exit__': lambda __self, __exctype, __value, __traceback: [ False for __out[0] in [[ lambda __after: __after() for __g['file'] in [raw_input('masukan file: ')] ][0]] ][0]})())))([None]))[1])(__import__('__builtin__', level=0).__dict__['print'], globals(), __import__('contextlib', level=0))
```

* Jika dilihat sepintas, script tersebut tampak cukup rumit. Namun, pada dasarnya script tersebut tidak serumit yang Anda kira. Script tersebut akan meminta _input_ berupa nama file kepada user. Lalu untuk _output_, nama file akan diambil dari file input lalu diberi _prefix_ **enc_**. Misalnya nama file _input_ adalah **tes.py**, maka file _output_ adalah **enc_tes.py**. Adapun operasi yang akan dilakukan terhadap file _input_, adalah isinya akan dibaca terlebih dahulu. Setelah itu, isinya akan di-encode menggunakan _zlib_. File output kemudian akan ditulisi terlebih dahulu dengan karakter comment **#** lalu diikuti dengan 10000 _null character_ baru kemudian diisi dengan hasil encode _zlib_ pada langkah sebelumnya. Langkah terakhir adalah melakukan kompilasi terhadap file tersebut dengan memanfaatkan modul `py_compile`.

* Untuk demonstrasi, kita akan membuat sebuah script python dengan nama **tes.py** yang isinya sebagai berikut:

```python
print "hello world"
```

* Script tersebut kemudian akan kita gunakan sebagai input untuk script **antiuncompyle6.pyc**:

```bash
% python2.7 antiuncompyle6.pyc
[*] anti uncompyle6 [*]
masukan file: tes.py
```

* Setelah proses tersebut selesai, kita bisa melihat bahwa file _output_ disimpan dengan nama **enc_tes.py** seperti yang telah dijelaskan di atas:

```bash
% ls -1 *tes*
enc_tes.py
tes.py
```

* Jika kita periksa tipe dari file **enc_tes.py** maka hasilnya sebagai berikut:

```bash
% file enc_tes.py
enc_tes.py: python 2.7 byte-compiled
```

* Selanjutnya, kita akan mengganti ekstensi dari file tersebut menjadi **pyc**:

```bash
% mv enc_tes.py enc_tes.pyc
```

* Jika kita mencoba melakukan dekompilasi menggunakan uncompyle6, maka kita akan menjumpai pesan error berikut ini:

```bash
% uncompyle6 enc_tes.pyc
# uncompyle6 version 3.7.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 3.8.5 (default, Aug 12 2020, 00:00:00)
# [GCC 10.2.1 20200723 (Red Hat 10.2.1-1)]
# Embedded file name: enc_tes.py
# Compiled at: 2020-09-24 16:25:58

maximum recursion depth exceeded while calling a Python object


Last file: enc_tes.pyc   Traceback (most recent call last):
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

* Demikian pula jika kita menggunakan **pycdc** untuk melakukan dekompilasi:

```bash
% pycdc enc_tes.pyc
# Source Generated with Decompyle++
# File: enc_tes.pyc (Python 2.7)

zsh: segmentation fault (core dumped)  pycdc enc_tes.pyc
```

* Namun, tidak demikian jika kita melakukan _disassemble_ terhadap file tersebut menggunakan **pycdas** seperti ini:

```bash
% pycdas enc_tes.pyc

        (... snip ...)

        100020  LOAD_CONST              3: 'x\x9c+(\xca\xcc+QP\xcaH\xcd\xc9\xc9W(\xcf/\xcaIQ\xe2\xe2\x02\x00T\xfe\x07\x02'
        100023  LOAD_ATTR               2: decode
        100026  LOAD_CONST              4: 'zlib'
        100029  CALL_FUNCTION           1
        100032  LOAD_CONST              2: None
        100035  DUP_TOP
        100036  EXEC_STMT
        100037  LOAD_CONST              2: None
        100040  RETURN_VALUE
```

* Bisa terlihat, bahwa bagian yang di-encode menggunakan _zlib_ masih dapat terlihat dengan jelas, dan kita dapat melakukan dekompilasi secara sederhana seperti ini:

```bash
% python2.7 -c "print 'x\x9c+(\xca\xcc+QP\xcaH\xcd\xc9\xc9W(\xcf/\xcaIQ\xe2\xe2\x02\x00T\xfe\x07\x02'.decode('zlib')"
print "hello world"
```

* Script asli file tersebut dapat terlihat dengan jelas.

* Jika Anda sulit untuk mengerti script **antiuncompyle6.py** tersebut, berikut ini adalah script tersebut dalam bentuk yang mudah dibaca:

```python
import py_compile

file  = raw_input('masukan file: ')
outf  = 'enc_' + file
awok  = 'exec str(chr(35){})'.format('+str(0)' * 10000)
awoka = 'exec {}.decode("zlib")'.format(repr(open(file).read().encode('zlib')))
open(outf, 'w').write(awok + '\n' + awoka)
py_compile.compile(outf, outf)
```

* Bisa terlihat bahwa script tersebut cukup sederhana.


## Penutup

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
