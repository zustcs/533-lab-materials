# 什么是Maven

乍一看，Maven似乎包含很多内容，但简而言之，Maven试图将模式应用于项目的构建基础设施，以便通过提供使用最佳实践的清晰路径来促进理解和生产力。Maven本质上是一个项目管理和理解工具，因此提供了一种帮助管理的方法:

- Builds
- Documentation
- Reporting
- Dependencies
- SCMs
- Releases
- Distribution

## 历史

Maven最初设计，是以简化Jakarta Turbine项目的建设。在几个项目，每个项目包含了不同的Ant构建文件。 JAR检查到CVS。

## 目标

1）为了使项目管理更加简单。

2）提供统一的构建系统。

3）提供优质项目的资讯。

4）为最佳实践开发提供指导。

5）允许透明地迁移到新特性。

## 创建

```
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

生成的文件目录：

```
my-app
|-- pom.xml
`-- src
    |-- main
    |   `-- java
    |       `-- com
    |           `-- mycompany
    |               `-- app
    |                   `-- App.java
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
```

### 创建一个工程

```
mvn package
```

结果：

```
 ...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2 seconds
[INFO] Finished at: Thu Jul 07 21:34:52 CEST 2011
[INFO] Final Memory: 3M/6M
[INFO] ------------------------------------------------------------------------
```

与执行的第一个命令（architetype:generate）不同，您可能会注意到第二个命令只是一个单词package。这不是一个目标，而是一个阶段。阶段是构建生命周期中的一个步骤，它是一个有序的阶段序列。当一个阶段被给定时，Maven将执行序列中的每个阶段，直到并包括定义的阶段。例如，如果我们执行编译阶段，实际执行的阶段是：

```
validate
generate-sources
process-sources
generate-resources
process-resources
compile
```

### 编译jar包

```
java -cp target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App
```

输出如下：

```
Hello World!
```

### Java9或者之后的版本

默认情况下，Maven版本可能使用Maven -compiler-plugin的旧版本，与Java 9或更高版本不兼容。要针对Java 9或更高版本，您至少应该使用maven-compiler-plugin的3.6.0版本，并将maven.compiler.release属性设置为您要针对的Java版本(例如9、10、11、12等)。
在下面的例子中，我们将Maven项目配置为使用Maven -compiler-plugin的3.8.1版本和目标Java 11:

```
    <properties>
        <maven.compiler.release>11</maven.compiler.release>
    </properties>
 
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
```

## Maven阶段

- **validate**: 验证项目是正确的，并且所有必要的信息都是可用的。
- **compile**: 编译项目的源代码。
- **test**:使用合适的单元测试框架测试编译后的源代码。这些测试不应该要求打包或部署代码。
- **package**:将编译后的代码打包成可分发的格式，比如JAR。
- **integration-test**: 如果需要，将包处理并部署到可以运行集成测试的环境中。
- **verify**: 运行任何检查来验证包是否有效并满足质量标准。
- **install**: 将包安装到本地存储库中，以便在本地的其他项目中作为依赖项使用。
- **deploy**: 在集成或发布环境中完成，将最终的包复制到远程存储库，以便与其他开发人员和项目共享。

在上面的缺省列表之外，还有两个Maven生命周期值得注意。他们是：

- **clean**: 清理先前构建创建的工件。

- **site**: 为这个项目生成站点文档。

# 快速开始

## 创建工程

使用命令：

```
mvn -B archetype:generate \
  -DarchetypeGroupId=org.apache.maven.archetypes \
  -DgroupId=com.mycompany.app \
  -DartifactId=my-app
```

执行此命令后，您将注意到发生了一些事情。首先，您将注意到为新项目创建了一个名为my-app的目录，该目录包含一个名为pom.xml的文件，如下所示:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

**pom.xml**包含该项目的项目对象模型(POM)。POM是Maven中的基本工作单元。记住这一点很重要，因为Maven本质上是以项目为中心的，因为一切都围绕着项目的概念。简而言之，POM包含关于项目的所有重要信息，本质上是一站式搜索，查找与项目相关的任何内容。理解POM非常重要，我们鼓励新用户参考对POM的介绍。
这是一个非常简单的POM，但仍然显示每个POM包含的关键元素 ,所以让我们逐一介绍一下，让您熟悉POM的要点:

- **project** 这是所有Maven pom.xml文件中的顶级元素。
- **modelVersion** 此元素指示此POM使用的对象模型的版本。模型本身的版本更改非常不频繁，但如果Maven开发人员认为有必要更改模型，则必须更改模型，以确保使用的稳定性。
- **groupId** 此元素指示创建项目的组织或组的唯一标识符。groupId是项目的关键标识符之一，通常基于组织的完全限定域名。例如org.apache.maven。插件是所有Maven插件的指定groupId。
- **artifactId** 此元素指示此项目生成的主要构件的唯一基名称。项目的主要构件通常是一个JAR文件。像源包这样的次要构件也使用artifactId作为它们最终名称的一部分。Maven生成的典型工件的形式是<artifactId>-<version>.<扩展名>(例如，myapp-1.0.jar)。
- **packaging** 此元素指示此构件(例如JAR、WAR、EAR等)要使用的包类型。这不仅意味着如果生成的工件是JAR、WAR或EAR，还可以指示作为构建过程一部分使用的特定生命周期。(生命周期是我们将在指南中进一步讨论的主题。现在，请记住，项目的指定打包可以在定制构建生命周期中发挥一定的作用。打包元素的默认值是JAR，因此您不必为大多数项目指定此值。
- **version** 此元素指示由项目生成的工件的版本。Maven在帮助您进行版本管理方面走了很长的路，您经常会在版本中看到快照设计器，这表明项目处于开发状态。我们将在本指南中进一步讨论快照的使用及其工作原理。
- **name** 此元素指示用于项目的显示名称。这通常在Maven生成的文档中使用。
- **url** 此元素指示可以在何处找到项目的站点。这通常在Maven生成的文档中使用。
- **description** 此元素提供项目的基本描述。这通常在Maven生成的文档中使用。

## 编译Maven

切换到原型创建pom.xml的目录:生成并执行以下命令编译应用程序源代码:

```
mvn compile
```

执行此命令后，您将看到如下输出:

```
[INFO] ----------------------------------------------------------------------------
[INFO] Building Maven Quick Start Archetype
[INFO]    task-segment: [compile]
[INFO] ----------------------------------------------------------------------------
[INFO] artifact org.apache.maven.plugins:maven-resources-plugin: \
  checking for updates from central
...
[INFO] artifact org.apache.maven.plugins:maven-compiler-plugin: \
  checking for updates from central
...
[INFO] [resources:resources]
...
[INFO] [compiler:compile]
Compiling 1 source file to <dir>/my-app/target/classes
[INFO] ----------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ----------------------------------------------------------------------------
[INFO] Total time: 3 minutes 54 seconds
[INFO] Finished at: Fri Sep 23 15:48:34 GMT-05:00 2005
[INFO] Final Memory: 2M/6M
[INFO] ----------------------------------------------------------------------------
```

第一次执行这个(或任何其他)命令时，Maven将需要下载完成该命令所需的所有插件和相关依赖项。从Maven的干净安装来看，这可能需要相当长的时间(在上面的输出中，它花费了近4分钟)。如果您再次执行该命令，Maven现在将拥有它所需要的东西，因此它不需要下载任何新内容，并且能够更快地执行该命令。

从输出中可以看到，编译后的类放在${basedir}/target/classes中，这是Maven使用的另一种标准约定。因此，如果您是一个敏锐的观察者，您会注意到，通过使用标准约定，上面的POM非常小，您不必显式地告诉Maven您的任何源文件在哪里，或者输出应该放在哪里。通过遵循标准Maven约定，您可以用很少的努力完成很多工作!作为一个随意的比较，让我们来看看您在Ant中为了完成同样的事情可能必须做些什么。

现在，只需编译一个应用程序源代码树，所示Ant脚本的大小与上面所示POM的大小基本相同。但是，我们将看到我们可以用这个简单的POM做更多的事情!

## 我如何编译我的测试源并运行我的单元测试?

现在，您已经成功地编译了应用程序的源代码，并且已经有了一些需要编译和执行的单元测试(因为每个程序员总是编写和执行他们的单元测试*nudge nudge wink*)。

执行以下命令:

```
mvn test
```

执行此命令后，您将看到如下输出:

```
[INFO] ----------------------------------------------------------------------------
[INFO] Building Maven Quick Start Archetype
[INFO]    task-segment: [test]
[INFO] ----------------------------------------------------------------------------
[INFO] artifact org.apache.maven.plugins:maven-surefire-plugin: \
  checking for updates from central
