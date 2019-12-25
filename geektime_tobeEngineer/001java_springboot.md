

	需求变化快和用户群体庞大，在这种情况下，如何从系统架构的角度出发，构建灵活、易扩展的系统，快速应对需求的变化；同时，随着用户的增加，如何保证系统的可伸缩性、高可用性，成为系统架构面临的挑战。分而治之的思想被提了出来，于是我们从单独架构发展到分布式架构，又从分布式架构发展到 SOA 架构，服务不断的被拆分和分解，粒度也越来越小，直到微服务架构的诞生。
	
	微服务架构是 SOA 架构的传承，但一个最本质的区别就在于微服务是真正的分布式的、去中心化的。把所有的“思考”逻辑包括路由、消息解析等放在服务内部，去掉一个大一统的 ESB，服务间轻通信，是比 SOA 更彻底的拆分。微服务架构强调的重点是业务系统需要彻底的组件化和服务化，原有的单个业务系统会拆分为多个可以独立开发，设计，运行和运维的小应用，这些小应用之间通过服务完成交互和集成。
	
	每个服务运行在其独立的进程中，服务和服务间采用轻量级的通信机制互相沟通（通常是基于 HTTP 的 RESTful API）。每个服务都围绕着具体业务进行构建，并且能够被独立地部署到生产环境、类生产环境等。另外，应尽量避免统一的、集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建。

**数据处理** 

在微服务架构中我们强调彻底的组件化和服务化，每个微服务都可以独立的部署和投产，其实也就意味着很多的微服务有自己独立的数据库。

	整个业务数据被分散在各个子服务之后会带来两个最明显的问题：1、业务管理系统对数据完整的查询，比如分页查询、多条件查询等，数据被割裂后如何来整合？2、如何对数据进一步的分析挖掘？这些需求可能需要分析全量的数据，并且在分析时不能影响到当前业务。
	
	从技术方案来讲，我们一般有两种选择来处理这些问题，第一种是在线处理数据，第二种是离线处理数据。

在线这种方案有两个弊端：1）一方面微服务数据方需要提供数据接口，一方面数据的使用者需要去写调用方法，并且调用者需要编写大量的代码进行数据处理；2）在对各个微服务进行调取数据时会影响微服务的正常业务处理性能。

离线处理数据方案，就是将业务数据准实时的同步到另外一个数据库中，在同步的过程中进行数据整合处理，以满足业务方对数据的需求，数据同步过来后，再提供另外一个服务接口专业负责对外输出数据信息。这种方案有两个特点：1）数据同步方案是关键，技术选型有很多，如何选择切合公司业务的技术方案；2）离线数据处理对微服务正常业务处理没有影响。

第二种方案

MongDB 和数据分析

MongoDB 称之为对开发人员最友好的数据库，不再强调传统关系数据库中的行和列，整个表可以看作一个 Json 文档，MongoDB 也被认为在 Nosql 中最像关系数据库的 Nosql 数据库，保留了类似关系数据库的数据库（DataBase）、集合（Collection）、文档对象（Document）。

MongoDB 最大的特点是支持的查询语言非常强大，其语法有点类似于面向对象的查询语言，几乎可以实现类似关系数据库单表查询的绝大部分功能，而且还支持对数据建立索引。MongoDB 在高可用和读写负载均衡上的实现非常简洁和友好，MongoDB 自带了副本集的概念，通过设计恰当的副本集和驱动程序，可以非常便地实现高可用、读写负载均衡。

MongoDB 的这些特性非常方便对数据进行高性能查询，MongoDB 支持 Aggregate 和 Mapreduce 利用分而治之的理念来处理大规模数据分析。Spring Boot 对 MongoDB 的支持非常友好，使用 Spring Boot 非常便利的处理对 MongoDB 查询和操作，Spring Boot 也提供了组件包来支持对 MongoDB的使用。

MongoDB 4.0 宣布将正式支持 ACID 事务，未来 MongoDB 的想象空间更加巨大！因此 MongDB + Spring Boot 是微服务架构中数据分析的理想选择之一。

# springboot

## Spring Boot 特性

使用 Spring 项目引导页面可以在几秒构建一个项目

方便对外输出各种形式的服务，如 REST API、WebSocket、Web、Streaming、Tasks

