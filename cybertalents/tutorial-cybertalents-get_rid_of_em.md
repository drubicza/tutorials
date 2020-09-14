## [Writeup] Cyber Talents: Get Rid of Em


[Soal](https://s3-eu-west-1.amazonaws.com/hubchallenges/Reverse/get-rid-of-them.jar) kali ini adalah aplikasi java dalam kategori mudah. Berikut ini adalah informasi yang diberikan:

```
Someone corrupted the file so that we cannot execute it, and we need your help.
Obtain the flag ,format flag{xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
```

Selanjutnya, kita akan melakukan dekompilasi menggunakan jd-gui. Berikut ini adalah hasil dekompilasi berupa file `ctf.java`:

```java
package ctf;

import java.io.PrintStream;

public class Ctf
{
  static String flag = "&^&@|* Zm}&,);\\('))[\\[$`|_^#(x*]>&hZ)'$ $#(: [$3;&$t \\_']?&>,&i)!QG{`- ,% ~<`._@'::_\\_{}-|_[&{<`~$) ?'?(!$,.{>? @!^:#|R,?')`[,`;?!f_:$$<)Y}$:[|^?2)_h&><.:.-{&[|&A\\*;*)-($.>>(<^';#Q@?,,H\\`|)$ <):@(;}?-[~(&)>>*)(~)`$:[;>!.&%<!.>~ %J}*zX:(&:~:<0)*>(B(!?.#@A*<*{-,[Q@{%!~)~-~:@:#|![>)]?];H;$-<}>!@~)<<) \\_!|]#,&!,@>\\[]|J ]\\^[?>$|$?'|,#.)$l[^@X.~! \\;0-&,;,!['@[J*~#`AQ[*&%<,~]?~_^~(;}\\$>)[&@) (]}];;*^<)''@\\E[.@! B*.<-A-,:-#`-.}<-|)^Z@](?;H >-}.%.?}@<!())0] <&=@(<*$\\((";

  public static void main(String[] args)
  {
    if (args.length < 10) {
      System.out.println("No");
      return;
    }

    ooo o = new ooo();
    flag = o._2(flag, args);
    System.out.println(flag);
    System.out.println(o._1(flag));
  }
}
```

Bisa terlihat bahwa variabel `flag` akan digunakan sebagai parameter dalam pemanggilan fungsi `_2` dan kemudian memanggil fungsi `_1` yang akan menampilkan string flag pada file `ooo.java`. Berikut ini adalah hasil dekompilasi berupa file `ooo.java`:

```java
package ctf;

import java.io.PrintStream;
import java.util.Base64;
import java.util.Base64.Decoder;

public class ooo
{
  public String _1(String a)
  {
    try
    {
      return new String(Base64.getDecoder().decode(a));
    }
    catch (Exception e) {
      System.out.println("Wrong argsssss");
      System.out.println(a);
    }
    return "";
  }

  public String _2(String a, String[] b) {
    String temp = "";
    int i = 0;
    int j = 0;
    boolean bad = false;

    while (i != a.length())
    {
      j = 0;
      bad = false;

      while (j != b.length)
      {
        if (a.charAt(i) == Integer.parseInt(b[j]))
          bad = true;
        j++;
      }

      if (!bad)
        temp = temp + a.charAt(i);
      i++;
    }
    return temp;
  }
}
```

Dari source di atas, bisa terlihat bahwa fungsi `_2` akan melakukan penyaringan karakter yang tidak valid (bad char), sedangkan fungsi `_1` akan melakukan decode base64 terhadap string yang menjadi parameter fungsi tersebut. Langkah selanjutnya adalah, kita akan melakukan decode base64 terhadap string `flag` pada `ctf.java` dan agar lebih mudah, maka kita akan menggunakan script python berikut

```python
#!/usr/bin/env python
from base64 import b64decode

print b64decode("&^&@|* Zm}&,);\\('))[\\[$`|_^#(x*]>&hZ)'$ $#(: [$3;&$t \\_']?&>,&i)!QG{`- ,% ~<`._@'::_\\_{}-|_[&{<`~$) ?'?(!$,.{>? @!^:#|R,?')`[,`;?!f_:$$<)Y}$:[|^?2)_h&><.:.-{&[|&A\\*;*)-($.>>(<^';#Q@?,,H\\`|)$ <):@(;}?-[~(&)>>*)(~)`$:[;>!.&%<!.>~ %J}*zX:(&:~:<0)*>(B(!?.#@A*<*{-,[Q@{%!~)~-~:@:#|![>)]?];H;$-<}>!@~)<<) \\_!|]#,&!,@>\\[]|J ]\\^[?>$|$?'|,#.)$l[^@X.~! \\;0-&,;,!['@[J*~#`AQ[*&%<,~]?~_^~(;}\\$>)[&@) (]}];;*^<)''@\\E[.@! B*.<-A-,:-#`-.}<-|)^Z@](?;H >-}.%.?}@<!())0] <&=@(<*$\\((")
```

Simpan kode di atas sebagai `getflag.py`, lalu jalankan:

```bash
python getflag.py
flag{b@d_ch@@rs_@@@re_B@@@@d}
```

Maka flagnya akan ditampilkan, yaitu `flag{b@d_ch@@rs_@@@re_B@@@@d}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Tuhan Yang Maha Esa, dan Anda yang telah membaca tutorial ini.