...
[INFO] [resources:resources]
[INFO] [compiler:compile]
[INFO] Nothing to compile - all classes are up to date
[INFO] [resources:testResources]
[INFO] [compiler:testCompile]
Compiling 1 source file to C:\Test\Maven2\test\my-app\target\test-classes
...
[INFO] [surefire:test]
[INFO] Setting reports dir: C:\Test\Maven2\test\my-app\target/surefire-reports
 
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
[surefire] Running com.mycompany.app.AppTest
[surefire] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0 sec
 
Results :
[surefire] Tests run: 1, Failures: 0, Errors: 0
 
[INFO] ----------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ----------------------------------------------------------------------------
[INFO] Total time: 15 seconds
[INFO] Finished at: Thu Oct 06 08:12:17 MDT 2005
[INFO] Final Memory: 2M/8M
[INFO] ----------------------------------------------------------------------------
```

关于输出需要注意的一些事情:

Maven这次下载了更多的依赖项。这些是执行测试所需的依赖项和插件(它已经拥有编译所需的依赖项，不会再下载它们)。

在编译和执行测试之前，Maven编译主代码(所有这些类都是最新的，因为自上次编译以来，我们没有更改任何东西)。

如果你只是想编译你的测试源(而不是执行测试)，你可以执行以下步骤:

```
mvn test-compile
```

## 如何创建一个JAR并将其安装到本地存储库中?

制作JAR文件非常简单，可以通过执行以下命令来完成:

```
mvn package
```

如果您查看项目的POM，您会注意到打包元素被设置为jar。Maven就是这样知道如何从上面的命令生成JAR文件的(稍后我们将对此进行更多讨论)。现在可以查看${basedir}/target目录，您将看到生成的JAR文件。
现在，您将希望将生成的工件(JAR文件)安装到本地存储库(${user.home) /中。m2/repository是默认位置)。有关存储库的更多信息，您可以参考我们对存储库的介绍，但是让我们继续安装我们的工件!执行以下命令:

```
mvn install
```

执行此命令后，应该会看到以下输出:

```
[INFO] ----------------------------------------------------------------------------
[INFO] Building Maven Quick Start Archetype
[INFO]    task-segment: [install]
[INFO] ----------------------------------------------------------------------------
[INFO] [resources:resources]
[INFO] [compiler:compile]
Compiling 1 source file to <dir>/my-app/target/classes
[INFO] [resources:testResources]
[INFO] [compiler:testCompile]
Compiling 1 source file to <dir>/my-app/target/test-classes
[INFO] [surefire:test]
[INFO] Setting reports dir: <dir>/my-app/target/surefire-reports
 
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
[surefire] Running com.mycompany.app.AppTest
[surefire] Tests run: 1, Failures: 0, Errors: 0, Time elapsed: 0.001 sec
 
Results :
[surefire] Tests run: 1, Failures: 0, Errors: 0
 
[INFO] [jar:jar]
[INFO] Building jar: <dir>/my-app/target/my-app-1.0-SNAPSHOT.jar
[INFO] [install:install]
[INFO] Installing <dir>/my-app/target/my-app-1.0-SNAPSHOT.jar to \
   <local-repository>/com/mycompany/app/my-app/1.0-SNAPSHOT/my-app-1.0-SNAPSHOT.jar
[INFO] ----------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ----------------------------------------------------------------------------
[INFO] Total time: 5 seconds
[INFO] Finished at: Tue Oct 04 13:20:32 GMT-05:00 2005
[INFO] Final Memory: 3M/8M
[INFO] ----------------------------------------------------------------------------
```

请注意surefire插件(执行测试)寻找包含在具有特定命名约定的文件中的测试。默认情况下，测试包括:

- `**/*Test.java`
- `**/Test*.java`
- `**/*TestCase.java`

默认不包括:

- `**/Abstract*Test.java`
- `**/Abstract*TestCase.java`

您已经完成了设置、构建、测试、打包和安装典型Maven项目的过程。这可能是绝大多数项目将使用Maven做的事情，如果您已经注意到，到目前为止您所能做的一切都是由一个18行文件驱动的，即项目的模型或POM。如果您查看一个典型的Ant构建文件，该文件提供了与我们到目前为止已经实现的功能相同的功能，您将注意到它已经是POM的两倍大，而我们才刚刚开始!Maven提供了更多的功能，而不需要像现在这样对POM进行任何添加。要从示例Ant构建文件中获得更多功能，必须不断添加容易出错的内容。

那么你还能免费得到什么呢?有很多Maven插件可以用上面所述的简单POM开箱即用。我们将在这里特别提到一个，因为它是Maven非常宝贵的特性之一:不需要您做任何工作，这个POM就有足够的信息为您的项目生成一个web站点!您很可能想自定义Maven站点，但如果时间紧迫，您只需执行以下命令即可提供关于项目的基本信息:

```
mvn site
```

还有很多其他独立的目标也可以执行，例如:

```
mvn clean
```

## 什么是快照版本?

注意，下面显示的pom.xml文件中的version标记的值有后缀:`-SNAPSHOT`。

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  ...
  <groupId>...</groupId>
  <artifactId>my-app</artifactId>
  ...
  <version>1.0-SNAPSHOT</version>
  <name>Maven Quick Start Archetype</name>
  ...
```

快照值指的是沿着开发分支的“最新”代码，不能保证代码是稳定的或不变的。相反，“release”版本(任何没有后缀SNAPSHOT的版本值)中的代码是不变的。

换句话说，快照版本是最终“发布”版本之前的“开发”版本。快照比它的发布“更老”。

在发布过程中，x的一个版本。y-SNAPSHOT更改为x.y.发布过程也将开发版本增加到x.(y+1)-SNAPSHOT。例如，版本1.0- snapshot作为版本1.0发布，而新的开发版本是版本1.1-SNAPSHOT。

## 我如何使用插件?

无论何时您想为Maven项目定制构建，都可以通过添加或重新配置插件来完成。

Maven 1.0用户注意:在Maven 1.0中，您将向Maven .xml添加一些preGoal，并向project.properties添加一些条目。在这里，情况有点不同。

对于本例，我们将配置Java编译器以允许JDK 5.0源代码。这是简单的添加到您的POM:

```
...
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.5</source>
        <target>1.5</target>
      </configuration>
    </plugin>
  </plugins>
</build>
...
```

您将注意到Maven中的所有插件看起来都很像依赖项——在某些方面确实如此。这个插件将自动下载和使用-包括一个特定的版本，如果你要求它(默认是使用最新可用的)。

configuration元素将给定的参数应用于编译器插件中的每个目标。在上面的例子中，编译器插件已经被用作构建过程的一部分，这只是改变了配置。还可以向流程添加新目标，并配置特定的目标。有关这方面的信息，请参阅构建生命周期的介绍。

要了解插件的可用配置，可以查看插件列表，并导航到正在使用的插件和目标。有关如何配置插件的可用参数的一般信息，请参阅配置插件的指南。

## 如何向JAR添加资源?

另一个可以满足的常见用例是将资源打包到JAR文件中，它不需要修改上面的POM。对于这个常见的任务，Maven再次依赖于标准目录布局，这意味着通过使用标准Maven约定，您只需将这些资源放在标准目录结构中，就可以将资源打包到jar中。

您可以在下面的示例中看到，我们添加了${basedir}/src/main/resources目录，将希望打包到JAR中的任何资源放入其中。Maven使用的简单规则是:${basedir}/src/main/resources目录中放置的任何目录或文件都打包在JAR中，从JAR的底部开始使用完全相同的结构。

