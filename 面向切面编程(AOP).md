# AOP面向切面编程

## AOP和OOP的关系？

大家都知道OOP，即ObjectOriented Programming，面向对象编程。
AOP是Aspect Oriented Programming的缩写，中译文为面向切向编程。OOP和AOP是什么关系呢？首先：

- OOP和AOP都是方法论。
- 在OOP的世界中，问题或者功能都被划分到一个一个的模块里边。每个模块专心干自己的事情，模块之间通过设计好的接口交互。

还记得Android 刚刚学习的时候，我们看过的Android的Framework的架构图吗？
![Android的Framework的架构图](https://ws1.sinaimg.cn/large/006tNbRwly1fwka7ydesij31kw0rk7f5.jpg)

​	OOP的精髓是把功能或问题模块化，每个模块处理自己的家务事。但在现实世界中，并不是所有问题都能完美得划分到模块中。举个最简单而又常见的例子：现在想为每个模块加上日志功能，要求模块运行时候能输出日志。在不知道AOP的情况下，一般的处理都是：先设计一个日志输出模块，这个模块提供日志输出API，比如Android中的Log类。然后，其他模块需要输出日志的时候调用Log类的几个函数，比如e(TAG,...)，w(TAG,...)，d(TAG,...)，i(TAG,...)等。

​	但是在实际的生活中或者是开发项目里，日志作为基础的功能组件，是每一个业务组件都需要依赖的。登录需要记录日志信息、订单需要记录日志信息，那么在每一个业务或者是功能组件中都嵌入日志的代码是违背设计模式的。如果将来日志模块需要重构，难道我们是要到处去修改吗？你会发现，日志作为基础模块我们需要把他作为一个切面进行代码的管理，这样就可以把日志功能作为一个横切面切入到整个业务代码中。

> 题外话：AOP做为*方法论*，它的学习和体会是需要一步一步，并且一定要结合实际来的。我们在学习的过程中切记贪多贪快，学习是个循序渐进的过程，在学习设计模式和方法论的时候一定要多结合实际。

实际开发的应用场景：

- 用户权限检查、APP的全局登录验证、APP系统权限判断等等
- 全局的日志打印、重要函数的执行时间统计、埋点设计等等



## AspectJ

AspectJ支持编译期和加载时代码注入，在开始之前，我们先熟悉一下AOP中的相关术语：
**Advice（通知）:** 典型的 Advice 类型有 before、after 和 around，分别表示在目标方法执行之前、执行后和完全替代目标方法执行的代码。

**Joint point（连接点）:** 程序中可能作为代码注入目标的特定的点和入口。

**Pointcut（切入点）:** 告诉代码注入工具，在何处注入一段特定代码的表达式。

**Aspect（切面）:** Pointcut 和 Advice 的组合看做切面。例如，我们在应用中通过定义一个 pointcut 和给定恰当的advice，添加一个日志切面。

**Weaving（织入）:** 注入代码（advices）到目标位置（joint points）的过程。

![image-20181025111621994](https://ws4.sinaimg.cn/large/006tNbRwly1fwkat19346j31120muq66.jpg)

AndroidStudio引入AspectJ：

参考资料：[Gradle引入aspectj](https://plugins.gradle.org/plugin/aspectj.gradle)
        [aspect-oriented-programming-in-android](https://fernandocejas.com/2014/08/03/aspect-oriented-programming-in-android)

根据 Gradle 官方给出来的 AspectJ 配置，需要在 `build.config` 添加如下代码：

```groovy
buildscript {
  repositories {
    maven {
      url "https://plugins.gradle.org/m2/"
    }
  }
  dependencies {
    classpath "gradle.plugin.aspectj:gradle-aspectj:0.1.6"
  }
}

apply plugin: "aspectj.gradle"
```

然而直接执行 `gradle build` 会出现以下错误提示：

> You must set the property ‘aspectjVersion’ before applying the aspectj plugin

按照提示为环境添加以下配置（需注意下面代码要放置在 `apply plugin: "aspectj.gradle"` 之前）：

```
project.ext {
    aspectjVersion = '1.8.10'
    ...
}
```

再执行 `gradle build`，正常运行。



