##1.找控件 ButterKnife

    AS中下载ButterKnife 

    app的build.gradle中加入 
    compile 'com.jakewharton:butterknife:7.0.1'
    鼠标点击到布局文件上，右键，generate，generate ButterKnife
    然后就好了
    setContentView(R.layout.activity_system_player)；
   
##2.RecycleView

    高度的解耦，异常的灵活，自己不用管复用item，不过监听需要自己写

    使用找到library，导入recycleView的V7包，Recycleview嵌套Recycleview高度设置为wrap_content，子item不显示（compile 

    'com.android.support:recyclerview-v7:23.2.0'之后才行，之前的版本有bug）
    
     //注意recycleview必须要加上这一句
    recycleview.setLayoutManager(new LinearLayoutManager(mContext,LinearLayoutManager.VERTICAL,false));
    //加载布局
    LayoutInflater.from(context).inflate(R.layout.simple_list_item_1, parent, false);
    这样加载布局可以使得布局文件中根节点如果只需要一个控件的话，不用外面套一个linearlayout

##3.第三方框架，LRecycleView 

    集成了SwipeMenu系列功能，包括item侧滑菜单，长按拖拽item，滑动删除item等功能。

    https://raw.githubusercontent.com/jdsjlzx/LRecyclerView/master/app/app-release.apk

##4.Glide加载图片

    可以加载gif，picasso不能加载gif，但是图片的质量高，Glide也可以设置图片的质量

##5.okhttputils 网络请求数据

##6.MaterialRefreshLayout RecycleView的下拉刷新，上拉加载

##7.photoView 点击图片后出现在一个自定义的Activity中，可以缩放

##8.Tablayout 和 viewpager一起绑定使用，可以一起滑动。

    导入的库：com.android.support:design:23.0.1

##9.第三方的slidingmenu 侧滑按钮，DrawerLayout谷歌官方推荐的组件，抽屉布局，在用抽屉布局的时候，
里面的最上面是个framlayout，把需要写的布局写在那个里面。