```
my-app
|-- pom.xml
`-- src
    |-- main
    |   |-- java
    |   |   `-- com
    |   |       `-- mycompany
    |   |           `-- app
    |   |               `-- App.java
    |   `-- resources
    |       `-- META-INF
    |           `-- application.properties
    `-- test
        `-- java
            `-- com
                `-- mycompany
                    `-- app
                        `-- AppTest.java
```

在我们的例子中，我们有一个META-INF目录和一个应用程序。该目录中的属性文件。如果您打开Maven为您创建的JAR并查看它，您将看到以下内容:

```
|-- META-INF
|   |-- MANIFEST.MF
|   |-- application.properties
|   `-- maven
|       `-- com.mycompany.app
|           `-- my-app
|               |-- pom.properties
|               `-- pom.xml
`-- com
    `-- mycompany
        `-- app
            `-- App.class
```

可以看到，`${basedir}/src/main/resources`的内容可以从JAR和我们的应用程序的底部开始找到。`application.properties`文件在`META-INF`目录中。您还会注意到其他一些文件，比如`META-INF/MANIFEST.MF`,以及`pom.xml` 和 `pom.properties`文件。这些都是Maven中生成JAR的标准配置。如果您选择，您可以创建自己的清单，但是Maven将在缺省情况下生成清单。(您还可以修改默认清单中的条目。这个我们以后再谈。)`pom.xml` 和`pom.properties`。属性文件打包在JAR中，因此Maven生成的每个构件都是自描述的，并且如果需要，还允许您在自己的应用程序中使用元数据。一个简单的用途可能是检索应用程序的版本。在POM文件上操作需要使用一些Maven实用程序，但是可以使用标准Java API使用这些属性，如下所示:

```
#Generated by Maven
#Tue Oct 04 15:43:21 GMT-05:00 2005
version=1.0-SNAPSHOT
groupId=com.mycompany.app
artifactId=my-app
```

要将资源添加到单元测试的类路径中，除了将资源放入的目录为${basedir}/src/test/resources之外，遵循与向JAR添加资源相同的模式。此时，您将拥有一个项目目录结构，如下所示:

```
my-app
|-- pom.xml
`-- src
    |-- main
    |   |-- java
    |   |   `-- com
    |   |       `-- mycompany
    |   |           `-- app
    |   |               `-- App.java
    |   `-- resources
    |       `-- META-INF
    |           |-- application.properties
    `-- test
        |-- java
        |   `-- com
        |       `-- mycompany
        |           `-- app
        |               `-- AppTest.java
        `-- resources
            `-- test.properties
```

在单元测试中，您可以使用如下简单的代码片段来访问测试所需的资源:

```
...
 
// Retrieve resource
InputStream is = getClass().getResourceAsStream( "/test.properties" );
 
// Do something with the resource
 
...
```

## 如何过滤资源文件?

有时，资源文件需要包含一个只能在构建时提供的值。要在Maven中实现这一点，可以使用${<property name>}语法将包含值的属性引用放到资源文件中。属性可以是pom中定义的值之一。xml，用户设置中定义的值。xml，在外部属性文件或系统属性中定义的属性。

要让Maven在复制时过滤资源，只需将pom.xml中的资源目录的`filtering`设置为true:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
 
  <build>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>
</project>
```

您将注意到，我们必须添加以前没有的 `build`, `resources` 和 `resource`元素。此外，我们必须显式地声明资源位于src/main/resources目录中。所有这些信息都是以前作为默认值提供的，但是因为用于`filtering`的默认值是false，所以我们必须将其添加到pom.xml中，以便覆盖该默认值并将`filtering`设置为true。

引用pom中定义的属性。属性名使用定义值的xml元素的名称，允许“pom”作为项目(根)元素的别名。因此`${project.name}`引用项目的名称，`${project.version}`引用项目的版本，`${project.build.finalName}`是指在打包构建的项目时创建的文件的最终名称，等等。请注意，POM的一些元素有默认值，因此不需要在`pom.xml`中显式地定义这些值。类似地，可以使用以“settings”开头的属性名引用用户`settings.xml`中的值(例如`${settings.localRepository}`引用用户的本地存储库的路径)。

为了继续我们的示例，让我们向`application.properties`添加几个属性(我们把它放在src/main/resources目录中)，当资源被过滤时，它的值将被提供:

```
# application.properties
application.name=${project.name}
application.version=${project.version}
```

有了它，您可以执行以下命令(process-resources是复制和过滤资源的构建生命周期阶段):

```
mvn process-resources
```

和`target/classes`下的`application.properties`(最终会进入jar)看起来是这样的:

```
# application.properties
application.name=Maven Quick Start Archetype
application.version=1.0-SNAPSHOT
```

要引用外部文件中定义的属性，只需在pom.xml中添加对该外部文件的引用。首先，让我们创建外部属性文件并调用它:

`src/main/filters/filter.properties`:

```
# filter.properties
my.filter.value=hello!
```

接下来，我们将在pom.xml中添加对这个新文件的引用:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
 
  <build>
    <filters>
      <filter>src/main/filters/filter.properties</filter>
    </filters>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>
</project>
```

然后，如果我们在应用程序中添加对该属性的引用。属性文件:

```
# application.properties
application.name=${project.name}
application.version=${project.version}
message=${my.filter.value}
```

`mvn process-resources`命令的下一个执行将把我们的新属性值放入`application.properties`。作为定义my.filter.value 的替代方法。在外部文件中，您也可以在pom.xml的properties部分中定义value属性，您将得到相同的效果(注意，我不需要对`src/main/filters/filter.properties`的引用):

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
 
  <build>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>
 
  <properties>
    <my.filter.value>hello</my.filter.value>
  </properties>
</project>
```

过滤资源还可以从系统属性中获取值;要么是内置到Java中的系统属性(比如`java.version` 或者`user.home`)。或在命令行上使用标准Java -D参数定义的属性。为了继续这个示例，让我们更改我们的应用程序。属性文件看起来像这样:

```
# application.properties
java.version=${java.version}
command.line.prop=${command.line.prop}
```

现在，当您执行以下命令时(注意command.line.prop的定义)，`application.properties`将包含来自系统属性的值。

```
mvn process-resources "-Dcommand.line.prop=hello again"
```

## 我如何使用外部依赖?

您可能已经注意到我们作为示例使用的POM中有一个`dependencies`元素。实际上，您一直在使用外部依赖项，但在这里我们将更详细地讨论它是如何工作的。有关更详细的介绍，请参阅我们对依赖机制的介绍。

pom.xml的`dependencies`部分列出了我们的项目为了构建而需要的所有外部依赖项(无论是在编译时、测试时、运行时还是其他时候)。现在，我们的项目只依赖于JUnit(为了清晰起见，我去掉了所有的资源过滤):

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

对于每个外部依赖项，至少需要定义4个东西:groupId、artifactId、version和scope。groupId、artifactId和版本与构建该依赖项的项目的`pom.xml`中给出的版本相同。scope元素指示项目如何使用该依赖项，可以是`compile`、`test`和`runtime`等值。有关可以为依赖项指定的所有内容的更多信息，请参见项目描述符引用(https://maven.apache.org/ref/3.6.1/maven-model/maven.html)。

有了这些关于依赖项的信息，Maven将能够在构建项目时引用依赖项。Maven从哪里引用依赖项?Maven查看本地存储库(`${user.home}/.m2/repository`是默认位置)来查找所有依赖项。在前一节中，我们将项目中的构件(my-app-1.0- snap .jar)安装到本地存储库中。一旦它安装在那里，另一个项目就可以将该jar引用为依赖项，只需将依赖项信息添加到它的pom.xml:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-other-app</artifactId>
  ...
  <dependencies>
    ...
    <dependency>
      <groupId>com.mycompany.app</groupId>
      <artifactId>my-app</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
</project>
```

那么在其他地方构建的依赖关系呢?它们如何进入我的本地存储库?当项目引用本地存储库中不可用的依赖项时，Maven将从远程存储库下载该依赖项到本地存储库。您可能注意到Maven在构建第一个项目时下载了很多东西(这些下载是用于构建项目的各种插件的依赖项)。默认情况下，可以通过http://repo.maven.apache.org/maven2/找到(并浏览)Maven使用的远程存储库。您还可以设置自己的远程存储库(可能是您公司的一个中央存储库)来代替或附加使用默认的远程存储库。有关存储库的更多信息，请参阅存储库介绍。

