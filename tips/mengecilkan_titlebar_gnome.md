
# Mengecilkan Titlebar Gnome

Tutorial singkat kali ini akan membahas cara mengecilkan ukuran _titlebar_ pada _window manager_ gnome. Alasannya adalah titlebar gnome saat ini terlalu besar, dan sudah banyak juga pengguna _window manager_ gnome yang mengeluhkan hal tersebut, misalnya yang dapat dibaca pada sebuah [_thread_ di reddit](https://www.reddit.com/r/gnome/comments/dtwdqs/how_to_make_the_title_bar_smaller/). Pada _thread_ tersebut terdapat jawaban yang mengarahkan kepada sebuah [tema gnome yang titlebarnya cukup kecil](https://www.gnome-look.org/p/1288797/). Namun, tema tersebut hanya akan mengecilkan titlebar untuk tema dengan warna cerah. Namun, kita dapat menggunakan tema gelap tersebut dengan titlebar yang kecil. Berikut ini adalah langkah-langkahnya:

* Pertama, unduh terlebih dahulu [tema tersebut](https://www.gnome-look.org/p/1288797/).

* Setelah mengunduh. Pindah ke sub direktori `~/.themes` pada direktori home Anda.

```
$ cd ~/.themes
```

* Selanjutnya ekstrak tema yang telah diunduh pada langkah pertama dengan perintah berikut ini:

```
$ tar xvf NewAdwaita-slim.tar.xz
```

* Pindah ke sub direktori hasil ekstraksi.

```
$ cd NewAdwaita-slim/gtk-3.0/
```

* Ubah nama file `gtk.css` menjadi `gtk-light.css` atau yang lainnya, dan ubah nama `gtk-dark.css` menjadi `gtk.css`:


```
$ mv gtk.css gtk-light.css
$ mv gtk-dark.css gtk.css
```

* Selanjutnya hapus file `gtk-contained-dark.css` dan unduh file yang sudah diedit dan terdapat pada repositori github penulis:

```
$ rm gtk-contained-dark.css
$ wget https://raw.githubusercontent.com/drubicza/misc/master/linux/gtk-contained-dark.css
```

* Langkah berikutnya adalah gunakan **gnome-tweak-tool** untuk mengubah tema gnome pada sistem Anda. Jika Anda belum memiliki **gnome-tweak-tool**, maka Anda dapat melakukan instalasi menggunakan aplikasi manajeman paket yang terdapat pada sistem Anda, misalnya menggunakan **dnf** pada distro Fedora dengan perintah berikut ini:

```
$ sudo dnf install gnome-tweak-tool
```

* Jalankan **gnome-tweak-tool**, lalu pada panel sebelah kiri pilih **Appearance**, lalu pada panel sebelah kanan pada bagian **Applications** pilih **NewAdwaita-slim**. Setelah memilih tema tersebut, maka _window manager_ gnome yang Anda gunakan otomatis akan mengaplikasikannya.


Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
