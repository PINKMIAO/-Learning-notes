# Spring 源码

容器

一开始说明Bean的对象创建采用的**反射**来生成的

xml 和 注解 都是用来生成对象的， 里面写的是 bean的描述信息 转换成 一个具体的对象，指向 IOC容器中的 BeadDefinition 接口

在他们之间还有一个读取的接口 BeanDefinitionReader 

这么一来就是 ：

- **xml**或**注解**或其他 ——> BeanDefinitionReader ——> BeanDefinition ——> BeanFactory —~反射~—> 实例化bean ——> 完整对象

如果还有新的要传输的类，那就创建一个子类实现这个Reader接口，统一出口放到Definition中去

**IOC容器中**

- 实例化：后👇

- 初始化：给属性赋值
  - 填充属性
  - 执行初始化方法
    - init-method

题外话：Spring生态，SpringBoot和SpringCloud都是在Spring的基础上扩展的，强调出了扩展性 （基石）



**源码执行步骤：**IOC

1. 创建BeanFactory才能把XML等配置文件放进BeanDefinition中

   先`prepareRefresh();`：刷新前准备工作

   - refreshBeanFactory中，执行hasBeanFactory判断，有-销毁destroyBeans，关闭closeBeanFactory
   - 后续在开始创建工厂、序列化指定id、设置相关属性，最后进行XML文件读取及解释
   - 其中读取完XML前后，可以看大beanDefinitionMap 和beanDefinnitionNames的大小都为配置文件中的bean个数（半成品bean，还没初始化）
   - 但是beanFactory中的大部分值都还是空的

2. `prepareBeanFactory(beanFactory);` 对beanFactory进行赋值操作

3. `invokeBeanFactoryPostProcessors(beanFactory);`调用各种bean的处理器

   `registerbeanPostProcessors(beanFactory);`		注册bean处理器，仅仅是注册

   `initMessageSource();`										  为上下文初始化message源，及不同的消息体，国际化

   `initApplicationEventMulticaster();`				  初始化事件监听多路广播器 （多播）

   `registerListeners();` 										 在所有注册的bean中查找listener bean，注册到消息广播器中。

   以上都是准备工作，虽然没有加prepare

4. `finishBeanFactoryInitialization(beanFactory);` 实例化剩下的单实例（非懒加载的）--正经实例化操作
   - 在进入方法后的最后一个调用`beanFactory.preInstantiateSingletons();`这里 实例化剩下的单例对象
     - 往下才开始真正实例
     - `getBean -> doGetBean(beanName);` 到这里面，才能看大有`createBean(beanName, mbd, args); ->  doCreateBean()`
  - 读取到已经实例化好的对象，放进List中，然后
   
5. 剩下的就跟背诵的步骤差不多，粗浅



**循环依赖问题：**两对象分别有互相的引用。A类中有B类的引用，B类有A类的引用。

- 实例化和初始化两者发生的问题（也可以顺带考察了bean对象的生命周期）

**一二三级缓存：**DefaultSingletonBeanRegistry 类中

- 一级缓存：用于保存BeanName和创建bean 实例之间的关系   	
- 二级缓存：用于保存BeanName和创建bean 实例之间的关系，与singletonFactories的不同之外在于，当一个单例bean被放到这里之后，那么当bean还在就可以通过getBean方法获取到，可以方便进行循环依赖的检测。
- 三级缓存：用于保存BeanName 和创建bean的工厂之间的关系（**解决循环依赖的问题**）


解决思想：实例化和初始化分开处理，提前暴露对象（已经完成了实例化但还没完成初始化的对象）

**1、三级缓存解决循环依赖问题的关键是什么？为什么通过提前暴露对象能解决？**

- 实例化和初始化分开操作，在中间过程中给其他对象赋值的时候，并不是一个完整对像，而是把半成品对象赋值给了其他对象。

**2、如果只使用一级缓存能不能解决问题？**

- 不能。在整个处理过程中，缓存中放的半成品和成品对象，如果只有一级缓存，那么成品和半成品都会放到一级缓存中，有可能在获取过程中获取到半成品，此时半成品对象是无法使用的，不能直接进行相关的处理，因此要把半成品和成品的存放空间分开来。

**3、只是用二级缓存行不行？为什么需要三级缓存？三级与二级相比，到底多了啥？**

**或能保证所有的bean对象都不去调用getEarlyBeanReference此方法，使用二级缓存可以吗？**

- 能保证不调用此方法，就可以只是用二级缓存

**使用三级缓存的本质在于使用AOP代理问题！！！**

**4、如果某个bean对象代理对象，那么会不会创建普通的bean对象？**

- 会

**5、为什么使用了三级缓存就能解决这问题？**

- 当一个对象需要被代理的时候，在整个创建过程中，是包含两个对象。一个是普通对象，一个是代理生成的对象，bean默认是单例的，那么我在整个生命周期的处理环节中，一个beanName能对应两个对象吗？不能，既然不能的话，保证我在使用的时候加一层的判断，判断是否需要进行代理的处理。

