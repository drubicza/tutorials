## [Writeup] Cyber Talents: I Love This Guy


Tutorial singkat kali ini akan membahas solusi untuk soal CTF [Cyber Talents: I Love This Guy](https://cybertalents.com/challenges/malware/i-love-this-guy) yang termasuk ke dalam kategori _reverse engineering_. Petunjuk untuk soal tersebut adalah sebagai berikut:

```
Can you find the password to obtain the flag?
```

Dan kita akan diberikan sebuah file _executable_ yang dapat [diunduh pada tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Reverse/ScrambledEgg.exe). Setelah mengunduh aplikasi tersebut, kita kemudian akan memeriksa tipenya:

```bash
% file ScrambledEgg.exe
ScrambledEgg.exe: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows
```

Ternyata _executable_ tersebut dibuat menggunakan dotNet. Ada beberapa aplikasi yang dapat digunakan untuk melakukan _reverse engineering_ aplikasi dotNet, diantaranya [ILSpy](https://github.com/icsharpcode/ILSpy), [dotPeek](https://www.jetbrains.com/decompiler/), [dnSpy](https://github.com/0xd4d/dnSpy) dan masih banyak aplikasi lainnya. Aplikasi yang digunakan pada tutorial ini, adalah dotPeek. Jadi jalankan aplikasi dotPeek lalu buka file soal tersebut (ScrambledEgg.exe). Lakukan dekompilasi, maka akan muncul hasil berikut ini yang merupakan hasil dekompilasi dari **MainWindow.cs**:

```cs
// Decompiled with JetBrains decompiler
// Type: FirstWPFApp.MainWindow
// Assembly: FirstWPFApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
// MVID: A243FE8A-363D-4E8F-8D2E-99994B7D1889
// Assembly location: C:\ScrambledEgg.exe

using System;
using System.CodeDom.Compiler;
using System.ComponentModel;
using System.Diagnostics;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Markup;

namespace FirstWPFApp
{
  public class MainWindow : Window, IComponentConnector
  {
    public char[] Letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_".ToCharArray();
    internal TextBox TextBox1;
    internal Button Button1;
    private bool _contentLoaded;

    public MainWindow()
    {
      this.InitializeComponent();
    }

    private void Button_Click(object sender, RoutedEventArgs e)
    {
      if (!this.TextBox1.Text.Equals(new string(new char[5]
      {
        this.Letters[5],
        this.Letters[14],
        this.Letters[13],
        this.Letters[25],
        this.Letters[24]
      })))
        return;
      int num = (int) MessageBox.Show(new string(new char[18]
      {
        this.Letters[5],
        this.Letters[11],
        this.Letters[0],
        this.Letters[6],
        this.Letters[26],
        this.Letters[8],
        this.Letters[28],
        this.Letters[11],
        this.Letters[14],
        this.Letters[21],
        this.Letters[4],
        this.Letters[28],
        this.Letters[5],
        this.Letters[14],
        this.Letters[13],
        this.Letters[25],
        this.Letters[24],
        this.Letters[27]
      }));
    }

    [DebuggerNonUserCode]
    [GeneratedCode("PresentationBuildTasks", "4.0.0.0")]
    public void InitializeComponent()
    {
      if (this._contentLoaded)
        return;
      this._contentLoaded = true;
      Application.LoadComponent((object) this, new Uri("/FirstWPFApp;component/mainwindow.xaml", UriKind.Relative));
    }

    [DebuggerNonUserCode]
    [GeneratedCode("PresentationBuildTasks", "4.0.0.0")]
    [EditorBrowsable(EditorBrowsableState.Never)]
    void IComponentConnector.Connect(int connectionId, object target)
    {
      if (connectionId != 1)
      {
        if (connectionId == 2)
        {
          this.Button1 = (Button) target;
          this.Button1.Click += new RoutedEventHandler(this.Button_Click);
        }
        else
          this._contentLoaded = true;
      }
      else
        this.TextBox1 = (TextBox) target;
    }
  }
}
```

Perhatikan hasil dekompilasi di atas. Kita bisa melihat bahwa aplikasi tersebut hanya melakukan komparasi sederhana untuk password yang dimasukkan. Yaitu dengan menggunakan index dari _string_ **ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_**. Untuk password, berikut ini adalah cara menghitungnya berdasarkan index _string_ tersebut:

```cs
public char[] Letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_".ToCharArray();
...
this.Letters[5]  = F
this.Letters[14] = O
this.Letters[13] = N
this.Letters[25] = Z
this.Letters[24] = Y
```

Jadi, password yang benar adalah **FONZY**. Jika kita memasukkan password tersebut, maka kita akan memperoleh flag yang juga cara menghitungnya sama dengan cara menghitung password tersebut, yaitu menggunakan indeks dari _string_ **ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_**. Jika ingin menghitung flagnya secara manual, berikut adalah caranya menggunakan python:

```python
s = "ABCDEFGHIJKLMNOPQRSTUVWXYZ{}_"
i = [5,11,0,6,26,8,28,11,14,21,4,28,5,14,13,25,24,27]
print("".join(s[n] for n in i))
```

Dan hasilnya adalah `FLAG{I_LOVE_FONZY}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
