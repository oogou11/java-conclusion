# JVM 与 GC面试知识点总结  

## GC ROOT  

1. GC ROOT 是什么？
2. 哪些对象可以作为GC ROOT  

## JVM的参数类型  
> _查看线上的JVM运行状况命令_
>-jps -l: java process server,可以查看对应的java进程ID  
>-jinfo -flag params pid
-------------------------------  
1. 标配参数  
>-version  
>-help  
>-showversion  
1. X参数  
>-Xint -解释执行  
>-Xcomp 第一次使用就编译成本地代码  
>-Xmixed 混合模式  
2. XX参数  
>_Boolean类型_
>-XX:+（开启）活着-（关闭）某个属性值
_KV设置类型_
-XX:属性key=属性值value
3. demo:  
> 堆空间大小设置-XX:MetaspaceSize=128m;-XXMaxTe 
> 多大年龄后可以申请养老区-XX:MaxTenuringThreshold=15
> -Xms1024m = -XX:InitialHeapSize=1024m 默认内存的1/64
> -Xmx1024m = -XX:MaxHeapSize=1204 --默认物理内存的 1/4
4. 其它参数  
>（出场设置） java -XX:+PrintFlagsInitial 判断JVM家底
> 修改后怎么查看 java -XX:PringFlagsFinal -version
> = 是初始的，:=是修改后的值
> 默认垃圾回收期 java -XX:+PrintCommandLineFlags
> -Xss = -XX:ThreadStackSize 栈空间初始化内存大小  
> -Xmn 设置年轻代内存大小
> -XX:MetaspaceSize(java7-永久代，java8-元空间，尽受本地内存空间的限制)  默认21807104=21MB；应该配置大点


## 常配置的JVM参数
> -Xms128m  -XX:InitialHeapSize 物理内存的 1/64 初始化堆内存大小
> -Xmx4098m  -XX:MaxHeapSize 物理内存的的 1/4 最大堆内存大小
> -Xss1024k  初始化栈的大小
> -XX:MetaspaceSize=512M  元空间大小
> -XX:+PrintCommandLineFlags  打印JVM参数
> -XX:+PrintGCDetails  打印出GC回收的详细参数
> -XX:+UseSerialGC 串行GC回收器

## OOM种类

## GC 参数
> -XX:+UserSerialGC  串行垃圾回收器
> -XX:+UserParalleGC 并行垃圾回收器（默认）