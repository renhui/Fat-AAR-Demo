# Fat-AAR-Demo
实践诉求：模块化 - 组件化，开发时，多个module合并输出一个AAR文件。



#### 一、AAR文件结构：
它的文件后缀名是.aar，它本身是一个zip文件，强制包含以下文件：
```
/AndroidManifest.xml
/classes.jar
/res/
/R.txt
```
另外，AAR文件可以包括以下可选条目中的一个或多个：
```
/assets/
/libs/name.jar
/jni/abi_name/name.so
/proguard.txt
/lint.jar
```

假设有两个libs，分别为module1和module2。此时，为了合成一个aar，需要将module2中的.jar,res,assets中的文件资源插入到module1中，将AndroidManifest.xml和R.txt相互合并Merge。

 **AAR的合并问题的核心就是将一个file下的文档拷贝到另一个下面，合并名称相同的两个文件，这些操作都是在恰当的Task执行前或后插入的。**



#### 二、AAR合并执行时的问题

1. 如何确认需要合并的module？
2. 确定需要合并的module后，如何找到相应的文件路径？
3. 不同的文件在Gradle Tasks中的哪个位置插入？



这里根据：https://github.com/kezong/fat-aar-android 开源项目来理解一下。



1. 通过 embed 来确认需要add 的 dependency，这样就能将需要合并的关联起来了。

2. 确认需要合并的module后，如何找到路径？

   答：build一个aar的适合会在各个module下生成一个build文件夹，和aar关联的文件夹是exploded-aar，里面的代码结构和我们直接打开aar的时候基本一致。

   这样，路径问题我们就解决了。

3. 不同的文件在Gradle Tasks中的哪个位置插入？
   fat-aar是这样做的，通过不同的task之间的dependsOn ，mustRunAfter 这种方式来插入自定义的task。



#### 三、推荐阅读

https://www.jianshu.com/p/f88ff677ac95?t=1490962970518



