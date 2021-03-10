### 使用方法介绍
- 2.0 如何使用该库
```
compile 'cn.yc:notificationLib:1.0.2'
```
- 2.1 最简单调用方式
```
NotificationUtils notificationUtils = new NotificationUtils(this);
notificationUtils.sendNotification(1,"这个是标题","这个是内容",R.mipmap.ic_launcher);

//在NotificationUtils构造方法中
public NotificationUtils(Context base) {
    super(base);
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        //android 8.0以上需要特殊处理，也就是targetSDKVersion为26以上
        createNotificationChannel();
    }
}
```

- 2.2 完整使用方法介绍
```
long[] vibrate = new long[]{0, 500, 1000, 1500};
//处理点击Notification的逻辑
//创建intent
Intent resultIntent = new Intent(this, TestActivity.class);
resultIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);           //添加为栈顶Activity
resultIntent.putExtra("what",3);
PendingIntent resultPendingIntent = PendingIntent.getActivity(this,3,resultIntent,PendingIntent.FLAG_UPDATE_CURRENT);
//发送pendingIntent

NotificationUtils notificationUtils = new NotificationUtils(this);
notificationUtils
        //让通知左右滑的时候是否可以取消通知
        .setOngoing(true)
        //设置内容点击处理intent
        .setContentIntent(resultPendingIntent)
        //设置状态栏的标题
        .setTicker("来通知消息啦")
        //设置自定义view通知栏布局
        .setContent(getRemoteViews())
        //设置sound
        .setSound(android.provider.Settings.System.DEFAULT_NOTIFICATION_URI)
        //设置优先级
        .setPriority(Notification.PRIORITY_DEFAULT)
        //自定义震动效果
        .setVibrate(vibrate)
        //必须设置的属性，发送通知
        .sendNotification(3,"这个是标题3", "这个是内容3", R.mipmap.ic_launcher);
```

- 2.3 发送通知处理逻辑，解决8.0通知栏问题
```
public void sendNotification(int notifyId, String title, String content , int icon) {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        //android 8.0以上需要特殊处理，也就是targetSDKVersion为26以上
        //通知用到NotificationCompat()这个V4库中的方法。但是在实际使用时发现书上的代码已经过时并且Android8.0已经不支持这种写法
        Notification.Builder builder = getChannelNotification(title, content, icon);
        getManager().notify(notifyId, builder.build());
    } else {
        NotificationCompat.Builder builder = getNotificationCompat(title, content, icon);
        getManager().notify(notifyId, builder.build());
    }
}
```
- 2.4 如果你想获取Notification对象，自己发送消息也可以
```
NotificationUtils notificationUtils = new NotificationUtils(this);
notificationUtils.setContent(getRemoteViews());
Notification notification = notificationUtils.getNotification("这个是标题4", "这个是内容4", R.mipmap.ic_launcher);
notificationUtils.getManager().notify(4,notification);
```

#### 关于LICENSE
```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```












