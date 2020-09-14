## [Writeup] Cyber Talents: Mobile App


Tutorial kali ini akan membahas solusi untuk soal CTF [Cyber Talents: Mobile App](https://cybertalents.com/challenges/malware/mobile-app) yang masuk ke dalam kategori reverse engineering. Berikut ini adalah petunjuk untuk soal kali ini:

```
Reverse engineer the APK to find the flag.
```

Soal kali ini mengharuskan kita untuk mengunduh sebuah aplikasi untuk Android dengan tipe APK pada [tautan ini](https://s3-eu-west-1.amazonaws.com/hubchallenges/Reverse/mobapp.apk). Unduh terlebih dahulu aplikasi tersebut. Setelah mengunduh aplikasi tersebut, kemudian ekstrak menggunakan aplikasi unzip atau [7zip](https://www.7-zip.org/) seperti ini:

```bash
% 7z x -oekstrak mobapp.apk
```

Hasil ekstraksi akan disimpan pada sub direktori **ekstrak**. Selanjutnya, kita dapat melakukan dekompilasi terhadap file **classes.dex** pada sub direktori tersebut menggunakan [jadx](https://github.com/skylot/jadx) atau aplikasi lainnya. Setelah mengamati hasil dekompilasi aplikasi tersebut, kita memperoleh informasi dari konfigurasi cordova, yaitu **org.apache.cordova.Config** berikut ini:

```java
package org.apache.cordova;

import android.app.Activity;
import android.content.Context;
import java.util.List;

@Deprecated
public class Config {
    private static final String TAG = "Config";
    static ConfigXmlParser parser;

    private Config() {
    }

    public static void init(Activity action) {
        parser = new ConfigXmlParser();
        parser.parse((Context) action);
        parser.getPreferences().setPreferencesBundle(action.getIntent().getExtras());
    }

    public static void init() {
        if (parser == null) {
            parser = new ConfigXmlParser();
        }
    }

    public static String getStartUrl() {
        if (parser == null) {
            return "file:///android_asset/www/index.html";
        }
        return parser.getLaunchUrl();
    }

    public static String getErrorUrl() {
        return parser.getPreferences().getString("errorurl", (String) null);
    }

    public static List<PluginEntry> getPluginEntries() {
        return parser.getPluginEntries();
    }

    public static CordovaPreferences getPreferences() {
        return parser.getPreferences();
    }

    public static boolean isInitialized() {
        return parser != null;
    }
}
```

Bisa terlihat bahwa konfigurasi untuk halaman awal berada pada _asset_ aplikasi tersebut. Coba kita periksa direktori **assets** pada sub direktori hasil ekstraksi:

```bash
% cd ekstrak
% tree assets
assets
└── www
    ├── cordova.js
    ├── cordova-js-src
    │   ├── android
    │   │   ├── nativeapiprovider.js
    │   │   └── promptbasednativeapi.js
    │   ├── exec.js
    │   ├── platform.js
    │   └── plugin
    │       └── android
    │           └── app.js
    ├── cordova_plugins.js
    ├── css
    │   └── index.css
    ├── img
    │   └── logo.png
    ├── index.html
    └── js
        └── index.js

8 directories, 11 files
```

Bisa terlihat bahwa, pada sub direktori **assets/www** terdapat file **index.html**. Coba kita lihat isinya:

```bash
% cat assets/www/index.html
<!DOCTYPE html>
<!--
    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
     KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.
-->
<html>
    <head>
        <!--
        Customize this policy to fit your own app's needs. For more guidance, see:
            https://github.com/apache/cordova-plugin-whitelist/blob/master/README.md#content-security-policy
        Some notes:
            * gap: is required only on iOS (when using UIWebView) and is needed for JS->native communication
            * https://ssl.gstatic.com is required only on Android and is needed for TalkBack to function properly
            * Disables use of inline scripts in order to mitigate risk of XSS vulnerabilities. To change this:
                * Enable inline JS: add 'unsafe-inline' to default-src
        -->
        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: https://ssl.gstatic.com 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src *; img-src 'self' data: content:;">
        <meta name="format-detection" content="telephone=no">
        <meta name="msapplication-tap-highlight" content="no">
        <meta name="viewport" content="user-scalable=no, initial-scale=1, maximum-scale=1, minimum-scale=1, width=device-width">
        <link rel="stylesheet" type="text/css" href="css/index.css">
        <title>Hello World</title>
    </head>
    <body>
        <div class="app">
            <h1>BlueKaizen</h1>
            <div id="deviceready" class="blink">
                <p class="event listening">Connecting to Device</p>
                <p class="event received">Device is Ready</p>
            </div>
        </div>
        <script type="text/javascript" src="cordova.js"></script>
        <script type="text/javascript" src="js/index.js"></script>
    </body>
</html>
```

Bisa terlihat bahwa halaman tersebut menggunakan 2 buah javascript, yaitu **cordova.js** dan **js/index.js**. Setelah mengamati isi dari file **cordova.js**, maka tidak ada hal yang mencurigakan. Namun berbeda dengan file **index.js**. Perhatikan potongan isi dari file **index.js** berikut ini:

```bash
% cat assets/www/js/index.js
...
app.initialize();
function whereIsMyFlag() {
        var flag = "W11bKCFbXStbXSlbK1tdXSsoWyFbXV0rW11bW11dKVsrIStbXStbK1tdXV0rKCFbXStbXSlbIStbXSsh ...
        W10rIStbXSshK1tdKyErW10rIStbXSshK1tdKyErW11dKyhbXVtbXV0rW10pWyErW10rIStbXV0pKSgp";
}
```

Bisa terlihat bahwa file **index.js** tersebut berisi sebuah fungsi `whereIsMyFlag` yang di dalamnya terdapat variabel `flag` dengan _string_ yang cukup panjang. Copy _string_ tersebut, dan simpan sebagai file **flag.base64**. Gunakan perintah berikut ini untuk melakukan decode terhadap file tersebut:

```bash
% base64 -d < flag.base64 > flag.dec
```

Hasilnya akan disimpan pada file **flag.dec**. Ternyata hasil decode tersebut adalah file yang diobfuscate:

```bash
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+ ... [+[]])[+[]]+[!+[]+!+[]+!+[]+!+[]+!+[]+!+[]+!+[]]+([][[]]+[])[!+[]+!+[]]))()
```

File tersebut diobfuscate menggunakan **JSFuck**, dan kita dapat men-deobfuscatenya menggunakan [Decoder JS-Fuck](https://enkhee-osiris.github.io/Decoder-JSFuck/). Setelah men-deobfuscate, maka kita akan menemukan flagnya, yaitu `FLAG{JS_AND_CORDOVA_FTW}`. Sekian tutorial singkat kali ini, semoga bermanfaat. Terima kasih kepada Allah SWT, dan Anda yang telah membaca tutorial ini.
