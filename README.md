# Android-Prior-Knowledge
经常用到的几个重要的知识

1.http协议，Android网络请求

2.堆和栈应该知道的  和垃圾回收机制GC

3.内存泄漏与解决，还有AS查看布局，内存，耗时，泄漏位置的工具

4.三级缓存的原理

 1.先从内存中找图片，不行的话走 2 ，
 2.从本地文件中找图片，如果有显示并往内存里缓存，没有走 3
 3.从网络请求图片，显示，然后往内存存一份，本地文件存一份

内存中缓存图片使用的是系统的LruCache<String, Bitmap>类，采用最近未使用的算法。缓存图片的内存是系统分配给应用
程序的八分之一内存来作为缓存大小。
往本地文件缓存的时候需要判断sd卡是否挂载，记得写sd卡的读写权限。

5.TabLayout的使用，与ViewPager搭配。
