# -
文章
# -
文章
gradle
https://juejin.cn/post/6989877607126794247

https://juejin.cn/post/6947675376835362846


gradle 是什么 是构建工具，不是语言
它用了 Groovy 这个语言，创造了一种 DSL，但它本身不是语言 怎么构建?
按照 gradle 的规则(build.gradle、settings.gradle、gradle-wrapper、gradle 语法)
闭包
Java 的 Lambda 表达式:是单抽象方法(SAM)的接口的匿名类对象的快捷写 法，只是一个语法糖。
Kotlin 的 Lambda 表达式:和匿名函数相当，实质上是一个函数类型的对象， 并不只是语法糖。
Groovy 的 Lambda 表达式:Groovy 里不叫「Lambda」表达式，而是叫「闭 包」;在功能上，和 Kotlin 的 Lambda 比较相似，都是一个「可以传递的代码 块」，具体的功能比 Kotlin 的 Lambda 更强一些，但基本的概念是一样的。
为什么 Groovy 可以写出类似 JSON 格式的配 置?
因为它们其实都是方法调用，只是用闭包来写成了看起来像是 JSON 型的格式。

compile, implementation 和 api
implementation:不会传递依赖
compile / api:会传递依赖;api 是 compile 的替代品，效果完全等同 当依赖被传递时，二级依赖的改动会导致 0 级项目重新编译;当依赖不传递 时，二级依赖的改动不会导致 0 级项目重新编译
Gradle Wrapper
通过「只同步版本，不同步文件」的方式来减小协作项目的大小 每个人电脑上的 Gradle 存放在固定位置，然后使用 Gradle Wrapper 的配置来 取用对应的版本就行了

oFirst() doLast() 和普通代码段的区别:
普通代码段:在 task 创建过程中就会被执行，发生在 configuration 阶段 doFirst() 和 doLast():在 task 执行过程中被执行，发生在 execution 阶 段。如果用户没有直接或间接执行 task，那么它的 doLast() doFirst() 代码 不会被执行
doFirst() 和 doLast() 都是 task 代码，其中 doFirst() 是往队列的前面插入代 码，doLast() 是往队列的后面插入代码

ask 的依赖:可以使用 task taskA(dependsOn: b) 的形式来指定依赖。 指定依赖后，task 会在自己执行前先执行自己依赖的 task。
gradle 执行的生命周期 三个阶段:
初始化阶段:执行 settings.gradle，确定主 project 和子 project 定义阶段:执行每个 project 的 bulid.gradle，确定出所有 task 所组成的有向无 环图
执行阶段:按照上一阶段所确定出的有向无环图来执行指定的 task
在阶段之间插入代码: 一二阶段之间:
settings.gradle 的最后 二三阶段之间:

Gradle 常用命令
1、Gradle 查询命令
1）、查看主要任务
    ./gradlew tasks
复制代码
2）、查看所有任务，包括缓存任务等等
    ./gradlew tasks --all
复制代码
2、Gradle 执行命令
1）、对某个module [moduleName]   的某个任务[TaskName] 运行
    ./gradlew :moduleName:taskName
复制代码
3、Gradle 快速构建命令
Gradle 提供了一系列的快速构建命令来替代 IDE 的可视化构建操作，如我们最常用的 clean、build 等等。需要注意的是，build 命令会把 debug、release 环境的包都构建出来。
1）、查看构建版本
    ./gradlew -v
复制代码
2）、清除 build 文件夹
    ./gradlew clean
复制代码
3）、检查依赖并编译打包
    ./gradlew build
复制代码
4）、编译并安装 debug 包
    ./gradlew installDebug
复制代码
5）、编译并打印日志
    ./gradlew build --info
复制代码
6）、编译并输出性能报告，性能报告一般在构建工程根目录 build/reports/profile 下
    ./gradlew build --profile
复制代码
7）、调试模式构建并打印堆栈日志
    ./gradlew build --info --debug --stacktrace
