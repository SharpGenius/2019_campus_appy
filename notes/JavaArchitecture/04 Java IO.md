##### 1.Java序列化，如何实现序列化和反序列化，常见的序列化协议有哪些 

- Java序列化定义 

  - 将那些实现了Serializable接口的对象转换成一个字节序列，并能够在以后将这个字节序列完全恢复为原来的对象，序列化可以弥补不同[操作系统](http://lib.csdn.net/base/operatingsystem)之间的差异。 

- Java序列化的作用 

  - Java远程方法调用（RMI） 
  - 对JavaBeans进行序列化 

- 如何实现序列化和反序列化 

  - 实现序列化方法 
    - 实现Serializable接口 
      - 该接口只是一个可序列化的标志，并没有包含实际的属性和方法。 
      - 如果不在改方法中添加readObject()和writeObject()方法，则采取默认的序列化机制。如果添加了这两个方法之后还想利用Java默认的序列化机制，则在这两个方法中分别调用defaultReadObject()和defaultWriteObject()两个方法。 
      - 为了保证安全性，可以使用transient关键字进行修饰不必序列化的属性。因为在反序列化时，private修饰的属性也能发查看到。 
    - 实现ExternalSerializable方法 
      - 自己对要序列化的内容进行控制，控制那些属性能被序列化，那些不能被序列化。 
  - 反序列化 
    - 实现Serializable接口的对象在反序列化时不需要调用对象所在类的构造方法，完全基于字节。 
    - 实现externalSerializable接口的方法在反序列化时会调用构造方法。 
  - 注意事项 
    - 被static修饰的属性不会被序列化 
    - 对象的类名、属性都会被序列化，方法不会被序列化 
    - 要保证序列化对象所在类的属性也是可以被序列化的 
    - 当通过网络、文件进行序列化时，必须按照写入的顺序读取对象。 
    - 反序列化时必须有序列化对象时的class文件 
    - 最好显示的声明serializableID，因为在不同的JVM之间，默认生成serializableID 可能不同，会造成反序列化失败。 

- 常见的序列化协议有哪些 

  - COM主要用于Windows平台，并没有真正实现跨平台，另外COM的序列化的原理利用了编译器中虚表，使得其学习成本巨大。 

  - CORBA是早期比较好的实现了跨平台，跨语言的序列化协议。COBRA的主要问题是参与方过多带来的版本过多，版本之间兼容性较差，以及使用复杂晦涩。 

  - XML&SOAP 

    - XML是一种常用的序列化和反序列化协议，具有跨机器，跨语言等优点。 
    - SOAP（Simple Object Access protocol） 是一种被广泛应用的，基于XML为序列化和反序列化协议的结构化消息传递协议。SOAP具有安全、可扩展、跨语言、跨平台并支持多种传输层协议。 

  - JSON（[JavaScript](http://lib.csdn.net/base/javascript) Object Notation） 

    - 这种Associative array格式非常符合工程师对对象的理解。 
    - 它保持了XML的人眼可读（Human-readable）的优点。 
    - 相对于XML而言，序列化后的数据更加简洁。  
    - 它具备[javascript](http://lib.csdn.net/base/javascript)的先天性支持，所以被广泛应用于Web browser的应用常景中，是Ajax的事实标准协议。 
    - 与XML相比，其协议比较简单，解析速度比较快。 
    - 松散的Associative array使得其具有良好的可扩展性和兼容性。 

  - Thrift是Facebook开源提供的一个高性能，轻量级RPC服务框架，其产生正是为了满足当前[大数据](http://lib.csdn.net/base/hadoop)量、分布式、跨语言、跨平台数据通讯的需求。Thrift在空间开销和解析性能上有了比较大的提升，对于对性能要求比较高的分布式系统，它是一个优秀的RPC解决方案；但是由于Thrift的序列化被嵌入到Thrift框架里面，Thrift框架本身并没有透出序列化和反序列化接口，这导致其很难和其他传输层协议共同使用 

  - Protobuf具备了优秀的序列化协议的所需的众多典型特征 

    - 标准的IDL和IDL编译器，这使得其对工程师非常友好。 
    - 序列化数据非常简洁，紧凑，与XML相比，其序列化之后的数据量约为1/3到1/10。 
    - 解析速度非常快，比对应的XML快约20-100倍。 
    - 提供了非常友好的动态库，使用非常简介，反序列化只需要一行代码。由于其解析性能高，序列化后数据量相对少，非常适合应用层对象的持久化场景 

     

  - Avro的产生解决了JSON的冗长和没有IDL的问题，Avro属于Apache [Hadoop](http://lib.csdn.net/base/hadoop)的一个子项目。 Avro提供两种序列化格式：JSON格式或者Binary格式。Binary格式在空间开销和解析性能方面可以和Protobuf媲美，JSON格式方便[测试](http://lib.csdn.net/base/softwaretest)阶段的调试。适合于高性能的序列化服务。 

- 几种协议的对比 

  - XML序列化（Xstream）无论在性能和简洁性上比较差； 
  - Thrift与Protobuf相比在时空开销方面都有一定的劣势； 
  - Protobuf和Avro在两方面表现都非常优越。 

##### concurrent包下面，都用过什么？

- concurrent下面的包 

  - Executor  用来创建线程池，在实现Callable接口时，添加线程。 
  - FeatureTask 此 FutureTask 的 get 方法所返回的结果类型。 
  - TimeUnit 
  - Semaphore  
  - LinkedBlockingQueue  
- 所用过的类 

  - Executor   





1. ##### Java中的NIO，BIO，AIO分别是什么？

- 先来个例子理解一下概念，以银行取款为例：
  - 同步 ： 自己亲自出马持银行卡到银行取钱（使用同步IO时，Java自己处理IO读写）。
  - 异步 ： 委托一小弟拿银行卡到银行取钱，然后给你（使用异步IO时，Java将IO读写委托给OS处理，需要将数据缓冲区地址和大小传给OS(银行卡和密码)，OS需要支持异步IO操作API）。
  - 阻塞 ： ATM排队取款，你只能等待（使用阻塞IO时，Java调用会一直阻塞到读写完成才返回）。
  - 非阻塞 ： 柜台取款，取个号，然后坐在椅子上做其它事，等号广播会通知你办理，没到号你就不能去，你可以不断问大堂经理排到了没有，大堂经理如果说还没到你就不能去（使用非阻塞IO时，如果不能读写Java调用会马上返回，当IO事件分发器会通知可读写时再继续进行读写，不断循环直到读写完成）。



- BIO 

  - 同步并阻塞，服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，当然可以通过线程池机制改善。 
  - BIO方式适用于连接数目比较小且固定的[架构](http://lib.csdn.net/base/architecture)，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1.4以前的唯一选择，但程序直观简单易理解。 

- NIO  

  - 同步非阻塞，服务器实现模式为一个请求一个线程，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求时才启动一个线程进行处理。 
  - NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，并发局限于应用中，编程比较复杂，JDK1.4开始支持。 

- AIO 

  - 异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是由OS先完成了再通知服务器应用去启动线程进行处理. 
  - AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持。 

  

- 另外，I/O属于底层操作，需要操作系统支持，并发也需要操作系统的支持，所以性能方面不同操作系统差异会比较明显。 



BIO、NIO和AIO的区别（简明版） - Nutty - 博客园
https://www.cnblogs.com/ygj0930/p/6543960.html

Java BIO、NIO、AIO 学习-力量来源于赤诚的爱！-51CTO博客
http://blog.51cto.com/stevex/1284437



 