非常简洁的安全策略集成

支持关系数据库和非关系数据库

支持运行期内嵌容器，如 Tomcat、Jetty

强大的开发包，支持热启动

自动管理依赖

自带应用监控

支持各种 IED，如 IntelliJ IDEA 、NetBeans

### 优点

总结一下，使用 Spring Boot 至少可以给我们带来以下几方面的改进：

Spring Boot 使编码变简单，Spring Boot 提供了丰富的解决方案，快速集成各种解决方案提升开发效率。

Spring Boot 使配置变简单，Spring Boot 提供了丰富的 Starters，集成主流开源产品往往只需要简单的配置即可。

Spring Boot 使部署变简单，Spring Boot 本身内嵌启动容器，仅仅需要一个命令即可启动项目，结合 Jenkins 、Docker 自动化运维非常容易实现。

Spring Boot 使监控变简单，Spring Boot 自带监控组件，使用 Actuator 轻松监控服务各项状态。

总结，Spring Boot 是 Java 领域最优秀的微服务架构落地技术，没有之一。

	微服务架构下，数据被分隔到 N 个独立的微服务中，如何应对市场、业务对大量数据的查询、分析就变的非常急迫，利用 Spring Boot 和 MongoDB 可以轻松的解决这个问题，通过技术手段将分裂到 N 个微服务的数据同步到 MongoDB 集群中，在同步的过程中进行数据清洗，来满足公司的各项业务需求。Spring Boot 对 MongoDB 的支持非常友好，一方面 Spring Data 技术预生成很多常用方法便于使用，另一方面 Spring Boot 封装了分布式计算的相关函数，可以让我们以较简洁的方式来实现统计查询。

Spring Boot 是 Java 领域微服务架构最优落地技术，Spring Boot+MongoDB 方案是在微服务架构下数据治理的最佳方案之一。



## springboot开源学习



### pom文件解析

#### 什么是pom

pom代表项目对象模型，它是Maven中工作的基本组成单位。它是一个XML文件，始终保存在项目的基本目录中的pom.xml文件中。pom包含的对象是使用maven来构建的，pom.xml文件包含了项目的各种配置信息。 创建一个POM之前，应该要先决定项目组(groupId)，项目名(artifactId)和版本（version），因为这些属性在项目仓库是唯一标识的。需要特别注意，每个项目都只有一个pom.xml文件。

### pom节点分布

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
            http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- 基本配置 -->
    <groupId>...</groupId>
    <artifactId>...</artifactId>
    <version>...</version>
    <packaging>...</packaging>

    <!-- 依赖配置 -->
    <dependencies>...</dependencies>
    <parent>...</parent>
    <dependencyManagement>...</dependencyManagement>
    <modules>...</modules>
    <properties>...</properties>

    <!-- 构建配置 -->
    <build>...</build>
    <reporting>...</reporting>

    <!-- 项目信息 -->
    <name>...</name>
    <description>...</description>
    <url>...</url>
    <inceptionYear>...</inceptionYear>
    <licenses>...</licenses>
    <organization>...</organization>
    <developers>...</developers>
    <contributors>...</contributors>

    <!-- 环境设置 -->
    <issueManagement>...</issueManagement>
    <ciManagement>...</ciManagement>
    <mailingLists>...</mailingLists>
    <scm>...</scm>
    <prerequisites>...</prerequisites>
    <repositories>...</repositories>
    <pluginRepositories>...</pluginRepositories>
    <distributionManagement>...</distributionManagement>
    <profiles>...</profiles>
</project>
```

### 各节点说明

#### 1.基本配置信息

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
   http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <!-- pom模型版本，maven2和3只能为4.0.0-->
   <modelVersion>4.0.0</modelVersion>
   <!-- 项目的组ID，用于maven定位-->
   <groupId>com.company.bank</groupId>
   <!-- 项目ID，通常是项目的名称,唯一标识符-->
   <artifactId>parent</artifactId>
   <!-- 项目的版本-->
   <version>1.0</version>
   <!-- 项目的打包方式-->
   <packaging>war</packaging>
<project>
```

