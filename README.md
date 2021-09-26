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
