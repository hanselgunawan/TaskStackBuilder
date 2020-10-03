# TaskStackBuilder
If we’re building a notification that points to an Activity deep within your app, there’s one case we want to avoid: tapping the back button should **NOT** close the app.

## Implementation Steps
### Declare Parent of DetailActivity on `AndroidManifest.xml` file
For Android 4.1 or above:
```
<activity android:name=".Activities.DetailActivity"
   android:parentActivityName=".Activities.MainActivity">
</activity>
```
For Android 4.0 or below:
```
<activity android:name=".Activities.DetailActivity"
   android:parentActivityName=".Activities.MainActivity">
   <meta-data
     android:name="android.support.PARENT_ACTIVITY"
     android:value=".Activities.MainActivity" />
</activity>
```

### Use `TaskStackBuilder` to create a `PendingIntent`
```
Intent detailActivity = new Intent(this, DetailActivity.this);
TaskStackBuilder stackBuilder = TaskStackBuilder.create(this);
stackBuilder.addNextIntentWithParentStack(detailActivity);
PendingIntent pendingIntent = stackBuilder
 .getPendingIntent(0, PendingIntent.FLAG_UPDATE_CURRENT);
```

### Pass the `pendingIntent` to the `Notification` object
```
val notification: Notification = NotificationCompat.Builder(this, "channelId")
            .setSmallIcon(R.drawable.ic_notification)
            .setContentTitle("Notification Test")
            .setContentText("Hey, This is just a test.")
            .setContentIntent(pendingIntent)
            .build()
```

## App Demo
### Before Implementing `TaskStackBuilder`
<img src="https://i.gyazo.com/72ea26a83533d8f5de24281231414035.gif" width="350px" height="650px" />

### After Implementing `TaskStackBuilder`
<img src="https://i.gyazo.com/68ffa18c1a55ab4b5a080f8cf03923bf.gif" width="350px" height="650px" />
