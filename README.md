# Java 单元测试 JUnit 5 快速上手
## 前言

虽然单元测试是软件开发中必不可少的一环，但是在平常开发中往往因为项目周期紧，工作量大而被选择忽略，但这样往往导致软件问题层出不同。线上出现的不少问题其实在有单元测试的情况下就可以及时发现和处理，因此培养自己在日常开发中写单元测试的能力是很有必要的。无论是对自己的编码能力的提高，还是项目质量的提升，都是大有好处，本文将介绍 Java 单元测试框架 JUnit 5 的基础认识和使用来编写单元测试，希望同样对你有所帮助。

> 本文所涉及所有代码片段均在下面仓库中，感兴趣的小伙伴欢迎参考学习：
>
> https://github.com/wrcj12138aaa/junit5-actions
>
> 版本支持：
>
> - JDK 8
> - JUnit 5.5.2
> - Lomok 1.18.8

## 什么是 JUnit 5

要说什么是 JUnit 5，首先就得聊下 Java 单元测试框架 JUnit，它与另一个框架 TestNG  占据了 Java领域里单元测试框架的主要市场，其中 JUnit 以着较长的发展历史和不断演进的丰富功能，备受大多数 Java 开发者的青睐。

而说到 JUnit 的历史，JUnit 起源于 1997年，最初版本在两位编程大师的 Kent Beck 和 Erich Gamma 的一次飞机之旅上完成，由于当时 Java 测试过程中缺乏成熟工具，两人在飞机上就合作设计实现了 JUnit 雏形，旨在成为更好用的测试框架。如今二十多年过去了，JUnit 经过各个版本迭代演进，已经发展到了 5.x 版本，为 JDK 8以及更高的版本上提供更好的支持 (如支持 Lambda ) 和更丰富的测试形式 (如重复测试，参数化测试)。

了解过 JUint 之后，再回头来看下 JUnit 5，这个版本可以说是 JUnit 单元测试框架的一次重大升级，首先需要 Java 8 以上的运行环境，虽然在旧版本 JDK 也能编译运行，但要完全使用 JUnit 5 功能， JDK 8 环境是必不可少的。

除此之外，JUnit 5 与以前版本的 JUnit 不同，拆分成由三个不同子项目的几个不同模块组成。

> **JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

- JUnit Platform： 用于JVM上启动测试框架的基础服务，提供命令行，IDE和构建工具等方式执行测试的支持。

- JUnit Jupiter：包含 JUnit 5 新的编程模型和扩展模型，主要就是用于编写测试代码和扩展代码。

- JUnit Vintage：用于在JUnit 5 中兼容运行 JUnit3.x 和 JUnit4.x 的测试用例。

基于上面的介绍，可以参考下图对 JUnit 5 的架构和模块有所了解：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70d3n7k2qj30j40rewg5.jpg)

## 为什么需要 JUnit 5

说完 JUnit 5 是什么之后，我们再来想一个问题：为什么需要一个 JUnit 5 呢？

自从有了类似 JUnit 之类的测试框架，Java 单元测试领域逐渐成熟，开发人员对单元测试框架也有了更高的要求：更多的测试方式，更少的其他库的依赖。因此，大家期待着一个更强大的测试框架诞生，JUnit 作为Java测试领域的领头羊，推出了 JUnit 5 这个版本，主要特性：

- 提供全新的断言和测试注解，支持测试类内嵌
- 更丰富的测试方式：支持动态测试，重复测试，参数化测试等
- 实现了模块化，让测试执行和测试发现等不同模块解耦，减少依赖

- 提供对 Java 8 的支持，如 Lambda 表达式，Sream API等。

## JUnit 5 常见用法介绍

接下来，我们看下 JUni 5 的一些常见用法，来帮助我们快速掌握 JUnit 5 的使用。

首先，在 Maven 工程里引入 JUnit 5 的依赖坐标，需要注意的是当前JDK 环境得在 Java 8 以上。

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-engine</artifactId>
  <version>5.5.2</version>
  <scope>test</scope>
</dependency>
```

### 第一个测试用例

引入JUnit 5，我们可以先快速编写一个简单的测试用例，从这个测试用例来认识初步下 JUnit 5：

```java
@DisplayName("我的第一个测试用例")
public class MyFirstTestCaseTest {