让我们为项目添加另一个依赖项。假设我们在代码中添加了一些日志记录，并且需要添加log4j作为依赖项。首先，我们需要知道log4j的groupId、artifactId和版本。Maven中心上的适当目录称为/maven2/log4j/log4j。在该目录中有一个名为maven-metada .xml的文件。log4j的maven-metada .xml是这样的:

```
<metadata>
  <groupId>log4j</groupId>
  <artifactId>log4j</artifactId>
  <version>1.1.3</version>
  <versioning>
    <versions>
      <version>1.1.3</version>
      <version>1.2.4</version>
      <version>1.2.5</version>
      <version>1.2.6</version>
      <version>1.2.7</version>
      <version>1.2.8</version>
      <version>1.2.11</version>
      <version>1.2.9</version>
      <version>1.2.12</version>
    </versions>
  </versioning>
</metadata>
```

从这个文件中，我们可以看到我们想要的groupId是“log4j”，而artifactId是“log4j”。我们看到有很多不同的版本值可供选择;现在，我们只使用最新版本1.2.12(一些maven-metada .xml文件也可能指定哪个版本是当前版本)。在maven-metada .xml文件旁边，我们可以看到与log4j库的每个版本对应的目录。在这些文件中，我们将找到实际的**jar文件**(例如log4j-1.2.12.jar)、**pom文件**(这是依赖项的pom.xml，表示它可能具有的任何进一步依赖项和其他信息)和另一个**maven-metada .xml文件**。还有一个**md5文件**对应于每个文件，其中包含这些文件的md5散列。您可以使用它对库进行身份验证，或者确定您可能已经在使用某个特定库的哪个版本。

现在我们知道了需要的信息，可以将依赖项添加到pom.xml:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.12</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
</project>
```

### 依赖传递

Maven通过自动包含传递依赖项，避免了发现和指定您自己的依赖项所需的库。

通过从指定的远程存储库中读取依赖项的项目文件，可以简化此功能。通常，这些项目的所有依赖项都将在您的项目中使用，就像项目从父项目继承的依赖项或从依赖项继承的依赖项一样。

可以收集依赖项的级别数量没有限制。只有在发现循环依赖项时才会出现问题。

使用传递依赖关系，所包含的库的图可以快速增长得相当大。由于这个原因，有一些额外的功能限制哪些依赖关系包括在内:

**依赖项中介**——当遇到多个版本作为依赖项时，它决定将选择工件的哪个版本。Maven选择“**最近的定义**”。也就是说，它使用依赖树中与项目最接近的依赖项的版本。您总是可以通过在项目的POM中显式声明一个版本来保证该版本。请注意，如果依赖项树中的两个依赖项版本具有相同的深度，则第一个声明将获胜。

> “最接近的定义”意味着所使用的版本将是依赖关系树中最接近您的项目的版本。例如，如果A、B和C的依赖关系定义为A -> B -> C -> D 2.0和A -> E -> D 1.0，那么在构建A时将使用D 1.0，因为从A到D到E的路径更短。您可以在a中显式地向d2.0添加依赖项，以强制使用d2.0。

**依赖关系管理**——这允许项目作者直接指定工件的版本，当它们遇到传递依赖项或没有指定版本的依赖项时使用。在前面的示例中部分依赖直接添加到即使它是不能直接使用的a .相反,可以包括D作为依赖dependencyManagement部分和直接控制哪个版本的D时使用,或者是引用。

**依赖关系范围**——这允许您只包含适合当前构建阶段的依赖关系。下面将对此进行更详细的描述。

**排除依赖关系**——如果项目X依赖于项目Y，而项目Y依赖于项目Z，那么项目X的所有者可以使用“"exclusion”元素显式地排除项目Z作为依赖关系。

**可选依赖项**——如果项目Y依赖于项目Z，项目Y的所有者可以使用“optional”元素将项目Z标记为可选依赖项。当项目X依赖于项目Y时，X将只依赖于Y，而不依赖于Y的可选依赖项Z。(将可选依赖项视为“默认排除”可能会有所帮助。)

虽然传递依赖项可以隐式地包含所需的依赖项，但显式地指定直接在源代码中使用的依赖项是一个很好的实践。这一最佳实践证明了它的价值，特别是当项目的依赖项更改其依赖项时。

例如,假设您的项目指定一个依赖另一个项目B, B和项目指定依赖项目C .如果你直接使用组件项目C,和你不指定项目C在您的项目中,它可能会导致构建失败当项目B突然更新/删除项目C的依赖。

直接指定依赖关系的另一个原因是，它为您的项目提供了更好的文档:只需阅读项目中的POM文件就可以了解更多信息。

Maven还提供了依赖关系:分析插件目标来分析依赖关系:它有助于使这一最佳实践更容易实现。

### 依赖范围

依赖项的范围——`compile`, `runtime`, `test`, `system`和 `provided`。用于计算用于编译、测试等的各种类路径。它还帮助确定在这个项目的发行版中包含哪些工件。有关更多信息，请参见依赖机制。默认范围是compile。

依赖关系管理是Maven的一个核心特性。管理单个项目的依赖关系很容易。管理由数百个模块组成的多模块项目和应用程序的依赖关系是可能的。Maven在使用定义良好的类路径和库版本定义、创建和维护可重复构建方面帮助很大。

依赖范围用于限制依赖项的传递性，还用于影响用于各种构建任务的类路径。

有6种适用范围:

**compile**

这是默认范围，如果没有指定则使用。编译依赖项在项目的所有类路径中都可用。此外，这些依赖项将传播到依赖的项目。

**provided**

这很像`compile`，但表明您希望JDK或容器在运行时提供依赖项。例如，当为Java Enterprise Edition构建web应用程序时，您将对Servlet API和相关Java EE API的依赖scope设置为`provided`，因为web容器提供了这些类。此范围仅在编译和测试类路径上可用，且不可传递。

**runtime**

此范围指示此依赖项不是编译所需的，而是执行所需的。它位于运行时和测试类路径中，但不在编译类路径中。

**test**

此范围表明，应用程序的正常使用不需要依赖项，仅在测试编译和执行阶段可用。这个范围不是可传递的。

**system**

除了必须显式地提供包含它的JAR之外，此范围与`provided`的类似。工件总是可用的，并且不会在存储库中查找。

**import**

此范围仅在`<dependencyManagement>`部分的`pom`类型依赖项上受支持。它指示要用指定POM的`<dependencyManagement>`节中的有效依赖项列表替换依赖项。由于替换了依赖项，具有导入范围的依赖项实际上并不参与限制依赖项的传递性。

每个范围(import除外)都以不同的方式影响传递依赖关系，如下表所示。如果将依赖项设置为左列中的作用域，则该依赖项与第一行中的作用域的传递依赖项将导致主项目中的依赖项，其作用域列在交集处。如果没有列出范围，则意味着将省略依赖项。

|          | compile    | provided | runtime  | test |
| -------- | ---------- | -------- | -------- | ---- |
| compile  | compile(*) | -        | runtime  | -    |
| provided | provided   | -        | provided | -    |
| runtime  | runtime    | -        | runtime  | -    |
| test     | test       | -        | test     | -    |

(*)注意:这应该是运行时范围，以便所有编译依赖项必须显式列出。但是，如果您所依赖的库从另一个库扩展了一个类，那么这两个库必须在编译时可用。因此，即使编译时依赖项是传递的，它们仍然作为编译范围。

### 依赖管理

依赖项管理部分是集中化依赖项信息的机制。当您有一组继承公共父类的项目时，可以将所有关于依赖关系的信息放在公共POM中，并对子POMs中的构件有更简单的引用。通过一些例子可以很好地说明这种机制。给定这两个延伸相同父节点的POMs:

Project A::

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-a</artifactId>
      <version>1.0</version>
      <exclusions>
        <exclusion>
          <groupId>group-c</groupId>
          <artifactId>excluded-artifact</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-b</artifactId>
      <version>1.0</version>
      <type>bar</type>
      <scope>runtime</scope>
    </dependency>
  </dependencies>
</project>
```

