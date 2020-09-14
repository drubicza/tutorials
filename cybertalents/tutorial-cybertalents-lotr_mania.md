## [Writeup] Cyber Talents: LOTR Mania


Pada [soal](https://s3-eu-west-1.amazonaws.com/talentchallenges/Reverse/app.apk.zip) kali ini yang berada pada kategori mudah, diberikan sebuah aplikasi APK. Berikut ini adalah langkah-langkah yang dilakukan untuk menemukan flag pada soal ini:

Pertama ekstrak arsip soal `app.apk.zip`

```bash
% unzip app.apk.zip
```

Hasilnya adalah aplikasi `app.apk`. Lanjutkan dengan mengekstrak `app.apk` tersebut ke dalam sub direktori `app`:
```bash
% unzip app.apk -d app
```

Pindah ke sub direktori `app` dan gunakan `dex2jar` untuk melakukan konversi file `classes.dex` menjadi `classes.jar`:

```bash
% cd app
% dex2jar classes.dex
```

Selanjutnya, gunakan aplikasi `jd-gui` untuk melakukan dekompilasi terhadap file `classes.jar`. Berikut ini adalah hasil dekompilasinya:

```java
package com.labs.adnromeda.test1;

import android.content.Intent;
import android.os.Bundle;
import android.support.v7.app.ActionBarActivity;
import android.util.Log;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity
  extends ActionBarActivity
{
  public int weezy = 152;

  private String getPass()
  {
    if (this.weezy > 152) {
      return "Saruman";
    }
    return "Gandalf";
  }

  private String getUser()
  {
    if (this.weezy > 152) {}
    for (String str = "Legolas";; str = "Aragon")
    {
      this.weezy += 100;
      return str;
    }
  }

  protected void onCreate(final Bundle paramBundle)
  {
    super.onCreate(paramBundle);
    setContentView(2130903063);
    paramBundle = (EditText)findViewById(2131427393);
    final EditText localEditText = (EditText)findViewById(2131427394);
    ((Button)findViewById(2131427395)).setOnClickListener(new View.OnClickListener()
    {
      public void onClick(View paramAnonymousView)
      {
        paramAnonymousView = paramBundle.getText().toString();
        String str = localEditText.getText().toString();
        Log.i("credentials check", paramAnonymousView + ":" + str);
        if ((paramAnonymousView.compareTo(MainActivity.this.getUser()) == 0) && (str.compareTo(MainActivity.this.getPass()) == 0))
        {
          Log.i("credentials check", "granted access");
          Toast.makeText(MainActivity.this.getApplicationContext(), "access granted!", 0).show();
          paramAnonymousView = new Intent(MainActivity.this.getApplicationContext(), MainActivity2.class);
          MainActivity.this.startActivity(paramAnonymousView);
          return;
        }
        Toast.makeText(MainActivity.this.getApplicationContext(), "access denied!", 0).show();
      }
    });
  }

  public boolean onCreateOptionsMenu(Menu paramMenu)
  {
    getMenuInflater().inflate(2131492864, paramMenu);
    return true;
  }

  public boolean onOptionsItemSelected(MenuItem paramMenuItem)
  {
    if (paramMenuItem.getItemId() == 2131427398) {
      return true;
    }
    return super.onOptionsItemSelected(paramMenuItem);
  }
}
```

Perhatikan bagian berikut ini:

```java
if ((paramAnonymousView.compareTo(MainActivity.this.getUser()) == 0) && (str.compareTo(MainActivity.this.getPass()) == 0))
```

Pada bagian tersebut, jika fungsi `getUser()` dan `getPass()` nilainya `0`, maka akses akan diberikan. Selanjutnya, perhatikan kedua fungsi tersebut, serta variabel `weezy` berikut ini:

```java
public int weezy = 152;

private String getPass()
{
  if (this.weezy > 152) {
    return "Saruman";
  }
  return "Gandalf";
}

private String getUser()
{
  if (this.weezy > 152) {}
  for (String str = "Legolas";; str = "Aragon")
  {
    this.weezy += 100;
    return str;
  }
}
```

Berikut ini adalah penjelasan singkat tentang kedua fungsi tersebut, serta variabel `weezy`:
* Fungsi `getUser()` akan dipanggil pertama kali.
* String `str` akan bernilai `Legolas`.
* Variabel `weezy` nilainya akan ditambah `100`, sehingga nilainya menjadi `252`.
* Fungsi `getPass()` akan dipanggil.
* Karena nilai variabel `weezy` saat ini adalah `252`, maka fungsi `getPass()` akan bernilai `Saruman`.
* Gabungan kedua string tersebut digabungkan lalu dihitung hash MD5nya untuk memperoleh flag.

Dengan informasi di atas, kita dapat membuat script php sederhana untuk menghitung flag. Berikut ini adalah scriptnya:

```php
<?php
$weezy = 152;

function getUser()
{
    global $weezy;

    if ($weezy > 152) {}
    for ($str = "Legolas";; $str = "Aragon")
    {
        $weezy += 100;
        return $str;
    }
}

function getPass()
{
    global $weezy;
    if ($weezy > 152) {
        return "Saruman";
    }
    return "Gandalf";
}

echo md5(getUser() . getPass()) . "\n";
?>
```

Jika script tersebut dijalankan, maka akan menampilkan flag yang benar:

```bash
php -f getFlag.php
d710d29360684aef13ea7cdfecf63a3a
```

Jadi, flag untuk soal ini adalah `d710d29360684aef13ea7cdfecf63a3a`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
