
# Instalasi rizin Secara Manual

Langkah pertama adalah menyiapkan sistem yang Anda gunakan. Jika sistem operasi yang Anda gunakan belum memiliki [meson](https://mesonbuild.com/), maka Anda terlebih dahulu harus melakukan instalasinya. Untuk sistem operasi berbasis Linux, Anda dapat melakukan instalasi menggunakan paket yang telah disediakan oleh distro yang Anda gunakan, atau lakukan instalasi manual menggunakan `pip`. Sebagai catatan, **meson** membutuhkan Python 3 (versi 3.6 atau yang lebih baru). Berikut ini adalah perintah untuk instalasi **meson** pada sistem secara menyeluruh (membutuhkan akses root, namun dapat digunakan oleh semua pengguna):

```bash
$ sudo pip3 install meson
```

Namun, jika Anda hanya ingin melakukan instalasi untuk Anda pakai sendiri, maka berikut ini adalah perintah yang digunakan:

```bash
$ pip3 install --user meson
```

Setelah melakukan instalasi **meson**, lanjutkan dengan melakukan instalasi [ninja](https://ninja-build.org/). Anda dapat melakukan instalasi melalui paket yang disediakana oleh distro linux yang Anda gunakan. Atau melakukan instalasi manual dengan mengunduh rilis ninja [di sini](https://github.com/ninja-build/ninja/releases). Atau Anda dapat pula melakukan instalasi dari kode sumbernya, menggunakan perintah berikut ini:

```bash
$ git clone https://github.com/ninja-build/ninja.git && cd ninja
$ git checkout release
$ ./configure.py --bootstrap
$ cmake --Bbuild-cmake -H.
$ cmake --build build-cmake
```

Setelah proses di atas selesai dijalankan tanpa ada kesalahan, maka hasilnya berupa aplikasi **ninja** yang terdapat pada sub direktori `build-cmake`. Aplikasi **ninja** tersebut kemudian dapat Anda pindahkan ke direktori yang terdapat pada sistem atau **PATH** sehingga mudah dijalankan. Jika Anda ingin menggunakannya untuk sistem secara keseluruhan, Anda dapat memindahkannya ke direktori `/usr/local/bin`.

Langkah berikutnya adalah melakukan instalasi rizin secara manual. Pertama, lakukan kloning terhadap repositori rizin:

```bash
$ git clone --recurse-submodules https://github.com/rizinorg/rizin
```

Selanjutnya, pindah ke sub direktori hasil kloning dan lakukan _build_ sebelum melakukan instalasi. Untuk instalasi pada sistem sehingga dapat digunakan oleh semua pengguna, maka gunakan langkah berikut ini:

```bash
$ meson build
$ ninja -C build                # atau gunakan perintah `meson compile -C build`
$ sudo ninja -C build install   # atau gunakan perintah `sudo meson install -C build`
```

Namun, jika Anda hanya ingin melakukan instalasi untuk diri Anda sendiri, maka gunakan perintah berikut ini:

```bash
$ meson --prefix=~/.local build
$ ninja -C build
$ ninja -C build install
```

Perintah di atas akan melakukan instalasi rizin pada direktori `~/.local/bin`. Langkah terakhir adalah menjalankan rizin menggunakan perintah berikut ini:

```bash
$ rizin -v
rizin 0.1.0-git 25553 @ linux-x86-64 git.0.1.0-git
commit: e4cdedcb3034900398ac8dcc476a6fafe1b3b25c build: 2021-01-17__19:22:00
```

Jika Anda ingin menambahkan plugin ghidra untuk rizin, gunakan perintah berikut ini:

```bash
$ rz-pm update
$ rz-pm -i rz-ghidra
```

Setelah menjalankan langkah di atas, maka plugin ghidra pada rizin sudah dapat digunakan. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
