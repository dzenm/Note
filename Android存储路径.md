```Java
getFileDir()
/data/data/com.din/files

getCacheDir()
/data/data/com.din/cache

getExternalCacheDir()
/storage/emulated/0/Android/data/com.din/cache

getExternalFilesDir()
/storage/emulated/0/Android/data/com.din/files

getExternalFilesDir("image")
/storage/emulated/0/Android/data/com.din/files/image

getPackageName()
com.din

getPackageCodePath()
/data/app/com.din-1.apk

getDatabasePath("config.db")
/data/data/com.din/databases/config.db

getPackageResourcePath()
/data/app/com.din-1.apk

getDir("temp",Context.MODE_PRIVATE)
/data/data/com.din/app_temp

Environment.getExternalStorageDirectory()
/storage/emulated/0

Environment.getDataDirectory()
/data

Environment.getDownloadCacheDirectory()
/cache

Environment.getRootDirectory()
/system

Environment.getExternalStoragePublicDirectory("others")
/storage/emulated/o/others


Environment.DIRECTORY_ALARMS
//标准的铃声目录

Environment.DIRECTORY_DCIM
//相机拍照或者录像文件的存储目录

Environment.DIRECTORY_DOWNLOADS
//下载目录

Environment.DIRECTORY_MOVIES
//电影目录

Environment.DIRECTORY_MUSIC
//音乐目录

Environment.DIRECTORY_NOTIFICATIONS
//提示音目录

Environment_DIRECTORY_PICTURE
//图片目录

Environment_DIRECTORY_PODCASTS
//播客目录

Environment.DIRECTORY_RINGTONES
//铃声目录

Environment.MEDIA_BAD_REMOVAL 
//非法移除状态：移除sdcard之前，没有卸载sdcard。  

Environment.MEDIA_CHECKING 
//检查状态：检查sdcard的有效性,正在磁盘检查  

Environment.MEDIA_MOUNTED 
//挂载状态：sdcard卡已经成功挂载,并具有读/写权限  

Environment.MEDIA_MOUNTED_READ_ONLY
//只读状态：sdcard已经挂载，但是是只读的。  

Environment.MEDIA_NOFS 
//NOFS状态：识别到sdcard卡，但无法挂载。无法挂载原因，可能是sdcard无存储介质，或者sdcard卡的文件系统与android无兼容。  

Environment.MEDIA_REMOVED 
//移除状态：sdcard成功移除  

Environment.MEDIA_SHARED 
//SDCard 共享状态：识别到sdcard卡，但sdcard未挂载，而是作为mass storage等设备(如以u盘的方式连接到电脑上)。  

Environment.MEDIA_UNMOUNTABLE 
//无法挂载状态：识别到sdcard卡，但无法挂载。无法挂载的原因，可能是sdcard的存储介质部分损坏。  

Environment.MEDIA_UNMOUNTED 
//未挂载：识别到sdcard，但没有挂载 

//	"/"应该用File.separator来替代显得更专业些
```