Project B:

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>group-c</groupId>
      <artifactId>artifact-b</artifactId>
      <version>1.0</version>
      <type>war</type>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-b</artifactId>
      <version>1.0</version>
      <type>bar</type>
      <scope>runtime</scope>
    </dependency>
  </dependencies>
</project>
```

这两个示例POMs共享一个公共依赖项，并且每个POMs都有一个重要的依赖项。这些信息可以像这样放在父POM中:

```
<project>
  ...
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>group-a</groupId>
        <artifactId>artifact-a</artifactId>
        <version>1.0</version>
 
        <exclusions>
          <exclusion>
            <groupId>group-c</groupId>
            <artifactId>excluded-artifact</artifactId>
          </exclusion>
        </exclusions>
 
      </dependency>
 
      <dependency>
        <groupId>group-c</groupId>
        <artifactId>artifact-b</artifactId>
        <version>1.0</version>
        <type>war</type>
        <scope>runtime</scope>
      </dependency>
 
      <dependency>
        <groupId>group-a</groupId>
        <artifactId>artifact-b</artifactId>
        <version>1.0</version>
        <type>bar</type>
        <scope>runtime</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
</project>
```

然后两个孩子的poms变得简单多了:

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-a</artifactId>
    </dependency>
 
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-b</artifactId>
      <!-- This is not a jar dependency, so we must specify type. -->
      <type>bar</type>
    </dependency>
  </dependencies>
</project>
```

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>group-c</groupId>
      <artifactId>artifact-b</artifactId>
      <!-- This is not a jar dependency, so we must specify type. -->
      <type>war</type>
    </dependency>
 
    <dependency>
      <groupId>group-a</groupId>
      <artifactId>artifact-b</artifactId>
      <!-- This is not a jar dependency, so we must specify type. -->
      <type>bar</type>
    </dependency>
  </dependencies>
</project>
```

注意:在这两个依赖项引用中，我们必须指定`<type/>`元素。这是因为，针对dependencyManagement部分匹配依赖项引用的最小信息集实际上是{groupId、artifactId、type、classifier}。在许多情况下，这些依赖关系将引用没有分类器的jar构件。这允许我们将标识简写为{groupId, artifactId}，因为类型字段的缺省值是jar，缺省分类器是null。

依赖项管理部分的第二个非常重要的用途是控制传递依赖项中使用的工件的版本。例如，考虑以下项目:

Project A:

```
<project>
 <modelVersion>4.0.0</modelVersion>
 <groupId>maven</groupId>
 <artifactId>A</artifactId>
 <packaging>pom</packaging>
 <name>A</name>
 <version>1.0</version>
 <dependencyManagement>
   <dependencies>
     <dependency>
       <groupId>test</groupId>
       <artifactId>a</artifactId>
       <version>1.2</version>
     </dependency>
     <dependency>
       <groupId>test</groupId>
       <artifactId>b</artifactId>
       <version>1.0</version>
       <scope>compile</scope>
     </dependency>
     <dependency>
       <groupId>test</groupId>
       <artifactId>c</artifactId>
       <version>1.0</version>
       <scope>compile</scope>
     </dependency>
     <dependency>
       <groupId>test</groupId>
       <artifactId>d</artifactId>
       <version>1.2</version>
     </dependency>
   </dependencies>
 </dependencyManagement>
</project>
```

Project B:

```
<project>
  <parent>
    <artifactId>A</artifactId>
    <groupId>maven</groupId>
    <version>1.0</version>
  </parent>
  <modelVersion>4.0.0</modelVersion>
  <groupId>maven</groupId>
  <artifactId>B</artifactId>
  <packaging>pom</packaging>
  <name>B</name>
  <version>1.0</version>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>test</groupId>
        <artifactId>d</artifactId>
        <version>1.0</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
 
  <dependencies>
    <dependency>
      <groupId>test</groupId>
      <artifactId>a</artifactId>
      <version>1.0</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>test</groupId>
      <artifactId>c</artifactId>
      <scope>runtime</scope>
    </dependency>
  </dependencies>
</project>
```

当maven在项目B上运行时，工件a、b、c和d的1.0版本将被使用，而不考虑它们的pom中指定的版本。

a和c都声明为项目的依赖项，因此由于依赖项中介使用1.0版本。两者都有运行时范围，因为它是直接指定的。

b在b的父依赖项管理部分中定义，由于依赖项管理对于传递依赖项优先于依赖项中介，所以如果在a或c的pom中引用1.0版本，则选择1.0版本。b也有编译范围。

最后，由于d是在B的依赖项管理部分中指定的，如果d是a或c的依赖项(或传递依赖项)，那么将选择1.0版本——同样，因为依赖项管理优先于依赖项中介，而且当前pom的声明优先于其父声明。

有关依赖项管理标记的引用信息可从项目描述符引用获得。

### 引入依赖项

上一节中的示例描述了如何通过继承指定托管依赖项。然而，在较大的项目中，这可能是不可能完成的，因为项目只能从单个父级继承。为了适应这一点，项目可以从其他项目导入托管依赖项。这是通过将pom工件声明为具有“import”范围的依赖项来实现的。

Project B:

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>maven</groupId>
  <artifactId>B</artifactId>
  <packaging>pom</packaging>
  <name>B</name>
  <version>1.0</version>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>maven</groupId>
        <artifactId>A</artifactId>
        <version>1.0</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <dependency>
        <groupId>test</groupId>
        <artifactId>d</artifactId>
        <version>1.0</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
 
  <dependencies>
    <dependency>
      <groupId>test</groupId>
      <artifactId>a</artifactId>
      <version>1.0</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>test</groupId>
      <artifactId>c</artifactId>
      <scope>runtime</scope>
    </dependency>
  </dependencies>
</project>
```

假设A是前面示例中定义的pom，那么最终结果将是相同的。除了d之外，A的所有托管依赖项都将被合并到B中，因为d是在这个pom中定义的。

Project X:

```
<project>
 <modelVersion>4.0.0</modelVersion>
 <groupId>maven</groupId>
 <artifactId>X</artifactId>
 <packaging>pom</packaging>
 <name>X</name>
 <version>1.0</version>
 
 <dependencyManagement>
   <dependencies>
     <dependency>
       <groupId>test</groupId>
       <artifactId>a</artifactId>
       <version>1.1</version>
     </dependency>
     <dependency>
       <groupId>test</groupId>
       <artifactId>b</artifactId>
       <version>1.0</version>
       <scope>compile</scope>
     </dependency>
   </dependencies>
 </dependencyManagement>
</project>
```

Project Y:

```
<project>
 <modelVersion>4.0.0</modelVersion>
 <groupId>maven</groupId>
 <artifactId>Y</artifactId>
 <packaging>pom</packaging>
 <name>Y</name>
 <version>1.0</version>
 
 <dependencyManagement>
   <dependencies>
     <dependency>
       <groupId>test</groupId>
       <artifactId>a</artifactId>
       <version>1.2</version>
     </dependency>
     <dependency>
       <groupId>test</groupId>
       <artifactId>c</artifactId>
       <version>1.0</version>
       <scope>compile</scope>
     </dependency>
   </dependencies>
 </dependencyManagement>
</project>
```

Project Z:

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>maven</groupId>
  <artifactId>Z</artifactId>
  <packaging>pom</packaging>
  <name>Z</name>
  <version>1.0</version>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>maven</groupId>
        <artifactId>X</artifactId>
        <version>1.0</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <dependency>
        <groupId>maven</groupId>
        <artifactId>Y</artifactId>
        <version>1.0</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
</project>
```

在上面的例子中，Z从X和Y中导入托管依赖项。然而，X和Y都包含依赖项a。

这个过程是递归的。例如，如果X导入另一个pom Q，当Z被处理时，它将简单地显示Q的所有托管依赖项都在X中定义。

当用于定义相关工件的“库”时，导入是最有效的，这些工件通常是多项目构建的一部分。一个项目使用这些库中的一个或多个构件是相当常见的。然而，有时很难使用构件将项目中的版本与库中分发的版本保持同步。下面的模式说明了如何创建“物料清单”(BOM)供其他项目使用。

项目的根是BOM pom。它定义了将在库中创建的所有构件的版本。希望使用该库的其他项目应该将此pom导入其pom的dependencyManagement部分。

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.test</groupId>
  <artifactId>bom</artifactId>
  <version>1.0.0</version>
  <packaging>pom</packaging>
  <properties>
    <project1Version>1.0.0</project1Version>
    <project2Version>1.0.0</project2Version>
  </properties>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>com.test</groupId>
        <artifactId>project1</artifactId>
        <version>${project1Version}</version>
      </dependency>
      <dependency>
        <groupId>com.test</groupId>
        <artifactId>project2</artifactId>
        <version>${project2Version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
 
  <modules>
    <module>parent</module>
  </modules>
</project>
```