复制代码
8）、强制更新最新依赖，清除构建后再构建
    ./gradlew clean build --refresh-dependencies
复制代码
9）、编译并打 Debug 包
    ./gradlew assembleDebug
    # 简化版命令，取各个单词的首字母
    ./gradlew aD
复制代码
10）、编译并打 Release 的包
    ./gradlew assembleRelease
    # 简化版命令，取各个单词的首字母
    ./gradlew aR
复制代码
4、Gradle 构建并安装命令
1）、Release 模式打包并安装
    ./gradlew installRelease
复制代码
2）、卸载 Release 模式包
    ./gradlew uninstallRelease
复制代码
3）、debug release 模式全部渠道打包
    ./gradlew assemble
复制代码
5、Gradle 查看包依赖命令
1）、查看项目根目录下的依赖
    ./gradlew dependencies
复制代码
2）、查看 app 模块下的依赖
    ./gradlew app:dependencies
复制代码
3）、查看 app 模块下包含 implementation 关键字的依赖项目
    ./gradlew app:dependencies --configuration implementation
复制代码
四、使用 Build Scan 诊断应用的构建过程
在了解 Build Scan 之前，我们需要先来一起学习下旧时代的 Gradle build 诊断工具 Profile report。
1、Profile report
通常情况下，我们一般会使用如下命令来生成一份本地的构建分析报告：
    ./gradlew assembleDebug --profile


https://juejin.cn/post/6844904132092903437



Gradle 常用命令
1、Gradle 查询命令
1）、查看主要任务
    ./gradlew tasks
复制代码
2）、查看所有任务，包括缓存任务等等
    ./gradlew tasks --all
复制代码
2、Gradle 执行命令
1）、对某个module [moduleName]   的某个任务[TaskName] 运行
    ./gradlew :moduleName:taskName
复制代码
3、Gradle 快速构建命令
Gradle 提供了一系列的快速构建命令来替代 IDE 的可视化构建操作，如我们最常用的 clean、build 等等。需要注意的是，build 命令会把 debug、release 环境的包都构建出来。
1）、查看构建版本
    ./gradlew -v
复制代码
2）、清除 build 文件夹
    ./gradlew clean
复制代码
3）、检查依赖并编译打包
    ./gradlew build
复制代码
4）、编译并安装 debug 包
    ./gradlew installDebug
复制代码
5）、编译并打印日志
    ./gradlew build --info
复制代码
6）、编译并输出性能报告，性能报告一般在构建工程根目录 build/reports/profile 下
    ./gradlew build --profile
复制代码
7）、调试模式构建并打印堆栈日志
    ./gradlew build --info --debug --stacktrace
复制代码
8）、强制更新最新依赖，清除构建后再构建
    ./gradlew clean build --refresh-dependencies
复制代码
9）、编译并打 Debug 包
    ./gradlew assembleDebug
    # 简化版命令，取各个单词的首字母
    ./gradlew aD
复制代码
10）、编译并打 Release 的包
    ./gradlew assembleRelease
    # 简化版命令，取各个单词的首字母
    ./gradlew aR
复制代码
4、Gradle 构建并安装命令
1）、Release 模式打包并安装
    ./gradlew installRelease
复制代码
2）、卸载 Release 模式包
    ./gradlew uninstallRelease
复制代码
3）、debug release 模式全部渠道打包
    ./gradlew assemble
复制代码
5、Gradle 查看包依赖命令
1）、查看项目根目录下的依赖
    ./gradlew dependencies
复制代码
2）、查看 app 模块下的依赖
    ./gradlew app:dependencies
复制代码
3）、查看 app 模块下包含 implementation 关键字的依赖项目
    ./gradlew app:dependencies --configuration implementation
复制代码
四、使用 Build Scan 诊断应用的构建过程
在了解 Build Scan 之前，我们需要先来一起学习下旧时代的 Gradle build 诊断工具 Profile report。
1、Profile report
通常情况下，我们一般会使用如下命令来生成一份本地的构建分析报告：
    ./gradlew assembleDebug --profile