##10.EventBus 

     EventBus是一款针对Android优化的发布/订阅事件总线。主要功能是替代Intent,Handler,BroadCast在Fragment，
     Activity，Service，线程之间传递消息.优点是开销小，代码更优雅。以及将发送者和接收者解耦。
     
     新版本是3.0,不兼容老版本
     EventBus3.0新版本特性

    1.可以用注解的方式配置

    2.可以配置优先级，优先级的数值越大，就优先执行

    3.订阅函数不需要都以onEvent开头了（此处有坑，订阅函数一定要写成public）
    
    //1.注册eventBus
        EventBus.getDefault().register(this);
    //2.eventBus 订阅函数(这个方法一定要是public)
    @Subscribe(threadMode = ThreadMode.MAIN)
    public void showDataView(MediaItem mediaItem) {
    //3.enventBus 事件发布（其他类，发布的）
        EventBus.getDefault().post(mediaItem);
    //4.取消注册
        EventBus.getDefault().unregister(this);
    
##11.fragment的框架，基本都是用fragment实现的各个功能

##12.代码的混淆

    buildTypes {
        release {
            minifyEnabled false  // 把这个false改为true
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro' 
            //工具的路径：C:\xxx\xxx\sdk\tools\proguard
        }
      在proguard-rules.pro文件中加入这些：
      
        #指定代码的压缩级别
     -optimizationpasses 5

     #包明不混合大小写
     -dontusemixedcaseclassnames

     #不去忽略非公共的库类
     -dontskipnonpubliclibraryclasses

      #优化  不优化输入的类文件
     -dontoptimize

      #预校验
     -dontpreverify

      #混淆时是否记录日志
     -verbose

      # 混淆时所采用的算法
     -optimizations !code/simplification/arithmetic,!field/*,!class/merging/*

     #保护注解
     -keepattributes *Annotation*

     # 保持哪些类不被混淆
     -keep public class * extends android.app.Fragment
     -keep public class * extends android.app.Activity
     -keep public class * extends android.app.Application
     -keep public class * extends android.app.Service
     -keep public class * extends android.content.BroadcastReceiver
     -keep public class * extends android.content.ContentProvider
     -keep public class * extends android.app.backup.BackupAgentHelper
     -keep public class * extends android.preference.Preference
     -keep public class com.android.vending.licensing.ILicensingService
     #如果有引用v4包可以添加下面这行
     -keep public class * extends android.support.v4.app.Fragment



     #忽略警告
     -ignorewarning

     ##记录生成的日志数据,gradle build时在本项目根目录输出##

     #apk 包内所有 class 的内部结构
     -dump class_files.txt
     #未混淆的类和成员
     -printseeds seeds.txt
     #列出从 apk 中删除的代码
     -printusage unused.txt
     #混淆前后的映射
     -printmapping mapping.txt

     ########记录生成的日志数据，gradle build时 在本项目根目录输出-end######


     #####混淆保护自己项目的部分代码以及引用的第三方jar包library#######

     #-libraryjars libs/umeng-analytics-v5.2.4.jar

     #三星应用市场需要添加:sdk-v1.0.0.jar,look-v1.0.1.jar
     #-libraryjars libs/sdk-v1.0.0.jar
     #-libraryjars libs/look-v1.0.1.jar

     #如果不想混淆 keep 掉
     -keep class com.lippi.recorder.iirfilterdesigner.** {*; }
     #友盟
     -keep class com.umeng.**{*;}
     #项目特殊处理代码

     #忽略警告
     -dontwarn com.lippi.recorder.utils**
     #保留一个完整的包
     -keep class com.lippi.recorder.utils.** {
         *;
      }

     -keep class  com.lippi.recorder.utils.AudioRecorder{*;}


     #如果引用了v4或者v7包
     -dontwarn android.support.**

     ####混淆保护自己项目的部分代码以及引用的第三方jar包library-end####

     -keep public class * extends android.view.View {
         public <init>(android.content.Context);
         public <init>(android.content.Context, android.util.AttributeSet);
         public <init>(android.content.Context, android.util.AttributeSet, int);
         public void set*(...);
     }

     #保持 native 方法不被混淆
     -keepclasseswithmembernames class * {
         native <methods>;
     }

     #保持自定义控件类不被混淆
     -keepclasseswithmembers class * {
         public <init>(android.content.Context, android.util.AttributeSet);
     }

     #保持自定义控件类不被混淆
     -keepclassmembers class * extends android.app.Activity {
        public void *(android.view.View);
     }

     #保持 Parcelable 不被混淆
     -keep class * implements android.os.Parcelable {
       public static final android.os.Parcelable$Creator *;
     }

     #保持 Serializable 不被混淆
     -keepnames class * implements java.io.Serializable

     #保持 Serializable 不被混淆并且enum 类也不被混淆
     -keepclassmembers class * implements java.io.Serializable {
         static final long serialVersionUID;
         private static final java.io.ObjectStreamField[] serialPersistentFields;
         !static !transient <fields>;
         !private <fields>;
         !private <methods>;
         private void writeObject(java.io.ObjectOutputStream);
         private void readObject(java.io.ObjectInputStream);
         java.lang.Object writeReplace();
         java.lang.Object readResolve();
     }

     #保持枚举 enum 类不被混淆 如果混淆报错，建议直接使用上面的 -keepclassmembers class * implements java.io.Serializable即可
     #-keepclassmembers enum * {
     #  public static **[] values();
     #  public static ** valueOf(java.lang.String);
     #}

     -keepclassmembers class * {
         public void *ButtonClicked(android.view.View);
     }

     #不混淆资源类
     -keepclassmembers class **.R$* {
         public static <fields>;
     }

     #避免混淆泛型 如果混淆报错建议关掉
     #–keepattributes Signature

     #移除log 测试了下没有用还是建议自己定义一个开关控制是否输出日志
     #-assumenosideeffects class android.util.Log {
     #    public static boolean isLoggable(java.lang.String, int);
     #    public static int v(...);
     #    public static int i(...);
     #    public static int w(...);
     #    public static int d(...);
     #    public static int e(...);
     #}

     #如果用用到Gson解析包的，直接添加下面这几行就能成功混淆，不然会报错。
     #gson
     #-libraryjars libs/gson-2.2.2.jar
     -keepattributes Signature
     # Gson specific classes
     -keep class sun.misc.Unsafe { *; }
     # Application classes that will be serialized/deserialized over Gson
     -keep class com.google.gson.examples.android.model.** { *; }
    }
    
  ##13.app的签名

     点击build，Generate signed apk ->选择要打包的文件，比如app->create new path->路径的文件名别写包名，
     alias是别名的意思，可以写个与上面文件不同的名字，第二次出现的输入密码与之前的密码不要一样，
     ok 后，就会生成.jks的文件然后就可以用它来打包apk。
     