父子项目以BOM pom作为父项目。这是一个普通的多项目pom。

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.test</groupId>
    <version>1.0.0</version>
    <artifactId>bom</artifactId>
  </parent>
 
  <groupId>com.test</groupId>
  <artifactId>parent</artifactId>
  <version>1.0.0</version>
  <packaging>pom</packaging>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.12</version>
      </dependency>
      <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>1.1.1</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
  <modules>
    <module>project1</module>
    <module>project2</module>
  </modules>
</project>
```

接下来是实际的项目poms:

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.test</groupId>
    <version>1.0.0</version>
    <artifactId>parent</artifactId>
  </parent>
  <groupId>com.test</groupId>
  <artifactId>project1</artifactId>
  <version>${project1Version}</version>
  <packaging>jar</packaging>
 
  <dependencies>
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
    </dependency>
  </dependencies>
</project>
 
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.test</groupId>
    <version>1.0.0</version>
    <artifactId>parent</artifactId>
  </parent>
  <groupId>com.test</groupId>
  <artifactId>project2</artifactId>
  <version>${project2Version}</version>
  <packaging>jar</packaging>
 
  <dependencies>
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
    </dependency>
  </dependencies>
</project>
```

下面的项目展示了如何在另一个项目中使用库，而不必指定依赖项目的版本。

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.test</groupId>
  <artifactId>use</artifactId>
  <version>1.0.0</version>
  <packaging>jar</packaging>
 
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>com.test</groupId>
        <artifactId>bom</artifactId>
        <version>1.0.0</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>com.test</groupId>
      <artifactId>project1</artifactId>
    </dependency>
    <dependency>
      <groupId>com.test</groupId>
      <artifactId>project2</artifactId>
    </dependency>
  </dependencies>
</project>
```

### 系统依赖

`重要提示:这是不推荐的。`

与范围系统的依赖关系总是可用的，并且不会在存储库中查找。它们通常用于告诉Maven JDK或VM提供的依赖关系。因此，系统依赖关系对于解决对工件的依赖关系特别有用，这些工件现在由JDK提供，但是在以前可以单独下载。典型的例子是JDBC标准扩展或Java身份验证和授权服务(JAAS)。

一个简单的例子是:

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>javax.sql</groupId>
      <artifactId>jdbc-stdext</artifactId>
      <version>2.0</version>
      <scope>system</scope>
      <systemPath>${java.home}/lib/rt.jar</systemPath>
    </dependency>
  </dependencies>
  ...
</project>
```

如果您的工件是由JDK的`tools.jar`提供的，系统路径定义如下:

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>sun.jdk</groupId>
      <artifactId>tools</artifactId>
      <version>1.5.0</version>
      <scope>system</scope>
      <systemPath>${java.home}/../lib/tools.jar</systemPath>
    </dependency>
  </dependencies>
  ...
</project>
```

## Optional & Exclusion

本节讨论可选的依赖项和依赖项排除。这将帮助用户了解它们是什么、何时以及如何使用它们。它还解释了为什么排除是在每个依赖项的基础上而不是在POM级别进行的。

### 可选依赖关系

当不可能(无论出于什么原因)将项目分割为子模块时，将使用可选依赖项。其思想是，一些依赖关系仅用于项目中的某些特性，如果不使用该特性，就不需要这些依赖关系。理想情况下，这样的特性将被划分为依赖于核心功能项目的子模块。这个新的子项目将只有非可选的依赖项，因为如果您决定使用子项目的功能，就需要所有这些依赖项。

然而，由于项目不能被分割(无论出于什么原因)，这些依赖项声明为可选的。如果用户希望使用与可选依赖项相关的功能，则必须在自己的项目中重新声明该可选依赖项。这不是处理这种情况的最清楚的方法，但是可选依赖项和依赖项排除都是权宜之计。

#### 为什么使用可选依赖项?

可选依赖项节省空间和内存。它们防止有问题的jar(违反许可协议或导致类路径问题)被绑定到WAR、EAR、fat jar或类似的jar中。

#### 如何使用optional标签

通过在依赖项声明中将`<optional>`元素设置为true，可以将依赖项声明为可选:

```
<project>
  ...
  <dependencies>
    <!-- declare the dependency to be set as optional -->
    <dependency>
      <groupId>sample.ProjectA</groupId>
      <artifactId>Project-A</artifactId>
      <version>1.0</version>
      <scope>compile</scope>
      <optional>true</optional> <!-- value will be true or false only -->
    </dependency>
  </dependencies>
</project>
```

#### 可选依赖项如何工作?

```
Project-A -> Project-B
```

上面的图表说明项目a依赖于项目b。当A在其POM中将B声明为可选依赖项时，此关系保持不变。它就像一个普通的构建，其中Project-B将被添加到Project-A的类路径中。

```
Project-X -> Project-A
```

当另一个项目(project - x)在其POM中将project - a声明为依赖项时，依赖项的可选属性将生效。Project-B不包含在Project-X的类路径中。您需要在项目X的POM中直接声明它，以便将B包含在X的类路径中。

#### 例子

假设有一个名为X2的项目，它具有与Hibernate类似的功能。它支持许多数据库，如MySQL、PostgreSQL和Oracle的几个版本。每个受支持的数据库都需要额外依赖于驱动程序jar。所有这些依赖项都需要在编译时构建X2。但是，您的项目只使用一个特定的数据库，其他数据库不需要驱动程序。X2可以将这些依赖项声明为可选的，这样当您的项目在其POM中将X2声明为直接依赖项时，X2支持的所有驱动程序不会自动包含在项目的类路径中。您的项目必须包含对它所使用的数据库的特定驱动程序的显式依赖。

### Dependency Exclusions

由于Maven临时解析依赖项，所以项目的类路径中可能包含不需要的依赖项。例如，某个较老的jar可能存在安全问题，或者与您正在使用的Java版本不兼容。为了解决这个问题，Maven允许您排除特定的依赖项。排除是针对POM中的特定依赖项设置的，并且针对特定的groupId和artifactId。当您构建项目时，该构件将不会通过声明排除的依赖项添加到项目的类路径中。

#### 如何使用dependency exclusions

在包含有问题jar的`<dependency>`元素中添加一个`<exclude>`元素。

```
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>sample.ProjectA</groupId>
      <artifactId>Project-A</artifactId>
      <version>1.0</version>
      <scope>compile</scope>
      <exclusions>
        <exclusion>  <!-- declare the exclusion here -->
          <groupId>sample.ProjectB</groupId>
          <artifactId>Project-B</artifactId>
        </exclusion>
      </exclusions> 
    </dependency>
  </dependencies>
</project>
```

#### 依赖性排除是如何工作的，以及什么时候使用它(作为最后的手段!)

```
Project-A
   -> Project-B
        -> Project-D <! -- This dependency should be excluded -->
              -> Project-E
              -> Project-F
   -> Project C
```

从图中可以看出，Project-A依赖于Project-B, Project-B依赖于Project-D。Project- D依赖于Project- E和F.默认情况下，Project A的类路径包括:

```
B, C, D, E, F
```

假设您不希望将项目D及其依赖项添加到项目A的类路径中，因为存储库中缺少了项目D的一些依赖项，而且您不需要项目b中依赖于项目D的功能。项目b的开发人员可以将依赖关系标记为项目d `<optional>true</optional>`:

```
<dependency>
  <groupId>sample.ProjectD</groupId>
  <artifactId>ProjectD</artifactId>
  <version>1.0-SNAPSHOT</version>
  <optional>true</optional>