| 节点         | 解释说明                                                     |
| ------------ | ------------------------------------------------------------ |
| modelVersion | pom模型版本，maven2和3只能为4.0.0                            |
| groupId      | 这是项目组的编号，这在组织或项目中通常是独一无二的。 例如，一家银行集团com.company.bank拥有所有银行相关项目。 |
| artifactId   | 这是项目的ID。这通常是项目的名称。 例如，consumer-banking。 除了groupId之外，artifactId还定义了artifact在存储库中的位置。 |
| version      | 这是项目的版本。与groupId一起使用，artifact在存储库中用于将版本彼此分离。 例如：com.company.bank:consumer-banking:1.0，com.company.bank:consumer-banking:1.1 |
| packaging    | 项目打包方式，有以下值：pom, jar, maven-plugin, ejb, war, ear, rar, par |

#### 2.依赖配置

##### (1) dependencies

项目相关依赖配置，如果在父项目写的依赖，会被子项目引用。一般会在父项目中定义子项目中所有共用的依赖。

```xml
<dependencies>
    <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
    </dependency>
</dependencies>
```

##### （2）parent

用于确定父项目的坐标位置。

```xml
<parent>
    <groupId>com.learnPro</groupId>
    <artifactId>SIP-parent</artifactId>
    <relativePath></relativePath>
    <version>0.0.1-SNAPSHOT</version>
</parent>
```

groupId: 父项目的组Id标识符

artifactId:父项目的唯一标识符

relativePath：Maven首先在当前项目中找父项目的pom，然后在文件系统的这个位置（relativePath），然后在本地仓库，再在远程仓库找。

version: 父项目的版本

##### (3) modules

有些maven项目会做成多模块的，这个标签用于指定当前项目所包含的所有模块。之后对这个项目进行的maven操作，会让所有子模块也进行相同操作。

```xml
<modules>
   <module>com-a</>
   <module>com-b</>
   <module>com-c</>
<modules/>
```

##### (4) properties

用于定义pom常量

```xml
<properties>
    <java.version>1.7</java.version>
</properties>
```

