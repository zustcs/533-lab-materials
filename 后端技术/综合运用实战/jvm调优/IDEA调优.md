# IDEA加速启动

众所周知，IDEA的启动速度一直很堪忧，在开发久了后，一般的电脑通常都要等个两分钟才能彻底唤醒它。现在我有个大胆的想法，设置JVM的参数，看看能否让它快速启动！

# VisualVM

VisualVM是一个多合一故障处理工具，在Java包的bin目录下，全称是jvisualvm.exe文件，双击打开它。在运行IDEA的时候，左边的应用程序栏会出现一个名为IntelliJ Platform进程，双击查看详情：

![image-20191113164648525](IDEA调优.assets/image-20191113164648525.png)

![image-20191113164758635](IDEA调优.assets/image-20191113164758635.png)

CPU活动的区间可大致推断出IDEA的启动时间为两分钟上下。同时对比堆与Metaspace，可以发现，在元空间的分配上，Metaspace大小与使用的Metaspace几乎重合在一起，这说明元空间中没有可回收的资源，所以使用的Metaspace不会向下发展，因此曲线不断向上扩展。

# 修改IDEA配置

打开IDEA安装目录bin目录下的idea.exe.vmoptions文件，如图所示：

![image-20191113172221943](IDEA调优.assets/image-20191113172221943.png)

修改如下：

```
-Xms1024m
-Xmx2048m
-XX:MetaspaceSize=512m
-XX:MaxMetaspaceSize=512m
-XX:ReservedCodeCacheSize=240m
-XX:+UseConcMarkSweepGC
-XX:SoftRefLRUPolicyMSPerMB=50
-ea
-Dsun.io.useCanonCaches=false
-Djava.net.preferIPv4Stack=true
-Djdk.http.auth.tunneling.disabledSchemes=""
-XX:+HeapDumpOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
```

重新启动观察，得到的曲线图如下：

![image-20191113172853454](IDEA调优.assets/image-20191113172853454.png)发现启动时间缩短了半分钟左右，但是可以发现元空间增长的曲线依旧是从40mb左右开始的，难道设置的参数不管用吗？让我们来看GC的信息：

![image-20191113173004261](IDEA调优.assets/image-20191113173004261.png)

发现相较于之前的18次Full GC，修改了参数后降低为了1次Full GC，这是虚拟机主动调用的System.gc()。我们可以调用命令：`jstat -gccause 22832(pid在应用程序栏中可以看到)`。如图所示：

![image-20191113181610444](IDEA调优.assets/image-20191113181610444.png)

也就是说，修改参数是有效的，只是初始参数它是一个固定的值，并不会改动，只有当扩容到-XX:MetaspaceSize参数指定的值之后，才会开始执行Full GC。

对于Full GC，我们可以再次优化，添加参数：`-XX:+DisableExplicitGC`屏蔽掉System.gc()。