</dependency>
```

不幸的是,他们没有。最后，您可以将其排除在您自己的POM中，用于项目a，如下所示:

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>sample.ProjectA</groupId>
  <artifactId>Project-A</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
  ...
  <dependencies>
    <dependency>
      <groupId>sample.ProjectB</groupId>
      <artifactId>Project-B</artifactId>
      <version>1.0-SNAPSHOT</version>
      <exclusions>
        <exclusion>
          <groupId>sample.ProjectD</groupId> <!-- Exclude Project-D from Project-B -->
          <artifactId>Project-D</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
  </dependencies>
</project>
```

如果您将Project-A部署到存储库中，并且Project-X声明了对Project-A的正常依赖关系，Project-D还会被排除在类路径之外吗?

```
Project-X -> Project-A
```

答案是肯定的。Project-A已经声明它不需要Project-D来运行，所以它不会作为Project-A的传递依赖项引入。
现在，考虑项目x依赖于项目y，如下图所示:

```
Project-X -> Project-Y
               -> Project-B
                    -> Project-D
                       ...
```

Project-Y也依赖于Project-B，它确实需要Project-D所支持的特性。因此，它不会在依赖项列表中的Project-D上放置排斥。它还可能提供一个额外的存储库，从这个存储库可以解析Project-E。在这种情况下，重要的是不要在全局中排除Project-D，因为它是Project-Y的合法依赖项。

作为另一个场景，假设您不想要的依赖项是Project-E而不是Project-D。你如何排除它?见下图:

```
Project-A
   -> Project-B
        -> Project-D 
              -> Project-E <!-- Exclude this dependency -->
              -> Project-F
   -> Project C
```

排除作用作用于声明它们的点以下的整个依赖关系图。如果您想排除Project-E而不是Project-D，只需将排除更改为指向Project-E，但不将排除移动到Project-D。您不能更改Project-D的POM。如果可以，您可以使用可选的依赖项而不是排除项，或者将Project-D分割为多个子项目，每个子项目只有正常的依赖项。

```
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>sample.ProjectA</groupId>
  <artifactId>Project-A</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
  ...
  <dependencies>
    <dependency>
      <groupId>sample.ProjectB</groupId>
      <artifactId>Project-B</artifactId>
      <version>1.0-SNAPSHOT</version>
      <exclusions>
        <exclusion>
          <groupId>sample.ProjectE</groupId> <!-- Exclude Project-E from Project-B -->
          <artifactId>Project-E</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
  </dependencies>
</project>
```

#### 为什么要在每个依赖项的基础上而不是在POM级别上进行排除

这主要是为了确保依赖关系图是可预测的，并防止继承影响排除不应该排除的依赖关系。如果您使用了最后一种方法，并且不得不进行排除，那么您应该绝对确定哪些依赖项引入了不需要的传递依赖项。