    @BeforeAll
    public static void init() {
        System.out.println("初始化数据");
    }

    @AfterAll
    public static void cleanup() {
        System.out.println("清理数据");
    }

    @BeforeEach
    public void tearup() {
        System.out.println("当前测试方法开始");
    }

    @AfterEach
    public void tearDown() {
        System.out.println("当前测试方法结束");
    }

    @DisplayName("我的第一个测试")
    @Test
    void testFirstTest() {
        System.out.println("我的第一个测试开始测试");
    }

    @DisplayName("我的第二个测试")
    @Test
    void testSecondTest() {
        System.out.println("我的第二个测试开始测试");
    }
}

```

直接运行这个测试用例，可以看到控制台日志如下：![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70emiq1ccj313i0nk0vh.jpg)

可以看到左边一栏的结果里显示测试项名称就是我们在测试类和方法上使用 **@DisplayName** 设置的名称，这个注解就是 JUnit 5 引入，用来定义一个测试类并指定用例在测试报告中的展示名称，这个注解可以使用在类上和方法上，在类上使用它就表示该类为测试类，在方法上使用则表示该方法为测试方法。

```java
@Target({ ElementType.TYPE, ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@API(status = STABLE, since = "5.0")
public @interface DisplayName {
	String value();
}
```

再来看下示例代码中使用到的一对注解 **@BeforeAll **和 **@AfterAll **，它们定义了整个测试类在开始前以及结束时的操作，只能修饰静态方法，主要用于在测试过程中所需要的全局数据和外部资源的初始化和清理。与它们不同，**@BeforeEach** 和 **@AfterEach** 所标注的方法会在每个测试用例方法开始前和结束时执行，主要是负责该测试用例所需要的运行环境的准备和销毁。

在测试过程中除了这些基本的注解，还有更多丰富强大的注解，接下来就我们一一学习下吧。

### 禁用执行测试：@Disabled

当我们希望在运行测试类时，跳过某个测试方法，正常运行其他测试用例时，我们就可以用上 @Disabled 注解，表明该测试方法处于不可用，执行测试类的测试方法时不会被 JUnit 执行。

下面看下使用 @Disbaled 之后的运行效果，在原来测试类中添加如下代码：

```java
@DisplayName("我的第三个测试")
@Disabled
@Test
void testThirdTest() {
	System.out.println("我的第三个测试开始测试");
}
```

运行后看到控制台日志如下，当用 @Disabled 标记的方法不会执行，只有单独的方法信息打印：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70g48ar0kj314u0p4gpk.jpg)

@Disabled 也可以使用在类上，用于标记类下所有的测试方法不被执行，一般使用对多个测试类组合测试的时候。

### 内嵌测试类：@Nested

当我们编写的类和代码越来越来，随之而来的需要测试的对应测试类也会越来越多。为了解决测试类数量爆炸的问题，JUnit 5提供了@Nested 注解，能够以静态内部成员类的形式对测试用例类进行逻辑分组。 并且每个静态内部类都可以有自己的生命周期方法， 这些方法将按从外到内层次顺序执行。 此外，嵌套的类也可以用@DisplayName 标记，这样我们就可以使用正确的测试名称。下面看下简单的用法：

```java
@DisplayName("内嵌测试类")
public class NestUnitTest {
    @BeforeEach
    void init() {
        System.out.println("测试方法执行前准备");
    }

    @Nested
    @DisplayName("第一个内嵌测试类")
    class FirstNestTest {
        @Test
        void test() {
            System.out.println("第一个内嵌测试类执行测试");
        }
    }

    @Nested
    @DisplayName("第二个内嵌测试类")
    class SecondNestTest {
        @Test
        void test() {
            System.out.println("第二个内嵌测试类执行测试");
        }
    }
}
```

运行所有测试用例后，在控制台能看到如下结果：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70ivdyv2oj313m0ca75e.jpg)

### 重复性测试：@RepeatedTest

在 JUnit 5 里新增了对测试方法指定运行测试的支持，允许对测试方法进行自动重复运行。当要运行一个测试方法 N次时，可以使用 @RepeatedTest 标记它，如下面的代码所示：

```java
@DisplayName("重复测试")
@RepeatedTest(value = 3)
public void i_am_a_repeated_test() {
	System.out.println("执行测试");
}
```

运行后测试方法会执行3次，在 IDEA 的运行效果如下图所示：

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70hvnssnmj315m0ak0u0.jpg)

这是基本的用法，我们还可以对重复运行的测试方法名称进行修改，利用 @RepeatedTest  提供的内置变量，以占位符方式在其 `name` 属性上使用，下面先看下使用方式和效果：

```
@DisplayName("自定义名称重复测试")
@RepeatedTest(value = 3, name = "{displayName} 第 {currentRepetition} 次")
public void i_am_a_repeated_test_2() {
System.out.println("执行测试");
}
```

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70hzmlb9gj30ks086t9z.jpg)

@RepeatedTest 注解内用 currentRepetition 表示已经重复的次数，totalRepetitions 表示总共要重复的次数，displayName 表示测试方法显示名称，我们直接就可以使用这些内置的变量来重新定义测试方法重复运行时的名称。

### 新的断言

在断言 API 设计上，JUnit 5 进行显著地改进，并且充分利用Java 8的新特性，特别是lambda表达式，最终提供了新的断言类: **org.junit.jupiter.api.Assertions** 。许多断言方法接受Lambda 表达式参数，在断言消息使用 Lambda 表达式的一个优点就是它是延迟计算的，如果消息构造开销很大，这样做一定程度上可以节省时间和资源。

现在还可以将一个方法内的多个断言进行分组，使用 assertAll 方法如下示例代码：

```java
@Test
void testGroupAssertions() {
    int[] numbers = {0, 1, 2, 3, 4};
    Assertions.assertAll("numbers",
            () -> Assertions.assertEquals(numbers[1], 1),
            () -> Assertions.assertEquals(numbers[3], 3),
            () -> Assertions.assertEquals(numbers[4], 4)
    );
}
```

如果分组断言中任一个断言的失败，都会将以 MultipleFailuresError 方式进行抛出提示。

### 超时操作的测试：assertTimeoutPreemptively

当我们希望测试长时间执行的方法的时间，并不想让测试方法无限地等待时，就可以对测试方法进行超时测试，JUnit 5 对此推出了断言方法 `assertTimeout`，提供了对超时的广泛支持。

假设我们希望测试代码在一秒内执行完毕，可以写如下测试用例：

```java
@Test
@DisplayName("超时方法测试")
void test_should_complete_in_one_second() {
  Assertions.assertTimeoutPreemptively(Duration.of(1, ChronoUnit.SECONDS), () -> Thread.sleep(2000));
}
```

这个测试运行失败，因为代码执行将休眠两秒钟，而我们期望测试用例在一秒钟之内成功。但是如果我们把休眠时间设置一秒钟，测试仍然会出现偶尔失败的情况，这是因为测试方法执行过程中除了目标代码还有额外的代码和指令执行会耗时，所以在超时限制上无法做到对时间参数设置过于精确。

### 异常测试：assertThrows

我们代码中对于带有异常的方法通常都是使用 try-catch 方式捕获处理，针对测试这样带有异常抛出的代码，而 JUnit 5 提供方法 `Assertions#assertThrows(Class<T>, Executable)` 来进行测试，第一个参数为异常类型，第二个为函数式接口参数，跟 Runnable 接口相似，不需要参数，也没有返回，并且支持 Lambda表达式方式使用，具体使用方式可参考下方代码：

```java
@Test
@DisplayName("测试捕获的异常")
void assertThrowsException() {
  String str = null;
  Assertions.assertThrows(IllegalArgumentException.class, () -> {
    Integer.valueOf(str);
  });
}
```

当Lambda表达式中代码出现的异常会跟首个参数的异常类型进行比较，如果不属于同一类异常，就会控制台输出如下类似的提示：`org.opentest4j.AssertionFailedError: Unexpected exception type thrown ==> expected: <IllegalArgumentException> but was: <...Exception>`

## JUnit 5 参数化测试

使用 JUnit 5 进行参数化测试，除了 junit-jupiter-engine 基础依赖之外，还需要另个模块依赖：junit-jupiter-params，其主要就是提供了编写参数化测试 API。同样方式，将相同版本的依赖引入 Maven 工程中：

```xml
<dependency>
  <groupId>org.junit.jupiter</groupId>
  <artifactId>junit-jupiter-params</artifactId>
  <version>5.5.2</version>
  <scope>test</scope>
</dependency>
```

### 基本数据源测试： @ValueSource

@ValueSource 是 JUnit 5 提供的最简单的数据参数源，支持 Java 的八大基本类型和字符串，Class，使用时赋值给注解上对应类型属性，以数组方式传递，示例代码如下：

```java
public class ParameterizedUnitTest {
    @ParameterizedTest
    @ValueSource(ints = {2, 4, 8})
    void testNumberShouldBeEven(int num) {
        Assertions.assertEquals(0, num % 2);
    }

    @ParameterizedTest
    @ValueSource(strings = {"Effective Java", "Code Complete", "Clean Code"})
    void testPrintTitle(String title) {
        System.out.println(title);
    }
}
```

> @ParameterizedTest 作为参数化测试的必要注解，替代了 @Test 注解。任何一个参数化测试方法都需要标记上该注解。

运行测试，结果如下图所示，针对 @ValueSource 里每个参数都会运行目标方法，一旦哪个参数运行测试失败，就意味着该测试方法不通过。

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70k7mpv10j316i0e4dh8.jpg)

## CSV 数据源测试：@CsvSource

通过 @CsvSource 可以注入指定 CSV 格式 (comma-separated-values)   的一组数据，用每个逗号分隔的值来匹配一个测试方法对应的参数，下面是使用示例：

```java
@ParameterizedTest
@CsvSource({"1,One", "2,Two", "3,Three"})
void testDataFromCsv(long id, String name) {
	System.out.printf("id: %d, name: %s", id, name);
}
```

运行结果如图所示，除了用逗号分隔参数外，@CsvSource 还支持自定义符号，只要修改它的 `delimiter` 即可，默认为 `，`。

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70kj1q8g5j30w60e0myu.jpg)

