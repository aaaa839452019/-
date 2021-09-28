# -
文章
# -
文章
gradle
https://juejin.cn/post/6989877607126794247

https://juejin.cn/post/6947675376835362846


https://blog.csdn.net/qq_20798591/article/details/107061701?spm=1001.2014.3001.5502

https://blog.csdn.net/qq_20798591/article/details/103886452?spm=1001.2014.3001.5502


gradle  是构建工具，不是语言
它用了 Groovy 这个语言，Gradle 是一个开源的自动化构建工具，专注于灵活性和性能。Gradle 构建脚本是使用 Groovy 或 Kotlin DSL 编写的。
DSL英文全称：domain specific language，中文翻译即领域特定语言，例如：HTML，XML等 DSL 语言，DSL 的核心思想就是：“求专不求全，解决特定领域的问题

闭包
Java 的 Lambda 表达式:是单抽象方法(SAM)的接口的匿名类对象的快捷写 法，只是一个语法糖。
Kotlin 的 Lambda 表达式:和匿名函数相当，实质上是一个函数类型的对象， 并不只是语法糖。
Groovy 的 Lambda 表达式:Groovy 里不叫「Lambda」表达式，而是叫「闭 包」;在功能上，和 Kotlin 的 Lambda 比较相似，都是一个「可以传递的代码 块」，具体的功能比 Kotlin 的 Lambda 更强一些，但基本的概念是一样的。
为什么 Groovy 可以写出类似 JSON 格式的配 置?
因为它们其实都是方法调用，只是用闭包来写成了看起来像是 JSON 型的格式。

----------------------------
构建流程
1、初始化阶段




、Gradle 对象：在项目初始化时构建，全局单例存在，只有这一个对象
2、Project 对象：每一个 build.gradle 都会转换成一个 Project 对象
3、Settings 对象：Seetings.gradle 会转变成一个 Seetings 对象

在这个阶段中，会读取根工程中的 setting.gradle 中的 include 信息，确定有多少工程加入构建，然后，会为每一个项目（build.gradle 脚本文件）创建一个个与之对应的 Project 实例，最终形成一个项目的层次结构。
与初始化阶段相关的脚本文件是 settings.gradle，而一个 settings.gradle 脚本对应一个 Settings 对象，我们最常用来声明项目的层次结构的 include 就是 Settings 对象下的一个方法，在 Gradle 初始化的时候会构造一个 Settings 实例对象，以执行各个 Project 的初始化配置。




2、配置阶段

配置阶段的任务是 执行各项目下的 build.gradle 脚本，完成 Project 的配置，与此同时，会构造 Task 任务依赖关系图以便在执行阶段按照依赖关系执行  Task。而在配置阶段执行的代码通常来说都会包括以下三个部分的内容，如下所示：

1)、build.gralde 中的各种语句。
2)、闭包。
3)、Task 中的配置段语句。

需要注意的是，执行任何 Gradle 命令，在初始化阶段和配置阶段的代码都会被执行。



3、执行阶段
在配置阶段结束后，Gradle 会根据各个任务 Task 的依赖关系来创建一个有向无环图，我们可以通过 Gradle 对象的 getTaskGraph 方法来得到该有向无环图 => TaskExecutionGraph，并且，当有向无环图构建完成之后，所有 Task 执行之前，我们可以通过 whenReady(groovy.lang.Closure) 或者 addTaskExecutionGraphListener(TaskExecutionGraphListener) 来接收相应的通知，其代码如下所示：










Gradle 初始化阶段主要就是执行 settings.gradle 脚本，构建 Project 对象

我们使用 AndroidStudio 新建一个 Android 项目的时候会自动生成 settings.gradle 文件，内容如下：
rootProject.name = "GradleDemo"
include ':app'

-------------------------------------
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
    
    
    
    //初始化阶段开始时间
long beginOfSetting = System.currentTimeMillis()
//配置阶段开始时间
def beginOfConfig
//配置阶段是否开始了，只执行一次
def configHasBegin = false
//存放每个 build.gradle 执行之前的时间
def beginOfProjectConfig = new HashMap()
//执行阶段开始时间
def beginOfTaskExecute
//初始化阶段执行完毕
gradle.projectsLoaded {
    println "初始化总耗时 ${System.currentTimeMillis() - beginOfSetting} ms"
}

//build.gradle 执行前
gradle.beforeProject {Project project ->
    if(!configHasBegin){
        configHasBegin = true
        beginOfConfig = System.currentTimeMillis()
    }
    beginOfProjectConfig.put(project,System.currentTimeMillis())
}

//build.gradle 执行后
gradle.afterProject {Project project ->
    def begin = beginOfProjectConfig.get(project)
    println "配置阶段，$project 耗时：${System.currentTimeMillis() - begin} ms"
}

//配置阶段完毕
gradle.taskGraph.whenReady {
    println "配置阶段总耗时：${System.currentTimeMillis() - beginOfConfig} ms"
    beginOfTaskExecute = System.currentTimeMillis()
}

//执行阶段
gradle.taskGraph.beforeTask {Task task ->
    task.doFirst {
        task.ext.beginOfTask = System.currentTimeMillis()
    }

    task.doLast {
        println "执行阶段，$task 耗时：${System.currentTimeMillis() - task.ext.beginOfTask} ms"
    }
}

//执行阶段完毕
gradle.buildFinished {
    println "执行阶段总耗时：${System.currentTimeMillis() - beginOfTaskExecute}"
}



