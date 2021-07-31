
# Instalasi ugrep Pada Fedora

Berikut ini adalah langkah-langkah untuk melakukan instalasi [ugrep](https://github.com/Genivia/ugrep#install) secara manual dari kode sumber pada distro Fedora:

* Unduh terlebih dahulu arsip kode sumber dari repositori ugrep:

```zsh
% wget https://github.com/Genivia/ugrep/archive/refs/tags/v3.1.12.tar.gz
```

* Selanjutnya ekstrak arsip ugrep tersebut:

```zsh
% tar xvf v3.1.12.tar.gz
```

* Pindah ke sub direktori hasil ekstraksi:

```zsh
% cd ugrep-3.1.12
```

* Lakukan rekonfigurasi:

```zsh
% autoreconf -fi
```

* Lanjutkan dengan kompilasi menggunakan perintah:

```zsh
% ./build.sh
```

* Setelah proses kompilasi selesai, kita dapat melakukan instalasi menggunakan perintah:

```zsh
% sudo make install
```

* Langkah selanjutnya adalah membuat file konfigurasi yang nantinya dapat digunakan oleh perintah `ug`:

```zsh
% ug --save-config
```

* Setelah file konfigurasi dibuat pada direktori dimana kita berada, maka konfigurasi tersebut dapat kita edit lalu pindahkan ke direktori **HOME**:

```zsh
% mv .ugrep ~/
```

Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