JUnit 还提供了读取外部 CSV 格式文件数据的方式作为数据源的实现，我们只要用 @CsvFileSource 指定资源文件路径即可，使用起来跟 @CsvSource 一样简单这里就不再重复演示了。

> @CsvFileSource 指定资源文件路径时要以 `/` 开始，寻找当前测试资源目录下文件。

除了上面提到的三种数据源方式外，JUnit 还提供了以下三种数据源：

- **@EnumSource**：允许我们通过参数值，给指定 Enum 枚举类型传入，构造出枚举类型中特定的值。
- **@MethodSource**：指定一个返回的Stream / Array / 可迭代值的方法作为数据源。 需要注意的是该方法必须是静态的，并且不能接受任何参数。
- **@ArgumentSource**：通过实现 ArgumentsProvider 接口的参数类来作为数据源，重写它的 `provideArguments` 方法可以返回自定义类型的 Stream\<Arguments\>  ，作为测试方法所需要的数据使用。

对上面三种数据源注解感兴趣的同学可以参考示例工程的 ParameterizedUnitTest 类，这里就不一一再介绍了。

## 结语

到这里，想必你对 JUnit 5 有了基本的了解和学会如何去用，都说单元测试是提升软件质量，提升研发效率的必备环节，从会用 JUnit 5 写单元测试，培养写测试代码的习惯，对自身的开发效率有所帮助。

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g70n5fwrawj30p00dwah3.jpg)

## 推荐阅读

- [一文掌握 Spring Boot Profiles](https://mp.weixin.qq.com/s/TfA8rSwYNpW3YV_N8oLJnw)
- [如何优雅关闭 Spring Boot 应用](https://mp.weixin.qq.com/s/-t2hrrVMBpPmVEzDcC8J5w)
- [需要接口管理的你了解一下？](https://mp.weixin.qq.com/s/uWRnRhH4et-XSD101Xh6LQ)
- [Java 之 Lombok 必知必会](https://mp.weixin.qq.com/s/2qkNz4VPgnixXjaVYUkvvQ)
- [Java 微服务新生代之 Nacos](https://mp.weixin.qq.com/s/vS36glyNoD26GL6cbNs5Qw)

## 参考资料

- https://junit.org/junit5/docs/current/user-guide/#overview-getting-started-junit-artifacts

- https://www.baeldung.com/junit-5

- https://zhuanlan.zhihu.com/p/43902194
- https://dev.to/stealthmusic/why-you-should-start-using-junit-5-5gk2
- 《Java Unit Testing with JUnit 5》
