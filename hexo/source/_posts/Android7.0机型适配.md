---
title: Android7.0机型适配
date: 2017-04-06 22:30:20
tags:
---
### Android7.0机型适配存储目录的共享
都知道现在AI都很火，AI必定会诞生一些下一代巨头企业，各行各业都争抢这一股风，百度CEO不扔下了公司的内务不管，一头扎进了AI的研究团队工作中，前段时间李彦宏驾着自己公司生产的无人驾驶汽车，就是6，各个大巨头都在AI方面有重要项目在实施，都争抢着下一个时代的山头。在物联网方面，做的不错的是小米，小米的生态链，聚集了70多家智能硬件公司，形成了小米生态链，未来的小米将会在物联网将是一个大赢家，我们知道的小米路由器，小米手环，小米扫地机器人等都是这70多家公司中来生产和研发的，雷军太有远见了。好了，不闲说了，进入正题，记录一下我吗7.0版本对使用存储的要求。![](https://user-gold-cdn.xitu.io/2017/11/9/273e2014d8fa7673c6d438ebe50a592f?w=640&h=426&f=jpeg&s=47213)
![](https://user-gold-cdn.xitu.io/2017/11/9/ec8c02abbf21a89ed4daffd611b8e6d5?w=640&h=457&f=jpeg&s=42366)
  
  
	最近根据需求做了在线更新app问题，起初没有对7.0以上的机型进行
	适配，造成7.0手机无法跳转到app安装界面。
	   说一下吧，android系统发展越来越好用，用户体验越来越好，对
	用户的隐私也进行了一定规范性的保护，我们都知道从6.0系统的开始
	goole就开始对app使用手机权限进行了一定的约束，对于6.0以上 
	的权限申请我就不多说了，对于权限申请，自己写了一个框架，在项
	目中使用很方便。下面我们就说说7.0系统对存储使用增加
	了“FileProvider”,我们来看看使用方式，阅读一下官方文档。

##### FileProvider介绍

<a>FileProvider is a special subclass of ContentProvider that facilitates secure sharing of files associated with an app by creating a content:// Uri for a file instead of a file:/// Uri.
A content URI allows you to grant read and write access using temporary access permissions. When 
you create an Intent containing a contentURI, in 
order to send the content URI to a client app, you 	can also call Intent.setFlags() to add permissions. These permissions are available to the client app for as long as the stack for a receiving Activity is active. For an Intent going to a Service, the permissions are available as long as the Service is running.

In comparison, to control access to a file:/// Uri you have to modify the file system permissions of the underlying file. The permissions you provide become available to any app, and remain in effect until you change them. This level of access is fundamentally insecure.

The increased level of file access security offered by a content URI makes FileProvider a key part of Android's security infrastructure.

This overview of FileProvider includes the following topics:<a/>

 意思就是说
 <a>
 FileProvider是一个特殊的ContentProvider子类，它通过为文件创建一个Content:/ / Uri而不是file:/ / Uri，从而促进与应用程序关联的文件的安全共享。

内容URI允许您使用临时访问权限授予读和写访问。当您创建包含内容URI的意图时，为了将内容URI发送到客户机应用程序，您还可以调用intent. setflags()来添加权限。只要接收活动的堆栈是活动的，客户机应用程序就可以使用这些权限。为了获得服务，只要服务运行，权限就可用。

相比之下，为了控制对文件的file:/ / / Uri，您必须修改底层文件的文件系统权限。您提供的权限对任何应用程序都可用，并且一直有效，直到您更改它们为止。这种级别的访问基本上是不安全的。

由内容URI提供的文件访问安全性的提高使得FileProvider成为Android安全基础设施的关键部分。
 <a/>
 
 
 
### 使用方式
1.在程序清单文件中添加代码：
	
	<manifest>
	...
	<application>
       ...
       <provider  android:name= "android.support.v4.content.FileProvid"  
            android:authorities="com.mydomain.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
                       <!--元数据-->
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_path" />
        </provider>
        ...
    </application>
    </manifest>
    
    
 com.mydomain更换成自己项目的包名，唯一性。

2.在res目录下新建一个xml文件夹，文件下新建一个xml文件“file_path”

3.file_path文件写法

	<?xml version="1.0" encoding="utf-8"?>
	<resources>
    <paths>
        <root-path name="download" path=""/>
    </paths>
    </resources>
    
 根据我们使用的存储路径不同，对第三层的配置也不一样。
 
 	（1）当我们使用Context.getFilesDir()获取路径时我们应该配
 	置成 <files-path name="name" path="path" />
 	（2）当我们使用getCacheDir()获取存储路径时，应该配置成
 	<cache-path name="name" path="path" />
 	（3）使用Environment.getExternalStorageDirectory()
 	获取目录时，<root-path name="download" path=""/>，path值可以为null，null代表授权这个目录下的所有文件，name必须写，不然会报严重异常
 	（4）使用Context.getExternalCacheDir()
 	<external-cache-path name="name" path="path" />
 	（5）使用Context.getExternalFilesDir(null)配置成
 	<external-files-path name="name" path="path" />

另外，我们在跳转到安装目录代码有变如下：

	 public static void install(Context context,String target) {
        File file = new File(target);
        Intent intent = new Intent(Intent.ACTION_VIEW);
        // 由于没有在Activity环境下启动Activity,设置下面的标签
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
        if(Build.VERSION.SDK_INT>=24) { //判读版本是否在7.0以上
            //参数1 上下文, 参数2 Provider主机地址 和配置文件中保持一致   参数3  共享的文件
            Uri apkUri =
                    FileProvider.getUriForFile(context, "com.gzzhe.zhhwlkj.zigou.fileprovider", file);
            //添加这一句表示对目标应用临时授权该Uri所代表的文件
            intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
            intent.setDataAndType(apkUri, "application/vnd.android.package-archive");
        }else{
            intent.setDataAndType(Uri.fromFile(file),
                    "application/vnd.android.package-archive");
        }
        context.startActivity(intent);
    }
 	
 
 
 
 好了就介绍到这儿吧。