上面这个常量可以在pom文件的任意地方通过${[Java](https://link.jianshu.com/?t=http://lib.csdn.net/base/java).version}来引用

##### 5）dependencyManagement

配置写法同dependencies

```xml
<dependencyManagement>
    <dependencies>
    .....
    </dependencies>
</dependencyManagement>
```

在父模块中定义后，子模块不会直接使用对应依赖，但是在使用相同依赖的时候可以不加版本号,这样的好处是，父项目统一了版本，而且子项目可以在需要的时候才引用对应的依赖。

```xml
父项目：
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

子项目：

<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
</dependency>
```

#### 3.构建配置

```xml
<build>    
    <!--该元素设置了项目源码目录，当构建项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。-->    
    <sourceDirectory/>    
    <!--该元素设置了项目脚本源码目录，该目录和源码目录不同：绝大多数情况下，该目录下的内容 会被拷贝到输出目录(因为脚本是被解释的，而不是被编译的)。-->    
  <scriptSourceDirectory/>    
  <!--该元素设置了项目单元测试使用的源码目录，当测试项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。-->    
  <testSourceDirectory/>    
  <!--被编译过的应用程序class文件存放的目录。-->    
  <outputDirectory/>    
  <!--被编译过的测试class文件存放的目录。-->    
  <testOutputDirectory/>    
  <!--使用来自该项目的一系列构建扩展-->    
  <extensions>    
   <!--描述使用到的构建扩展。-->    
   <extension>    
    <!--构建扩展的groupId-->    
    <groupId/>    
    <!--构建扩展的artifactId-->    
    <artifactId/>    
    <!--构建扩展的版本-->    
    <version/>    
   </extension>    
  </extensions>    
  <!--当项目没有规定目标（Maven2 叫做阶段）时的默认值-->    
  <defaultGoal/>    
  <!--这个元素描述了项目相关的所有资源路径列表，例如和项目相关的属性文件，这些资源被包含在最终的打包文件里。-->    
  <resources>    
   <!--这个元素描述了项目相关或测试相关的所有资源路径-->    
   <resource>    
    <!-- 描述了资源的目标路径。该路径相对target/classes目录（例如${project.build.outputDirectory}）。举个例 子，如果你想资源在特定的包里(org.apache.maven.messages)，你就必须该元素设置为org/apache/maven /messages。然而，如果你只是想把资源放到源码目录结构里，就不需要该配置。-->    
    <targetPath/>    
    <!--是否使用参数值代替参数名。参数值取自properties元素或者文件里配置的属性，文件在filters元素里列出。-->    
    <filtering/>    
    <!--描述存放资源的目录，该路径相对POM路径-->    
    <directory/>    
    <!--包含的模式列表，例如**/*.xml.-->    
    <includes/>    
    <!--排除的模式列表，例如**/*.xml-->    
    <excludes/>    
   </resource>    
  </resources>    
  <!--这个元素描述了单元测试相关的所有资源路径，例如和单元测试相关的属性文件。-->    
  <testResources>    
   <!--这个元素描述了测试相关的所有资源路径，参见build/resources/resource元素的说明-->    
   <testResource>    
    <targetPath/><filtering/><directory/><includes/><excludes/>    
   </testResource>    
  </testResources>    
  <!--构建产生的所有文件存放的目录-->    
  <directory/>    
  <!--产生的构件的文件名，默认值是${artifactId}-${version}。-->    
  <finalName/>    
  <!--当filtering开关打开时，使用到的过滤器属性文件列表-->    
  <filters/>    
  <!--子项目可以引用的默认插件信息。该插件配置项直到被引用时才会被解析或绑定到生命周期。给定插件的任何本地配置都会覆盖这里的配置-->    
  <pluginManagement>    
   <!--使用的插件列表 。-->    
   <plugins>    
    <!--plugin元素包含描述插件所需要的信息。-->    
    <plugin>    
     <!--插件在仓库里的group ID-->    
     <groupId/>    
     <!--插件在仓库里的artifact ID-->    
     <artifactId/>    
     <!--被使用的插件的版本（或版本范围）-->    
     <version/>    
     <!--是否从该插件下载Maven扩展（例如打包和类型处理器），由于性能原因，只有在真需要下载时，该元素才被设置成enabled。-->    
     <extensions/>    
     <!--在构建生命周期中执行一组目标的配置。每个目标可能有不同的配置。-->    
     <executions>    
      <!--execution元素包含了插件执行需要的信息-->    
      <execution>    
       <!--执行目标的标识符，用于标识构建过程中的目标，或者匹配继承过程中需要合并的执行目标-->    
       <id/>    
       <!--绑定了目标的构建生命周期阶段，如果省略，目标会被绑定到源数据里配置的默认阶段-->    
       <phase/>    
       <!--配置的执行目标-->    
       <goals/>    
       <!--配置是否被传播到子POM-->    
       <inherited/>    
       <!--作为DOM对象的配置-->    
       <configuration/>    
      </execution>    
     </executions>    
     <!--项目引入插件所需要的额外依赖-->    
     <dependencies>    
      <!--参见dependencies/dependency元素-->    
      <dependency>    
       ......    
      </dependency>    
     </dependencies>         
     <!--任何配置是否被传播到子项目-->    
     <inherited/>    
     <!--作为DOM对象的配置-->    
     <configuration/>    
    </plugin>    
   </plugins>    
  </pluginManagement>    
  <!--使用的插件列表-->    
  <plugins>    
   <!--参见build/pluginManagement/plugins/plugin元素-->    
   <plugin>    
    <groupId/><artifactId/><version/><extensions/>    
    <executions>    
     <execution>    
      <id/><phase/><goals/><inherited/><configuration/>    
     </execution>    
    </executions>    
    <dependencies>    
     <!--参见dependencies/dependency元素-->    
     <dependency>    
      ......    
     </dependency>    
    </dependencies>    
    <goals/><inherited/><configuration/>    
   </plugin>    
  </plugins>    
 </build>
```

#### 4.reporting

该元素描述使用报表插件产生报表的规范。当用户执行“mvn site”，这些报表就会运行。 在页面导航栏能看到所有报表的链接。

```xml
<reporting>    
  <!--true，则，网站不包括默认的报表。这包括“项目信息”菜单中的报表。-->    
  <excludeDefaults/>    
  <!--所有产生的报表存放到哪里。默认值是${project.build.directory}/site。-->    
  <outputDirectory/>    
  <!--使用的报表插件和他们的配置。-->    
  <plugins>    
   <!--plugin元素包含描述报表插件需要的信息-->    
   <plugin>    
    <!--报表插件在仓库里的group ID-->    
    <groupId/>    
    <!--报表插件在仓库里的artifact ID-->    
    <artifactId/>    
    <!--被使用的报表插件的版本（或版本范围）-->    
    <version/>    
    <!--任何配置是否被传播到子项目-->    
    <inherited/>    
    <!--报表插件的配置-->    
    <configuration/>    
    <!--一组报表的多重规范，每个规范可能有不同的配置。一个规范（报表集）对应一个执行目标 。例如，有1，2，3，4，5，6，7，8，9个报表。1，2，5构成A报表集，对应一个执行目标。2，5，8构成B报表集，对应另一个执行目标-->    
    <reportSets>    
     <!--表示报表的一个集合，以及产生该集合的配置-->    
     <reportSet>    
      <!--报表集合的唯一标识符，POM继承时用到-->    
      <id/>    
      <!--产生报表集合时，被使用的报表的配置-->    
      <configuration/>    
      <!--配置是否被继承到子POMs-->    
      <inherited/>    
      <!--这个集合里使用到哪些报表-->    
      <reports/>    
     </reportSet>    
    </reportSets>    
   </plugin>    
  </plugins>    
 </reporting>
```

#### 5.项目信息

name：给用户提供更为友好的项目名

description：项目描述，maven文档中保存

url：主页的URL，maven文档中保存

inceptionYear：项目创建年份，4位数字。当产生版权信息时需要使用这个值

licenses：该元素描述了项目所有License列表。 应该只列出该项目的-

license列表，不要列出依赖项目的 license列表。如果列出多个license，用户可以选择它们中的一个而不是接受所有license。（如下）

```xml
<license>    
    <!--license用于法律上的名称-->    
    <name>...</name>     
    <!--官方的license正文页面的URL-->    
    <url>....</url>
    <!--项目分发的主要方式：repo，可以从Maven库下载 manual， 用户必须手动下载和安装依赖-->    
    <distribution>repo</distribution>     
    <!--关于license的补充信息-->    
    <comments>....</comments>     
</license>
```

- organization：1.name 组织名 2.url 组织主页url
- developers：项目开发人员列表（如下）
- contributors：项目其他贡献者列表，同developers

```xml
<developers>  
    <!--某个开发者信息-->
    <developer>  
        <!--开发者的唯一标识符-->
        <id>....</id>  
        <!--开发者的全名-->
        <name>...</name>  
        <!--开发者的email-->
        <email>...</email>  
        <!--开发者的主页-->
        <url>...<url/>
        <!--开发者在项目中的角色-->
        <roles>  
            <role>Java Dev</role>  
            <role>Web UI</role>  
        </roles> 
        <!--开发者所属组织--> 
        <organization>sun</organization>  
        <!--开发者所属组织的URL-->
        <organizationUrl>...</organizationUrl>  
        <!--开发者属性，如即时消息如何处理等-->
        <properties>
            <!-- 和主标签中的properties一样，可以随意定义子标签 -->
        </properties> 
        <!--开发者所在时区， -11到12范围内的整数。--> 
        <timezone>-5</timezone>  
    </developer>  
</developers>
```

#### 6.环境配置

issueManagement
目的问题管理系统(Bugzilla, Jira, Scarab)的名称和URL

```xml
<issueManagement>
    <system>Bugzilla</system>
    <url>http://127.0.0.1/bugzilla/</url>
</issueManagement>
```

ciManagement
项目的持续集成信息

```xml
<ciManagement>
    <system>continuum</system>
    <url>http://127.0.0.1:8080/continuum</url>
    <notifiers>
      <notifier>
        <type>mail</type>
        <sendOnError>true</sendOnError>
        <sendOnFailure>true</sendOnFailure>
        <sendOnSuccess>false</sendOnSuccess>
        <sendOnWarning>false</sendOnWarning>
        <address>continuum@127.0.0.1</address>
        <configuration></configuration>
      </notifier>
    </notifiers>
  </ciManagement>
```

system：持续集成系统的名字

url：持续集成系统的URL

notifiers：构建完成时，需要通知的开发者/用户的配置项。包括被通知者信息和通知条件（错误，失败，成功，警告）
 type：通知方式
 sendOnError：错误时是否通知
 sendOnFailure：失败时是否通知
 sendOnSuccess：成功时是否通知
 sendOnWarning：警告时是否通知
 address：通知发送到的地址
 configuration：扩展项

mailingLists
项目相关邮件列表信息

```xml
<mailingLists>
    <mailingList>
      <name>User List</name>
      <subscribe>user-subscribe@127.0.0.1</subscribe>
      <unsubscribe>user-unsubscribe@127.0.0.1</unsubscribe>
      <post>user@127.0.0.1</post>
      <archive>http://127.0.0.1/user/</archive>
      <otherArchives>
        <otherArchive>http://base.google.com/base/1/127.0.0.1</otherArchive>
      </otherArchives>
    </mailingList>
    .....
  </mailingLists>
```

subscribe, unsubscribe: 订阅邮件（取消订阅）的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建

archive：浏览邮件信息的URL

post：接收邮件的地址

scm
许你配置你的代码库，供Maven web站点和其它插件使用

```xml
<scm>
    <connection>scm:svn:http://127.0.0.1/svn/my-project</connection>
    <developerConnection>scm:svn:https://127.0.0.1/svn/my-project</developerConnection>
    <tag>HEAD</tag>
    <url>http://127.0.0.1/websvn/my-project</url>
  </scm>
```

connection, developerConnection：这两个表示我们如何连接到maven的版本库。connection只提供读，developerConnection将提供写的请求
 写法如：scm:[provider]:[provider_specific]
 如果连接到CVS仓库，可以配置如下：-

scm:cvs:pserver:127.0.0.1:/cvs/root:my-project

tag：项目标签，默认HEAD

url：共有仓库路径

prerequisites
项目构建的前提

```xml
<prerequisites>
    <maven>2.0.6</maven>
</prerequisites>
```

epositories,pluginRepositories
依赖和扩展的远程仓库列表，同上篇文章，setting.xml配置中介绍的。

```xml
<repositories>
    <repository>
      <releases>
        <enabled>false</enabled>
        <updatePolicy>always</updatePolicy>
        <checksumPolicy>warn</checksumPolicy>
      </releases>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>never</updatePolicy>
        <checksumPolicy>fail</checksumPolicy>
      </snapshots>
      <id>codehausSnapshots</id>
      <name>Codehaus Snapshots</name>
      <url>http://snapshots.maven.codehaus.org/maven2</url>
      <layout>default</layout>
    </repository>
  </repositories>
  <pluginRepositories>
    ...
  </pluginRepositories>
```

releases, snapshots:这是各种构件的策略，release或者snapshot。这两个集合，POM就可以根据独立仓库任意类型的依赖改变策略。如：一个人可能只激活下载snapshot用来开发。

enable：true或者false，决定仓库是否对于各自的类型激活(release 或者 snapshot)。

updatePolicy: 这个元素决定更新频率。maven将比较本地pom的时间戳（存储在仓库的maven数据文件中）和远程的. 有以下选择: always, daily (默认), interval:X (x是代表分钟的整型) ， never.

checksumPolicy：当Maven向仓库部署文件的时候，它也部署了相应的校验和文件。可选的为：ignore，fail，warn，或者不正确的校验和。

layout：在上面描述仓库的时候，提到他们有统一的布局。Maven 2有它仓库默认布局。然而，Maven 1.x有不同布局。使用这个元素来表明它是default还是legacy。



### 基本实例介绍



### 常用注解

SpringBoot的核心注解@SpringBootApplication以及run方法，理解下springBoot为什么不需要XML，达到零配置。<https://juejin.im/post/5ce3a27ce51d455d6d535769> 



1.@SpringBootApplication  等同于@ComponentScan+@Configuration+@EnableAutoConfiguration

2.@RestController 等同于@Controller+@ResponseBody

3.@Autowired

#### 1.@SpringBootApplication  

等同于@ComponentScan+@Configuration+@EnableAutoConfiguration详解

在了解@ComponentScan之前，我们先了解下Spring，Spring是一个依赖注入(dependency injection)框架,所有的内容都是关于bean的定义及其依赖关系。定义Spring Beans的第一步是使用正确的注解@Component或@Service或@Repository或@Controller等这些注解的类，Spring就会把他们注册成为Bean。

##### 1.1 @ComponentScan

对于@ComponentScan来说不仅可以把以上的@Component或@Service或@Repository或@Controller等这些注解的类注册成为Bean，包括带有@Configuration类。

```java
package com.tydic.springboot.springbootdemo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class SpringbootDemo {

public static void main(String[] args) {
    ApplicationContext applicationContext = 
            SpringApplication.run(SpringbootIn10StepsApplication.class, args);
 
    for (String name : applicationContext.getBeanDefinitionNames()) {
        System.out.println(name);
    }
}
```

类 SpringbootDemo 在com.tydic.springboot.springbootdemo包下，这个类使用了@SpringBootApplication注解，该注解定义了Spring将自动扫描包com.tydic.springboot.springbootdemo及其子包下的带有@Service,@Repository等注解的类。
但假如你一个类定义在包com.tydic.springboot.other下，那么你的启动类和你新建的这个新包不属于同级包及其子包,这个时候我们的@ComponentScan就会起到作用了。

定义@CoponentScan(“com.tydic.springboot.other”)

这么做扫描的范围扩大到整个父包com.tydic.springboot.springbootdemo



```java
package com.tydic.springboot.springbootdemo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.ComponentScan;

@ComponentScan(basePackages = "com.tydic.other")
@SpringBootApplication
public class SpringbootDemo {

public static void main(String[] args) {
    ApplicationContext applicationContext = 
            SpringApplication.run(SpringbootIn10StepsApplication.class, args);
 
    for (String name : applicationContext.getBeanDefinitionNames()) {
        System.out.println(name);
    }
}
```

总之一句话：ComponentScan做的事情就是告诉Spring从哪里找到bean

##### 1.2 @Configuration  

SpringBoot推荐使用java代码的形式申明注册bean，@Configuration注解可以用java代码的形式实现spring中xml配置文件配置的效果。所以@Configuration这个注解等同于spring的xml配置文件。

能够去注册一些额外的Bean，并且导入一些额外的配置。那@Configuration还有一个作用就是把该类变成一个配置类，不需要额外的XML进行配置。所以@SpringBootConfiguration就相当于@Configuration。

###### 1.2.1通过java代码注册bean

```java
@Configuration
public class TestMybaitsConf {
@Bean
public DataSource dataSource() {
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    try {
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost/CMX?useUnicode=true&amp;characterEncoding=utf-8");
        dataSource.setUser("root");
        dataSource.setPassword("123456");
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
    return dataSource;
}
 
@Bean
public SqlSessionFactory sqlSessionFactory(DataSource dataSource) {
    SqlSessionFactory factory = null;
    SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
    bean.setDataSource(dataSource);
    ResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
    bean.setConfigLocation(resolver.getResource("classpath:mybatis.xml"));
    try {
        factory = bean.getObject();
    } catch (Exception e) {
        throw new RuntimeException(e);
    }
    return factory;
}
 
@Bean
public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory) {
    return new SqlSessionTemplate(sqlSessionFactory);
}
 
@Bean
public PlatformTransactionManager transactionManager(DataSource dataSource) {
    return new DataSourceTransactionManager(dataSource);
}
```

###### 1.2.2使用xml配置bean

```java
@Configuration
@ImportResource("classpath:spring-mybatis.xml")
public class TestMybaitsConf {
}
```

spring-mybatis.xml :

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xmlns:mvc="http://www.springframework.org/schema/mvc" xmlns:tx="http://www.springframework.org/schema/tx"
    xmlns:jee="http://www.springframework.org/schema/jee" xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/mvc  http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/jee  http://www.springframework.org/schema/jee/spring-jee-4.0.xsd
        http://www.springframework.org/schema/beans  http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context  http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/tx  http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">
 
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://192.168.100.25:6660/TXSMS?useUnicode=true&amp;characterEncoding=utf-8"></property>
        <property name="user" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>
 
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="configLocation" value="classpath:mybatis.xml"></property>
    </bean>
 
    <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg index="0" ref="sqlSessionFactory"></constructor-arg>
    </bean>
 
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
 
    <tx:annotation-driven transaction-manager="transactionManager"/>
</beans>
```

总结：两种注册bean的效果完全一样，但springboot推荐使用1.2.1中的方式，使用java代码注册bean

##### 1.3 @EnableAutoConfiguration 

自动配置，此注释自动载入应用程序所需的所有Bean——这依赖于Spring Boot在类路径中的查找，这个注释告诉SpringBoot“猜”你将如何想配置Spring,基于你已经添加jar依赖项。如果spring-boot-starter-web已经添加           Tomcat和Spring MVC,这个注释自动将假设您正在开发一个web应用程序并添加相应的spring设置。

#### 2.@RestController 

等同于@Controller+@ResponseBody 表示这是个控制器bean,并且是将函数的返回值直 接填入HTTP响应体中,是REST风格的控制器。

```java
@Controller
public class User1MapperController {
 
    @ResponseBody
    @RequestMapping("/addUser1")
    public String addUser1(String name,Integer age){
        user1Mapper.addUser1(name,age);
        return "成功。。。。";
    }
}
```

等同于

```java
@RestController
public class User1MapperController {
 
    @RequestMapping("/addUser1")
    public String addUser1(String name,Integer age){
        user1Mapper.addUser1(name,age);
        return "成功。。。。";
    }
}
```

#### 3.@Autowired 

自动导入依赖的bean(通过@Autowired注入spring管理的bean)

```java
@Controller
public class User1MapperController {
 
    @Autowired
    private User1Mapper user1Mapper;
 
    @Autowired
    private User2Mapper user2Mapper;
 
    @ResponseBody
    @RequestMapping("/addUser1")
    public String addUser1(String name,Integer age){
        user1Mapper.addUser1(name,age);
        return "成功。。。。";
    }
 
    @ResponseBody
    @RequestMapping("/addUser2")
    public String addUser2(String name,Integer age){
        user2Mapper.addUser2(name,age);
        return "成功。。。!!";
    }
 
}
```



spring的bean的加载过程，解析springIOC加载过程。如果你看过Spring源码的话 ，应该知道这些方法都是做什么的。现在我们不关心其他的，我们来看一个方法叫做 onRefresh();方法。

















# 笔记

## KindleNote

<https://github.com/BadTudou/KindleNote-Rails>，https://ruby-china.org/topics/34814

**KindleNote**可以导出您**Kindle**中的**标注**与**笔记**，并支持将它们转换为**MarkDown**文件。

您可以选择将导出的**标记**与**笔记**存储于**Evernote**、**有道云笔记**等云笔记平台，或者**KindleNote**的服务器中。

## 功能

- [x] **笔记导出为Markdown** 
- [x] **笔记保存到Evernote** 
- [x] **批量导出为Markdown / 导出到第三方云笔记 / 删除** 
- [x] **通过豆瓣图书自动获取笔记对应的图书信息** 
- [x] **重复笔记自动合并** 
- [x] **第三方登录：QQ** 
- [x] **第三方登录：Evernote** 
- [ ] **分享到QQ空间、微博等社交网站** 
- [ ] **笔记保存到有道云笔记** 
- [ ] **搜索笔记** 



## 笔记管理工具Clippings

 Kindle Mate

Klib: <https://toolinbox.net/Klib/>



对比一下我用过的几款 PDF 标记软件，主要是 [Marginnote](https://itunes.apple.com/cn/app/marginnote-pro/id723205553?mt=8&at=10lJSw)，[PDF Expert](https://itunes.apple.com/us/app/pdf-expert-by-readdle/id743974925?mt=8)，[iAnnotate 4](https://itunes.apple.com/cn/app/iannotate-4-pdfs-more/id1093924230?mt=8&at=10lJSw)，以及 [Evernote](https://itunes.apple.com/cn/app/evernote/id281796108?mt=8&at=10lJSw) （你没看错，就是Evernote），希望思路对大家有帮助。



# JAVA学习

## 跨平台

需要安装JVM，java虚拟机



## JRE和JDK

java runtime environment，java运行环境

包括JVM和java程序需要的核心类库

如果要运行一个java程序则需要安装jre即可

java development kit，JDKjava开发工具包

JDK提供给开发人员使用，其中保护开发工具，也包括了JRE。其中开发工具：编译工具javac.exe，打包工具jar.exe

JDK开发完成的程序交给jre去运行。






