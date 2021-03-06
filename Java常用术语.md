# Java常用术语解释

| **名词**            | **解释**                                   |
| ----------------- | ---------------------------------------- |
| **AAA**           | 认证(Authentication)：验证用户的身份与可使用的网络服务；授权(Authorization)：依据认证结果开放网络服务给用户；计帐(Accounting)：记 录用户对各种网络服务的用量，并提供给计费系统。简称AAA系统。 |
| **AWT**           | Abstract Window Toolkit(抽象窗口工具包)，第一代的 Java GUI工具包，现在基本已经不使用其中的组件，已经被Swing取代，但是Swing是扩展AWT而来。AWT中还包含很多现在GUI编程还在频繁使用的内容，例如事件处理及监听、布局管理器等。AWT也是JFC的一部分。 |
| **API**           | Application Programming Interface(应用编程接口)， 语言、框架以及类库对外提供的编码的接口。 |
| **AOP**           | Aspect Oriented Programming（面向切面编程），可以 通过预编译方式和运行期动态代理实现在不修改源代码的情况下给程序动态统一 添加功能的一种技术。 |
| **BMP**           | Bean-Managed Persistent（Bean管理的持久性），EJB中由 Bean自己负责持久性管理的方法，Bean的内容的同步（保存）需要自己编写代码 实现。 |
| **CALLBACK**      | CALLBACK首先是基于多线程的,没有线程的调用就不要谈回调.子类调用父类的构造方法叫回调用,那TMD的任何构造对象都叫回调了,因为任何对象至少继承了Object,构造时至少要调用Object的构造方法. |
| **CALLBACK机制**    | 一个主线程管理其它线程时,不用轮询方法检查各个线程的状态,而是在子线程中出现某种状态时通知主线程,啊,有人要按下我了,啊,我的值到达100了,(术语叫触发了某种事件)这样主线程收到这些消息再根据消息类型去调用相应的方法. 一个例子,我(主线程)坐车去北京,当车到北京时我要调用"下车"这个方法,如果不用回调用机制,我要不断地问driver,到了没有啊?如果我问超过三次而那个driver力气又比我大的话,他肯定要打我,如果用回调用机制,就是用一个子线程(可以让driver承担这个角色)在那运行,当到的时候通知我到了,我就调用"下车()",而不用过一会就问一次,这样我可以省下时间睡觉或和车上的美眉聊天. |
| **CMP**           | Container-Managed Persistent（容器管理的持久性），EJB 中由容器负责entity beans的持久性管理的方法，即容器负责将 entity beans的更新同步（保存）到数据库。 |
| **CORBA**         | Common Object Request Broker Architecture（公用对象请求代理[调度]程序体系结构），是一组用来定义"分布式对象系统"的标准， 由OMG(Object Menagement Group)作为发起和标准制定单位。CORBA的 目的是定义一套协议，符合这个协议的对象可以互相交互，不论它们是用什么样的语言写的，不论它们运行于什么样的机器和操作系统。 |
| **DTD**           | Document type Definition(文档类型定义)，它为一个 XML文档或者文档集合建立一套规则。它本身不是独立的技术规范，而是属于规范的一部分，XML文档中的文档类型声明既可以是标记约束，也可以是带有标记约束的外部文档。这两种约束的总和就是DTD。它规定了XML文档的构建方式。 |
| **DI**            | Dependency Injection（依赖注入），即组件之间的依赖关系 由容器在运行期决定，形象的来说，即由容器动态的将某种依赖关系注入到组件之中。依赖注入的目标并非为软件系统带来更多的功能，而是为了提升组件重用 的概率，并为系统搭建一个灵活、可扩展的平台。通过依赖注入机制，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务 逻辑，而不用关心具体的资源来自何处、由谁实现。（以上同样摘自夏昕的 Spring开发指南）。DI和IoC是同义词。 |
| **EJB**           | Enterprise JavaBeans，Java中用于开发企业级应用的技术标 准，他定义了一个用于开发和发布可重用的服务器端组件的模型，包括 Session beans，Entity beans以及Message-driven beans三种 。 |
| **Hibernate**     | Hibernate是一个开放源代码的O/R Mapping (对象关系 映射框架)，它对JDBC进行了轻量级的对象封装，使Java程序员可以随心所欲的使 用对象编程思维来操纵数据库。 |
| **IDL**           | Interface Definition Language（接口定义语言）， CORBA的一个关键特性，是一个语言中立的接口定义语言，每个支持CORBA的语言 都会有一个自己的IDL映射。 |
| **IIOP**          | Internet Inter-ORB Protocol(互联网内部对象请求代 理协议)，Java中使得程序可以和其他语言的CORBA实现实现互操作性的协议。 |
| **IoC**           | Inversion of Control（控制反转），由容器控制程序 之间的关系，而非传统实现中，由程序代码直接操控，控制权由应用代码中转到 了外部容器，控制权的转移，是所谓反转。（以上摘自夏昕的Spring开发指南） |
| **I18N**          | internationalization（国际化），这个单词的长度是20，然后取 其首尾字母，中间省略的字母刚好18个。 |
| **JCA**           | Java Cryptography Architecture，Java加密架构， java平台中用于访问和开发加密功能的框架。 |
| **JTS**           | Java Transaction Service（Java事务服务），Java中 进行分布式事务管理的技术标准，它是基于CORBA对象事务服务（CORBA Object Transaction Service）的。 使得EJB和它的客户端能够进行事务操作；可以对应用程序中的若干个Bean进行更新，并保证所有的更改在事务的最后能够提交或者回滚；依赖JDBC-2驱动程序来支持XA协 议进而支持通过一个或多个资源管理者执行分布式事务处理的能力 |
| **JNDI**          | Java Naming and Directory Interface （Java命名和目录服务接口），Java中使用目录和命名服务的技术规范，和JDBC 类似，他由API和SPI构成。J2EE的目录服务使得Java客户端和Web层 Servlet 能够查询用户定义的对象，比如说，EJB和环境配置项（比如JDBC 驱动程序的地址） |
| **JMS**           | Java Messaging Service（Java消息服务），使用基于 点到点（一对一）或者发布订阅（多对多）的交互方式来支持J2EE应用程序之间 的异步通讯；所有消息可被设定为具有与其关联的服务的特性，从最佳效果服务 特性到事务性服务特性 |
| **JCP**           | Java Community Process（Java社区过程），负责Java 技术发展与审核技术规格的开放组织，JCP对提出的请求投票表决，JCP的专家组 成员一般都是业界比较有影响力的企业或者组织。 |
| **JNode**         | JNode 是个特殊的 JVM，可以在没有其他 OS 的?机上运行 Java 程序。可惜刚刚成形，不能实 用。 相关网站：[http://jnode.sourcefor ge.net/portal/ ](http://jnode.sourceforge.net/portal/); |
| **JTA**           | Java Transaction API（Java事务API），Java中进行事 务划分的技术。 |
| **JSF**           | Java Server Faces，新一代的Java Web应用技术 标准，吸收了很多Servlet、JSP以及其他的Web应用框架的特性。JSF为Web应用开 发定义了一个事件驱动的、基于组件的模型。 |
| **JNI**           | java本地编程接口。是 Java Native Interface 的英文缩写。他能够使java 代码与用其他编程语言编写的应用程序和库进行互操作。（其他编程语言大多是 c,c++和汇编语言。） |
| **JDBC**          | Java DataBase Connectivity（Java数据库连接），用 于访问关系型数据库的Java技术，仅仅是一种技术标准，访问不同的关系型数据 库需要相应的JDBC规范的实现包。 |
| **JSP**           | Java Server Pages（Java服务器端页面），J2EE标准中 用于创建动态页面内容的技术标准，基于Servlet技术，需要支持该标准的服务器 才能运行，最常用的JSP服务器之一就是Tomcat。 |
| **JFC**           | Java Foundation Classes（JAVA基础类），集合了GUI 组件以及其他能简化开发和展开桌面和Internet/Intranet应用的服务，其核心就 是Swing。 |
| **JVM**           | Java Virtual Machine（Java虚拟机），它是一个虚构 出来的计算机,是通过在实际的计算机上仿真模拟各种计算机功能来实现的，。 Java虚拟机有自己完善的硬件架构,如处理器、堆栈、寄存器等,还具有相应的指令系统。JVM屏蔽了与具体操作系统平台相关的信息,使得Java程序只需生成在 Java虚拟机上运行的目标代码(字节码),就可以在多种平台上不加修改地运行。 Java虚拟机在执行字节码时,实际上最终还是把字节码解释成具体平台上的机器指 令执行。 |
| **JRE**           | Java Runtime Environment（Java运行环境），运行 JAVA程序所必须的环境的集合，包含JVM标准实现及Java核心类库。 |
| **JSDK**          | Java Software Development Kit，和JDK以及J2SE 等同。 |
| **JDK**           | Java Development Kit(Java开发工具包):包括运行环境 、编译工具及其它工具、源代码等，基本上和J2SE等同。 |
| **J2ME**          | Java 2 Micro Edition（JAVA2精简版）API规格基 于J2SE ，但是被修改为可以适合某种产品的单一要求。J2ME使JAVA程序可以很方便的应用于电话卡、寻呼机等小型设备，它包括两种类型的组件，即配置 （configuration）和描述（profile）。 |
| **J2EE**          | Java 2 Enterprise Edition（JAVA2企业版），使用Java进行企业开发的一套扩展标准，必须基于J2SE，提供一个基于组件设计、 开发、集合、展开企业应用的途径。J2EE 平台提供了多层、分布式的应用 模型，重新利用组件的能力，统一安全的模式以及灵活的处理控制能力。J2EE包 括 EJB, JTA, JDBC, JCA, JMX, JNDI, JMS, ;JavaMail, Servlet, JSP等规范。 |
| **J2SE**          | Java 2 Standard Edition（JAVA2标准版），用来 开发Java程序的基础，包括编译器、小工具、运行环境，SUN发布的标准版本中还 包括核心类库的所有源代码。 |
| **L10N**          | localization（本地化），和I18N类似，取首尾字母，中间省略10 个字母。 |
| **MVC**           | Model View Controller的缩写，为了获得更好的系统结 构而推出的一种宏观的设计模式，model代表系统的模型层，view是模型的展现层 ，controller负责业务的流转，使用MVC可以使得系统的层次清晰，降低各个部分 的耦合。 |
| **PI**            | Processing Instruction(处理指令)，XML中指示应用程序执 行一些特定的任务。其格式是 <!---->，它只 能是解析器可以识别的XML标准处理指令集中一部分。有时它也被应用程序用来传 达信息，这些信息可用来帮助进行解析，在这种情况下，应用程序中要有可以作 为处理指令执行对象的关键字。 |
| **PO**            | persisent object 持久对象                    |
| **POJO**          | pure old java object or plain ordinary java object or what ever. (英文太烂,没看懂这句话的意思,有知道的人请赐教!) |
| **RADIUS**        | Remote Authentication Dial In User Service广泛应用于宽带窄带认证系统的协议，前端一般为PPPoE或者802.1x。 |
| **RMI**           | Remote Method Invocation(远程方法调用)，Java中进行分布式编程的基础技术，EJB技术也是基于RMI的。 RMI让你能够通过自己机子上的对象运用方式，使用其它机子上的对象。 |
| **RTTI**          | run-time type identification,执行期类型识别。当你有一个指向基类的reference时，RTTI机制让你得以找出它所指向的对象以及类的相关信息。（JAVA提供的另一个方法就是reflection[反射/映射]机制） |
| **SERIALIZATION** | 序列化。是一切对象深度CLONE,对象的存储与恢复,对象的远程调用的基础,也就是说它是对象池化管理,分布式引用的基础,想想J2EE平台如果不靠它能做什么? 这个机制让我们得以实现轻量级持久机制 |
| **SWT**           | SWT 本身仅仅是Eclipse组织为了开发 Eclipse IDE环境所编写的一组底层图形界面 API。至今为止，SWT无论是在性能和外观上，都超越了SUN公司提供的AWT和SWING。目前 Eclipse IDE已经开发到了2.1版本，SWT已经十分稳定 [http://www.javaresearch.org/article/showarticl e.jsp?column=287&thread=24407](http://www.javaresearch.org/article/showarticle.jsp?column=287&thread=24407) |
| **SOA**           | Service-Oriented Architecture，面向服务架构，SOA是一种 架构模型，它可以根据需求通过网络对松散耦合的粗粒度应用组件进行分布式部署、组合和使用。服务层是SOA的基础，可以直接被应用调用，从而有效控制系统中与软件代理交互的人为依赖性。SOA的几个关键特性：一种粗粒度、松耦合服务架构，服务之间通过简单、精确定义接口进行通讯，不涉及底层编程接口和通讯 模型。 |
| **SPI**           | Service Provider Interface（服务提供商接口），满 足某种服务标准的供应商提供的符合该标准的应用程序接口，SPI应该和该服务的 API标准是兼容的，应用程序一般应该是基于API编写，除非是SPI中包含API中没 有提供的功能而又必须使用。 |
| **SableVM**       | SableVM是用C语言写的非常简便的JAVA 虚拟机网站详细地址：[http://sablevm.org/](http://sablevm.org/) |
| **WFC**           | Windows Foundation Classes for Ja va 的英文缩写，他提供了Java 软件包的架构，他支持面向 Windows 操作系统和Dynamic HTML对象模型的组件。 |
| **WORA**          | Write Once, Run Anywhere（一次编写，到处运行 ），Java的宣传口号，在一定程度上可以达到，对于复杂应用在不同平台上可能 需要进行调试。 |
| **XML**           | Extentsible Markup Language（可扩展标记语言）的缩 写，是用来定义其它语言的一种元语言，其前身是SGML(标准通用标记语言)。它没有标签集（tag set），也没有语法规则（grammatical rule），但 是它有句法规则（syntax rule）。任何XML文档对任何类型的应用以及正确 的解析都必须是良构的（well-formed），即每一个打开的标签都必须有匹配的结束标签，不得含有次序颠倒的标签，并且在语句构成上应符合技术规范的要求。 XML文档可以是有效的（valid），但并非一定要求有效。所谓有效文档是指其符合其文档类型定义（DTD）的文档。如果一个文档符合一个模式（schema）的规定 ，那么这个文档是"模式有效的（schema valid）"。 |
| **XSL**           | Extensible Stylesheet Language(可扩展样式表语言)，它能够改变及转换一种XML格式的数据为另一种XML格式。它提供一个已定义好的样式表，通过这个结构可以完成不同格式的转换。为了避免因为一个不同的表示方式就要不得不改动数据，XSL使数据或文档内容与表示形式相透明。它所采用的方法可以与用java写一个将数据转换成其它格式的小程序相提并论，而且还提供一个标准接口。 |
| **单元测试**          | 单元测试测的是独立的一个工作单元。在Java应用程序中，"独立的一个工作单元"常常指的是一个方法（但并不总是如此）。作为对比，集成测试和接收测试则检查多个组件如何交互。一个工作单元是一项任务，它不依赖于其他任何任务的完成。(摘自《JUnit in action中文版》) |
| **反射(也可以叫映射)**    | 是RUNTIME 的事,完成类的加载,类的分析,bean的自省等功能,JBUILDER为什么敲一个类的名称后面就列出了它的成员变量和方法供你选择?如果你想知道一个对象的某种方法被调用过多少次你用什么方法?这些知识可以让你从一行Exception就能分析出错误原因.以及自己实现classloader,安全管理等方面的工作. |
| **框架**            | 框架是一个应用程序的半成品。框架提供了可在应用程序之间共享的可复用的公共结构。开发者把框架融入他们自己的应用程序，并加以扩展，以满足他们特定的需要。框架和工具包的不同之处在于，框架提供了一致的结构，而不仅仅是一组工具类。(摘自《JUnit in action中文版》) |
| **持久性**           | 指的是某个对象的生命周期不取决于程序的执行与否。                 |
| **名字空间**          | namespace 就是一个元素前缀与URI（统一资源标识符）之间的一种映射关系，这个映射可以用来处理名字空间冲突，定义可以允许解析器处理冲突的数据结构。XML名字空间推荐标准定义了规范这些名字的机制，这种机制依靠URI来完成任务，详细情况后面有叙述。名字空间是用一个XML元素加一个前缀组成的，比如 <html:table> </html:table> 和 <form:table> ， </form:table> 这样XML解析器就可以在不使用完全不同的元素名字的情况下区分上述两个元素的名字。它经常在XML文档中使用，也可以在模式以及XSL样式表或者xml有关的规范中使用。 |
| **开放封闭法则**        | 软件实体应该是可扩展的，但是不可修改的(Software Entities Should Be Open For Extension, Yet Closed For Modification)，简称OCP，这个法则是OO中最重要的一条法则，其含义是我们应该能够不用修改软件实体的源代码，就能更改软件实体的行为，符合该法则便意味着最高等级的复用性（reusability）和可维护性（maintainability）。 |