如果您确实希望确保某个特定依赖项不会出现在类路径中，无论路径是什么，都可以将[禁止依赖项规则](https://maven.apache.org/enforcer/enforcer-rules/bannedDependencies.html)配置为在发现有问题的依赖项时构建失败。当构建失败时，您需要在强制程序找到的每个路径上添加特定的排除。

## 如何在远程存储库中部署jar ?

要将jar部署到外部存储库，您必须在pom.xml中配置存储库url，并在settings.xml中配置连接到存储库的身份验证信息。

下面是一个使用scp和用户名/密码身份验证的例子:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
 
  <name>Maven Quick Start Archetype</name>
  <url>http://maven.apache.org</url>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.codehaus.plexus</groupId>
      <artifactId>plexus-utils</artifactId>
      <version>1.0.4</version>
    </dependency>
  </dependencies>
 
  <build>
    <filters>
      <filter>src/main/filters/filters.properties</filter>
    </filters>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>
  <!--
   |
   |
   |
   -->
  <distributionManagement>
    <repository>
      <id>mycompany-repository</id>
      <name>MyCompany Repository</name>
      <url>scp://repository.mycompany.com/repository/maven2</url>
    </repository>
  </distributionManagement>
</project>
```

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
  ...
  <servers>
    <server>
      <id>mycompany-repository</id>
      <username>jvanzyl</username>
      <!-- Default value is ~/.ssh/id_dsa -->
      <privateKey>/path/to/identity</privateKey> (default is ~/.ssh/id_dsa)
      <passphrase>my_key_passphrase</passphrase>
    </server>
  </servers>
  ...
</settings>
```

注意,如果您正在连接到一个openssh ssh服务器的参数“PasswordAuthentication”设置sshd_confing为"no",你必须输入你的密码每次用户名/密码身份验证(尽管您可以使用另一个ssh客户机登录输入用户名和密码)。在本例中，您可能希望切换到公钥身份验证。
如果在settings.xml中使用密码，应该小心。有关更多信息，请参见密码加密。（https://maven.apache.org/guides/mini/guide-encryption.html）

## 如何创建文档?

要开始使用Maven的文档系统，可以使用原型机制使用以下命令为现有项目生成站点:

```
mvn archetype:generate \
  -DarchetypeGroupId=org.apache.maven.archetypes \
  -DarchetypeArtifactId=maven-archetype-site \
  -DgroupId=com.mycompany.app \
  -DartifactId=my-app-site
```

## 我如何构建其他类型的项目?

注意，生命周期适用于任何项目类型。例如，回到基本目录，我们可以创建一个简单的web应用程序:

```
mvn archetype:generate \
    -DarchetypeGroupId=org.apache.maven.archetypes \
    -DarchetypeArtifactId=maven-archetype-webapp \
    -DgroupId=com.mycompany.app \
    -DartifactId=my-webapp
```

注意，这些必须都在一行上。这将创建一个名为my-webapp的目录，其中包含以下项目描述符:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-webapp</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
 
  <build>
    <finalName>my-webapp</finalName>
  </build>
</project>
```

注意`<packaging>`元素——这告诉Maven将构建为一个WAR。切换到webapp项目的目录，并尝试:

```
mvn package
```

你会看到`target/my-webapp.war`被构建了，所有正常的步骤都被执行了。

## 如何同时构建多个项目?

Maven内置了处理多个模块的概念。在本节中，我们将展示如何构建上面的WAR，并在一个步骤中包含前面的JAR。
首先，我们需要在前面两个目录中添加一个父pom.xml文件，所以它应该是这样的:

```
+- pom.xml
+- my-app
| +- pom.xml
| +- src
|   +- main
|     +- java
+- my-webapp
| +- pom.xml
| +- src
|   +- main
|     +- webapp
```

您将创建的POM文件应该包含以下内容:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>
 
  <modules>
    <module>my-app</module>
    <module>my-webapp</module>
  </modules>
</project>
```

我们需要一个从webapp依赖于JAR，所以添加到`my-webapp/pom.xml`:

```
  ...
  <dependencies>
    <dependency>
      <groupId>com.mycompany.app</groupId>
      <artifactId>my-app</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
    ...
  </dependencies>
```

最后，将以下`<parent>`元素添加到子目录中的其他pom.xml文件中:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <parent>
    <groupId>com.mycompany.app</groupId>
    <artifactId>app</artifactId>
    <version>1.0-SNAPSHOT</version>
  </parent>
  ...
```

现在,试试…从顶层目录运行:

```
mvn verify
```

WAR现在已经在`my-webapp/target/my-webapp.war`中创建。JAR包括:

```
$ jar tvf my-webapp/target/my-webapp-1.0-SNAPSHOT.war
   0 Fri Jun 24 10:59:56 EST 2005 META-INF/
 222 Fri Jun 24 10:59:54 EST 2005 META-INF/MANIFEST.MF
   0 Fri Jun 24 10:59:56 EST 2005 META-INF/maven/
   0 Fri Jun 24 10:59:56 EST 2005 META-INF/maven/com.mycompany.app/
   0 Fri Jun 24 10:59:56 EST 2005 META-INF/maven/com.mycompany.app/my-webapp/
3239 Fri Jun 24 10:59:56 EST 2005 META-INF/maven/com.mycompany.app/my-webapp/pom.xml
   0 Fri Jun 24 10:59:56 EST 2005 WEB-INF/
 215 Fri Jun 24 10:59:56 EST 2005 WEB-INF/web.xml
 123 Fri Jun 24 10:59:56 EST 2005 META-INF/maven/com.mycompany.app/my-webapp/pom.properties
  52 Fri Jun 24 10:59:56 EST 2005 index.jsp
   0 Fri Jun 24 10:59:56 EST 2005 WEB-INF/lib/
2713 Fri Jun 24 10:59:56 EST 2005 WEB-INF/lib/my-app-1.0-SNAPSHOT.jar
```

这是怎么回事?首先，创建的父POM(称为`app`)有一个`pom`打包和定义的模块列表。这告诉Maven在一组项目上运行所有操作，而不是只运行当前的一个(要覆盖此行为，可以使用`--non-recursive`命令行选项)。

接下来，我们告诉WAR它需要`my-app` JAR。这做了一些事情:它使WAR中的任何代码在类路径上都可用(在本例中没有)，它确保JAR总是在WAR之前构建的，并指示WAR插件将JAR包含在其库目录中。

您可能已经注意到`junit-4.11.jar`是一个依赖项，但最终没有进入WAR。原因是`<scope>test</scope>`元素-它只用于测试，因此不像编译时依赖my-app那样包含在web应用程序中。

最后一步是包含父定义。这与Maven 1.0中您可能熟悉的扩展元素不同:这确保了即使项目是通过在存储库中查找而与父项目单独分布的，也始终能够找到POM。

# 构建生命周期介绍

## 构建生命周期基础知识

Maven基于构建生命周期的核心概念。这意味着构建和分发特定工件(项目)的过程被清晰地定义了。

对于构建项目的人员来说，这意味着只需要学习一小组命令就可以构建任何Maven项目，POM将确保他们得到他们想要的结果。

有三个内置的构建生命周期:default、clean和site。`default`生命周期处理项目部署，`clean`生命周期处理项目清理，而`site`生命周期处理项目站点文档的创建。

## 构建生命周期阶段组成

每个构建生命周期都由不同的构建阶段列表定义，其中一个构建阶段表示生命周期中的一个阶段。
例如，默认的生命周期包括以下几个阶段(完整的生命周期阶段列表，请参考生命周期参考):

- `validate` -验证项目是正确的，并且所有必要的信息都是可用的
- `compile` - 编译项目的源代码
- `test` - 使用合适的单元测试框架测试编译后的源代码。这些测试不应该要求打包或部署代码
- `package` - 将编译后的代码以其可分发格式打包，例如JAR。
- `verify` - 对集成测试的结果进行任何检查，以确保满足质量标准
- `install` - 将包安装到本地存储库中，以便在本地的其他项目中作为依赖项使用
- `deploy` - 在构建环境中完成后，将最终的包复制到远程存储库，以便与其他开发人员和项目共享。

这些生命周期阶段(加上这里没有显示的其他生命周期阶段)按顺序执行，以完成默认的生命周期。鉴于上面的生命周期阶段,这意味着当默认使用生命周期,Maven将首先验证项目,然后将试图编译源代码,运行这些测试,包二进制文件(如jar),运行集成测试方案,验证了集成测试,验证包安装到本地存储库,然后将安装包部署到远程存储库。

### 常用命令行调用

在开发环境中，使用以下调用将构件构建并安装到本地存储库中。

```
mvn install
```

在执行安装之前，此命令按顺序执行每个默认的生命周期阶段(`validate`, `compile`, `package`等)。您只需要调用要执行的最后一个构建阶段，在这种情况下，`instal`l:
在构建环境中，使用以下调用干净地构建并将构件部署到共享存储库中。

```
mvn clean deploy
```

同一个命令可以在多模块场景中使用(例如，具有一个或多个子项目的项目)。Maven遍历每个子项目并执行clean，然后执行deploy(包括所有先前的构建阶段步骤)。

### 构建阶段由插件目标组成

然而，即使构建阶段负责构建生命周期中的特定步骤，它执行这些职责的方式也可能不同。这是通过声明绑定到那些构建阶段的插件目标来实现的。

插件目标表示一个特定的任务(比构建阶段更精细)，它有助于构建和管理项目。它可能被绑定到零个或多个构建阶段。不绑定到任何构建阶段的目标可以通过直接调用在构建生命周期之外执行。执行的顺序取决于调用目标和构建阶段的顺序。例如，考虑下面的命令。clean和package参数是构建阶段，而`dependency:copy-dependencies`是(插件的)目标。

```
mvn clean dependency:copy-dependencies package
```

如果这是执行,`clean` 阶段将首先执行(这意味着它将运行所有干净的前阶段生命周期,加上`clean`阶段本身),然后以`dependency:copy-dependencies`为目标,最后执行方案阶段(及其构建阶段之前的缺省生命周期)。

此外，如果一个目标被绑定到一个或多个构建阶段，那么该目标将在所有这些阶段中被调用。
此外，构建阶段还可以有零个或多个目标。如果构建阶段没有绑定目标，那么该构建阶段将不会执行。但如果它有一个或多个目标，它将执行所有这些目标。

## 设置您的项目以使用构建生命周期

构建生命周期非常简单，可以使用，但是当您为项目构建Maven构建时，如何为每个构建阶段分配任务呢?

### Packaging

第一种也是最常见的方法是通过同样命名的POM元素`<packaging>`设置项目的打包。一些有效的打包值是jar、war、ear和pom。如果没有指定打包值，则默认为jar。

每个包都包含一个要绑定到特定阶段的目标列表。例如，jar打包将绑定以下目标来构建默认生命周期的各个阶段。

| Phase                    | plugin:goal               |
| :----------------------- | :------------------------ |
| `process-resources`      | `resources:resources`     |
| `compile`                | `compiler:compile`        |
| `process-test-resources` | `resources:testResources` |
| `test-compile`           | `compiler:testCompile`    |
| `test`                   | `surefire:test`           |
| `package`                | `jar:jar`                 |
| `install`                | `install:install`         |
| `deploy`                 | `deploy:deploy`           |

这几乎是一组标准的绑定;然而，有些包装对它们的处理不同。例如，一个纯元数据的项目(打包值是pom)只将目标绑定到安装和部署阶段(对于一些打包类型的目标到构建阶段的完整列表，请参考生命周期引用)。

注意，对于某些可用的打包类型，您可能还需要在POM的`<build>`部分中包含一个特定的插件，并为该插件指定`<extensions>true</extensions>`。丛应用程序和丛服务打包提供了丛应用程序和丛服务打包。

# POM文件介绍

## 什么是POM

项目对象模型或POM是Maven中的基本工作单元。它是一个XML文件，包含Maven用于构建项目的有关项目和配置细节的信息。它包含大多数项目的默认值。例如build目录，它是目标;源目录，即src/main/java;测试源目录，即src/test/java;等等。在执行任务或目标时，Maven在当前目录中查找POM。它读取POM，获取所需的配置信息，然后执行目标。

POM中可以指定的一些配置包括项目依赖项、可以执行的插件或目标、构建概要文件等等。还可以指定项目版本、描述、开发人员、邮件列表等其他信息。

## Super POM

Super POM是Maven的默认POM。除非显式设置，否则所有POMs都会继承Super POM，这意味着在Super POM中指定的配置将由为项目创建的POMs继承。下面的代码片段是Maven 3.5.4的Super POM。

```
<project>
  <modelVersion>4.0.0</modelVersion>
 
  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>
 
  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>
 
  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${project.basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
      <resource>
        <directory>${project.basedir}/src/main/resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>${project.basedir}/src/test/resources</directory>
      </testResource>
    </testResources>
    <pluginManagement>
      <!-- NOTE: These plugins will be removed from future versions of the super POM -->
      <!-- They are kept for the moment as they are very unlikely to conflict with lifecycle mappings (MNG-4453) -->
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.5.3</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
 
  <reporting>
    <outputDirectory>${project.build.directory}/site</outputDirectory>
  </reporting>
 
  <profiles>
    <!-- NOTE: The release profile will be removed from future versions of the super POM -->
    <profile>
      <id>release-profile</id>
 
      <activation>
        <property>
          <name>performRelease</name>
          <value>true</value>
        </property>
      </activation>
 
      <build>
        <plugins>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar-no-fork</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-javadoc-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-javadocs</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-deploy-plugin</artifactId>
            <configuration>
              <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>
 
</project>
```

