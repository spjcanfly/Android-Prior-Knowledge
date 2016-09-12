##1.找控件 ButterKnife

    AS中下载ButterKnife 

    app的build.gradle中加入 
    compile 'com.jakewharton:butterknife:7.0.1'
    鼠标点击到布局文件上，右键，generate，generate ButterKnife
    然后就好了
    setContentView(R.layout.activity_system_player)；
   
##2.RecycleView

    高度的解耦，异常的灵活，自己不用管复用item，不过监听需要自己写

    使用找到library，导入recycleView的V7包

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

##9.slidingmenu 侧滑按钮

##10.EventBus 

     EventBus是一款针对Android优化的发布/订阅事件总线。主要功能是替代Intent,Handler,BroadCast在Fragment，
     Activity，Service，线程之间传递消息.优点是开销小，代码更优雅。以及将发送者和接收者解耦。
     
     新版本是3.0,不兼容老版本
     EventBus3.0新版本特性

    1.可以用注解的方式配置

    2.可以配置优先级，优先级的数值越大，就优先执行

    3.订阅函数不需要都以onEvent开头了
    
