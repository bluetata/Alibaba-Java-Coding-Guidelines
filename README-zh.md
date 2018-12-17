## Table of Contents

* [前言](#前言)
* [1. Programming Specification](#1-programming-specification)
   * [Naming Conventions](#naming-conventions)
   * [Constant Conventions](#constant-conventions)
   * [Formatting Style](#formatting-style)
   * [OOP Rules](#oop-rules)
   * [Collection](#collection)
   * [Concurrency](#concurrency)
   * [Flow Control Statements](#flow-control-statements)
   * [Code Comments](#code-comments)
   * [Other](#other)
* [2. Exception and Logs](#2-exception-and-logs)
   * [Exception](#exception)
   * [Logs](#logs)
* [3. MySQL Rules](#3-mysql-rules)
   * [Table Schema Rules](#table-schema-rules)
   * [Index Rules](#index-rules)
   * [SQL Rules](#sql-rules)
   * [ORM Rules](#orm-rules)
* [4. Project Specification](#4-project-specification)
   * [Application Layers](#application-layers)
   * [Library Specification](#library-specification)
   * [Server Specification](#server-specification)
* [5. Security Specification](#5-security-specification)

## 前言

我们很高兴向您介绍 **阿里巴巴Java开发手册**，它整合了阿里巴巴集团技术团队的集体智慧结晶
和经验总结。随着我们鼓励重用和更好地理解彼此的程序，大量Java编程团队对跨项目的代码质量提
出了要求。我们在过去看到过许多的编程问题。例如，数据库的表结构和索引设计缺陷可能导致软件
的架构缺陷或性能风险；工程结构混乱导致后续维护艰难；没有鉴权的漏洞代码易被黑客攻击等等。
为了解决这些问题，我们为阿里巴巴的Java开发人员提供了该文档。

本文档共包含5个部分：**编程规约**、**异常日志**、**MySQL 数据库**、**工程结构** 和
**安全规约**。根据约束力强弱及故障敏感性，每个规约依次分为 **强制**、**推荐**、**参考**
三大类。对于规约条目的延伸信息：  
 (1) **说明**：对内容做了适当扩展和解释；  
 (2) **正例**：提倡什么样的编码和实现方式；  
 (3) **反例**：说明需要提防的雷区，以及真实的错误案例。

本手册的愿景是 **码出高效**，**码出质量**。现代软件架构都需要协同开发完成，高效协作即降
低协同成本， 提升沟通效率， 所谓无规矩不成方圆，无规范不能协作。众所周知，制订交通法规表
面上是要限制行车权，实际上是保障公众的人身安全。试想如果没有限速，没有红绿灯，谁还敢上路
行驶。对软件来说，适当的规范和标准绝不是消灭代码内容的创造性、优雅性，而是限制过度个性化，
以一种普遍认可的统一方式一起做事，提升协作效率。 代码的字里行间流淌的是软件生命中的血液，
质量的提升是尽可能少踩坑，杜绝踩重复的坑，切实提升质量意识（摘自码出高效阿里巴巴Java开发手册1.3.1.pdf）。

我们将持续收集社区反馈，完善阿里巴巴Java开发手册。

## <font color="green">1. 编程规约</font>
### <font color="green">命名规约</font>
1\. **【强制】** 代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束。   
> <font color="#FF4500">反例：</font> \_name / \_\_name / \$Object / name_ / name\$ / Object\$

2\. **【强制】** 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。<font color="#977C00">说明</font>：正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式也要避免采用。   
> <font color="#019858">正例：</font>alibaba / taobao / youku / hangzhou 等国际通用的名称， 可视同英文。  
> <font color="#FF4500">反例：</font>DaZhePromotion [打折] / getPingfenByName() [评分] / int 某变量 = 3  

3\. **【强制】** 类名使用UpperCamelCase风格，必须遵从驼峰形式，但以下情形例外：（领域模型的相关命名）DO / BO / DTO / VO等。   
 > <font color="#019858">正例：</font>MarcoPolo  /  UserDO  /  HtmlDTO  /  XmlService  /  TcpUdpDeal / TaPromotion  
 > <font color="#FF4500">反例：</font>marcoPolo  /  UserDo  /  HTMLDto  /  XMLService  /  TCPUDPDeal / TAPromotion  

4\. **【强制】** 方法名、参数名、成员变量、局部变量都统一使用lowerCamelCase风格，必须遵从驼峰形式。   
> <font color="#019858">正例：</font> localValue / getHttpMessage()  /  inputUserId    


5\. **【强制】** 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。   
> <font color="#019858">正例：</font> MAX\_STOCK\_COUNT   
> <font color="#FF4500">反例：</font> MAX\_COUNT   

6\. **【强制】** 抽象类命名使用 *Abstract* 或 *Base* 开头； 异常类命名使用 *Exception* 结尾； 测试类
命名以它要测试的类名开始，以 *Test* 结尾。

7\. **【强制】** 中括号是数组类型的一部分，数组定义如下：*<font color="blue">String[]</font> args;*（base on: 码出高效阿里巴巴Java开发手册1.3.1.pdf）  
> <font color="#019858">正例：</font>定义整形数组 *int[] arrayDemo;*   
> <font color="#FF4500">反例：</font>在 main 参数中，使用 *String args[]* 来定义。   

8\. **【强制】** POJO 类中布尔类型的变量，都不要加 is 前缀，否则部分框架解析会引起序列化错误。  
> <font color="#FF4500">反例：</font>定义为基本数据类型 *Boolean isDeleted;* 的属性，它的方法也是 `isDeleted()`，RPC框架在反向解析的时候， “误以为” 对应的属性名称是 deleted，导致属性获取不到，进而抛出异常。

9\. **【强制】** 包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。  
> <font color="#019858">正例：</font> 应用工具类包名为 `com.alibaba.open.util`、类名为 `MessageUtils`（此规则参考spring的框架结构）

10\. **【强制】** 杜绝完全不规范的缩写， 避免望文不知义。     
> <font color="#FF4500">反例：</font>AbstractClass “缩写” 命名成 AbsClass； condition “缩写” 命名成 condi，此类随
意缩写严重降低了代码的可阅读性。

11\. **【推荐】** 如果使用任何设计模式，建议将模式名称包含在类名称中。  
> <font color="#019858">正例：</font>`public class OrderFactory;` `public class LoginProxy;` `public class ResourceObserver;`  
> <font color="#977C00">注意：</font>将设计模式体现在名字中，有利于阅读者快速理解架构设计理念。

12\. **【推荐】** 接口类中的方法和属性不要加任何修饰符号（public 也不要加） ，保持代码的简洁性，并加上有效的 Javadoc 注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是与接口方法相关，并且是整个应用的基础常量。  
> <font color="#019858">正例：</font>接口方法签名 `void f(); `  
接口基础常量 `String COMPANY = "alibaba";`    
> <font color="#977C00">注意：</font>JDK8 中接口允许有默认实现，那么这个 default 方法，是对所有实现类都有价值的默认实现。

13\. 接口和实现类的命名有两套规则：  
&emsp;&emsp;1) **【强制】** 对于 *Service* 和 *DAO* 类，基于 SOA 的理念，暴露出来的服务一定是接口，内部的实现类用 *Impl* 的后缀与接口区别。  
> <font color="#019858">正例：</font>`CacheServiceImpl` 实现 `CacheService` 接口。  

&emsp;&emsp;2) **【推荐】** 如果是形容能力的接口名称，取对应的形容词为接口名（通常是–able 的形式） 。  
> <font color="#019858">正例：</font>`AbstractTranslator` 实现 `Translatable`。

14\. **【参考】** 枚举类名建议带上 *Enum* 后缀，枚举成员名称需要全大写，单词间用下划线隔开。  
> <font color="#977C00">注意：</font>枚举其实就是特殊的常量类，且构造方法被默认强制是私有。  
> <font color="#019858">正例：</font>枚举名字为 `ProcessStatusEnum` 的成员名称：`SUCCESS / UNKNOWN_REASON`。

15\. **【参考】** 各层命名规约：  
&emsp;&emsp;A) Service/DAO 层方法命名规约  
&emsp;&emsp;&emsp;&emsp;1) 获取单个对象的方法用 `get` 作前缀。  
&emsp;&emsp;&emsp;&emsp;2) 获取多个对象的方法用 `list` 作前缀。  
&emsp;&emsp;&emsp;&emsp;3) 获取统计值的方法用 `count` 作前缀。  
&emsp;&emsp;&emsp;&emsp;4) 插入的方法用 `save/insert` 作前缀。  
&emsp;&emsp;&emsp;&emsp;5) 删除的方法用 `remove/delete` 作前缀。  
&emsp;&emsp;&emsp;&emsp;6) 修改的方法用 `update` 作前缀。   
&emsp;&emsp;B) 领域模型命名规约  
&emsp;&emsp;&emsp;&emsp;1) 数据对象： xxxDO， xxx 即为数据表名。  
&emsp;&emsp;&emsp;&emsp;2) 数据传输对象： xxxDTO， xxx 为业务领域相关的名称。  
&emsp;&emsp;&emsp;&emsp;3) 展示对象： xxxVO， xxx 一般为网页名称。  
&emsp;&emsp;&emsp;&emsp;4) POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。

### <font color="green">常量定义</font>
1\. **【强制】** Magic values, except for predefined, are forbidden in coding.       
> <font color="#FF4500">反例：</font> String key = <font color="blue">"Id#taobao_"</font> + tradeId;  

2\. **【强制】** 'L' instead of 'l' should be used for long or Long variable because 'l' is easily to be regarded as number 1 in mistake.  
> <font color="#FF4500">反例：</font>`Long a = 2l`, it is hard to tell whether it is number 21 or Long 2.

3\. **【推荐】** Constants should be placed in different constant classes based on their functions. For example, cache related constants could be put in `CacheConsts` while configuration related constants could be kept in `ConfigConsts`.   
 > <font color="#977C00">注意：</font>It is difficult to find one constant in one big complete constant class.

4\. **【推荐】** Constants can be shared in the following 5 different layers: *shared in multiple applications; shared inside an application;  shared in a sub-project;  shared in a package; shared in a class*.  
&emsp;&emsp;1) Shared in multiple applications: keep in  a library, under `constant` directory in client.jar;  
&emsp;&emsp;2) Shared in an application: keep in shared modules within the application, under `constant` directory;  
&emsp;&emsp;<font color="#FF4500">反例：</font>Obvious variable names should also be defined as common shared constants in an application. The following definitions caused an exception in the production environment: it returns *false*, but is expected to return *true* for `A.YES.equals(B.YES)`.  
&emsp;&emsp;Definition in Class A: `public static final String YES = "yes";`    
&emsp;&emsp;Definition in Class B: `public static final String YES = "y";`    
&emsp;&emsp;3) Shared in a sub-project: placed under `constant` directory in the current project;  
&emsp;&emsp;4) Shared in a package: placed under `constant` directory in current package;  
&emsp;&emsp;5) Shared in a class: defined as 'private static final' inside class.

5\. **【推荐】** Use an enumeration class if values lie in a fixed range or if the variable has attributes. The following example shows that extra information (which day it is) can be included in enumeration:  
 > <font color="#019858">正例：</font>public Enum{ MONDAY(1), TUESDAY(2), WEDNESDAY(3), THURSDAY(4), FRIDAY(5), SATURDAY(6), SUNDAY(7);}

### <font color="green">Formatting Style</font>
1\. **【强制】** Rules for braces. If there is no content, simply use *{}* in the same line. Otherwise:    
&emsp;&emsp;1) No line break before the opening brace.   
&emsp;&emsp;2) Line break after the opening brace.     
&emsp;&emsp;3) Line break before the closing brace.   
&emsp;&emsp;4) Line break after the closing brace, *only if* the brace terminates a statement or terminates a method body, constructor or named class. There is *no*    line break after the closing brace if it is followed by `else` or a comma.

2\. **【强制】** No space is used between the '(' character and its following character. Same for the ')' character and its preceding character. Refer to the *Positive Example* at the 5th rule.  

3\. **【强制】** There must be one space between keywords, such as if/for/while/switch, and parentheses.

4\. **【强制】** There must be one space at both left and right side of operators, such as '=', '&&', '+', '-', *ternary operator*, etc.

5\. **【强制】** Each time a new block or block-like construct is opened, the indent increases by four spaces. When the block ends, the indent returns to the previous indent level. Tab characters are not used for indentation.   
> <font color="#977C00">注意：</font>To prevent tab characters from being used for indentation, you must configure your IDE. For example, "Use tab character" should be unchecked in IDEA, "insert spaces for tabs" should be checked in Eclipse.  
> <font color="#019858">正例：</font>     
```java
public static void main(String[] args) {
    // four spaces indent
    String say = "hello";
    // one space before and after the operator
    int flag = 0;
    // one space between 'if' and '(';
    // no space between '(' and 'flag' or between '0' and ')'
    if (flag == 0) {
        System.out.println(say);
    }
    // one space before '{' and line break after '{'
    if (flag == 1) {
        System.out.println("world");
    // line break before '}' but not after '}' if it is followed by 'else'
    } else {  
        System.out.println("ok");
    // line break after '}' if it is the end of the block
    }
}
```

6\. **【强制】** Java code has a column limit of 120 characters. Except import statements, any line that would exceed this limit must be line-wrapped as follows:   
&emsp;&emsp;1) The second line should be intented at 4 spaces with respect to the first one. The third line and following ones should align with the second line.      
&emsp;&emsp;2) Operators should be moved to the next line together with following context.    
&emsp;&emsp;3) Character '.' should be moved to the next line together with the method after it.    
&emsp;&emsp;4) If there are multiple parameters that extend over the maximum length, a line break should be inserted after a comma.   
&emsp;&emsp;5) No line breaks should appear before parentheses.  
> <font color="#019858">正例：</font>
```java
StringBuffer sb = new StringBuffer();
// line break if there are more than 120 characters, and 4 spaces indent at
// the second line. Make sure character '.' moved to the next line
// together.  The third and fourth lines are aligned with the second one.
sb.append("zi").append("xin").
    .append("huang")...
    .append("huang")...
    .append("huang");
```
> <font color="#FF4500">反例：</font>
```java
StringBuffer sb = new StringBuffer();
// no line break before '('
sb.append("zi").append("xin")...append
    ("huang");  
// no line break before ',' if there are multiple params
invoke(args1, args2, args3, ...
    , argsX);
```

7\. **【强制】** There must be one space between a comma and the next parameter for methods with multiple parameters.   
> <font color="#019858">正例：</font>One space is used after the *'<font color="blue">,</font>'* character in the following method definition.
```java
f("a", "b", "c");
```

8\. **【强制】** The charset encoding of text files should be *UTF-8* and the characters of line breaks should be in *Unix* format, instead of *Windows* format.

9\. **【推荐】** It is unnecessary to align variables by several spaces.   
> <font color="#019858">正例：</font>
```java
int a = 3;
long b = 4L;
float c = 5F;
StringBuffer sb = new StringBuffer();
```
> <font color="#977C00">注意：</font>It is cumbersome to insert several spaces to align the variables above.   

10\. **【推荐】** Use a single blank line to separate sections with the same logic or semantics.
> <font color="#977C00">注意：</font>It is unnecessary to use multiple blank lines to do that.

### <font color="green">OOP Rules</font>
1\. **【强制】** A static field or method should be directly referred to by its class name instead of its corresponding object name.

2\. **【强制】** An overridden method from an interface or abstract class must be marked with `@Override` annotation.   
 > <font color="#FF4500">反例：</font>For `getObject()` and `get0bject()`, the first one has a letter 'O', and the second one has a number '0'. To accurately determine whether the overriding is successful, an `@Override` annotation is necessary. Meanwhile, once the method signature in the abstract class is changed, the implementation class will report a compile-time error immediately.

3\. **【强制】** *varargs* is recommended only if all parameters are of the same type and semantics. Parameters with `Object` type should be avoided.  
 > <font color="#977C00">注意：</font>Arguments with the *varargs* feature must be at the end of the argument list. (Programming with the *varargs* feature is not recommended.)  
 > <font color="#019858">正例：</font>
```java
public User getUsers(String type, Integer... ids);
```

4\. **【强制】** Modifying the <font color="blue">method signature</font> is forbidden to avoid affecting the caller. A `@Deprecated` annotation with an explicit description of the new service is necessary when an interface is deprecated.

5\. **【强制】** Using a deprecated class or method is prohibited.  
 > <font color="#977C00">注意：</font>For example, `decode(String source, String encode)` should be used instead of the deprecated method `decode(String encodeStr)`. Once an interface has been deprecated, the interface provider has the obligation to provide a new one. At the same time, client programmers have  the obligation to use the new interface.

6\. **【强制】** Since `NullPointerException` can possibly be thrown while calling the *equals* method of `Object`, *equals* should be invoked by a constant or an object that is definitely not *null*.  
 > <font color="#019858">正例：</font> `"test".equals(object);`   
 > <font color="#FF4500">反例：</font> `object.equals("test");`    
 > <font color="#977C00">注意：</font>`java.util.Objects#equals` (a utility class in JDK7) is recommended.

7\. **【强制】** Use the `equals` method, rather than reference equality '==', to compare primitive wrapper classes.
 > <font color="#977C00">注意：</font>Consider this assignment: `Integer var = ?`. When it fits the range from <font color="blue">-128 to 127</font>, we can use `==` directly for a comparison. Because the `Integer` object will be generated by `IntegerCache.cache`, which reuses an existing object. Nevertheless, when it fits the complementary set of the former range, the `Integer` object will be allocated in the heap, which does not reuse an existing object. This is a pitfall. Hence the `equals` method is recommended.

8\. **【强制】** Rules for using primitive data types and wrapper classes:     
&emsp;&emsp;1) Members of a POJO class must be wrapper classes.  
&emsp;&emsp;2) The return value and arguments of a RPC method must be wrapper classes.   
&emsp;&emsp;3) **【推荐】** Local variables should be primitive data types.  
&emsp;&emsp;<font color="#977C00">注意：</font>In order to remind the consumer of explicit assignments, there are no initial values for members in a POJO class. As a consumer, you should check problems such as `NullPointerException` and warehouse entries for yourself.   
&emsp;&emsp;<font color="#019858">正例：</font>As the result of a database query may be *null*, assigning it to a primitive date type will cause a risk of `NullPointerException` because of autoboxing.     
&emsp;&emsp;<font color="#FF4500">反例：</font>Consider the output of a  transaction volume's amplitude, like *±x%*. As a primitive data, when it comes to a failure of calling a RPC service, the default return value: *0%* will be assigned, which is not correct. A hyphen like *-* should be assigned instead. Therefore, the *null* value of a wrapper class can represent additional information, such as a failure of calling a RPC service, an abnormal exit, etc.

9\. **【强制】** While defining POJO classes like DO, DTO, VO, etc., do not assign any <font color="blue">default values</font> to the members.   

10\. **【强制】** To avoid a deserialization failure, do not change the *serialVersionUID* when a serialized class needs to be updated, such as adding some new members. If a completely incompatible update is needed, change the value of *serialVersionUID* in case of a confusion when deserialized.  
> <font color="#977C00">注意：</font>The inconsistency of *serialVersionUID* may cause an `InvalidClassException` at runtime.

11\. **【强制】** Business logic in constructor methods is prohibited. All initializations should be implemented in the `init` method.

12\. **【强制】** The `toString` method must be implemented in a POJO class. The `super.toString` method should be called in in the beginning of the implementation if the current class extends another POJO class.
 > <font color="#977C00">注意：</font>We can call the `toString` method in a POJO directly to print property values in order to check the problem when a method throws an exception in runtime.

13\. **【推荐】** When accessing an array generated by the split method in String using an index, make sure to check the last separator whether it is null to avoid `IndexOutOfBoundException`.  
> <font color="#977C00">注意：</font>
``` java
String str = "a,b,c,,";
String[] ary = str.split(",");
// The expected result exceeds 3. However it turns out to be 3.
System.out.println(ary.length);  
```

14\. **【推荐】** Multiple constructor methods or homonymous methods in a class should be put together for better readability.

15\. **【推荐】** The order of methods declared within a class is:  
*public or protected methods -> private methods -> getter/setter methods*.          
 > <font color="#977C00">注意：</font> As the most concerned ones for consumers and providers, *public* methods should be put on the first screen. *Protected* methods are only cared for by the subclasses, but they have chances to be vital when it comes to Template Design Pattern. *Private* methods, the black-box approaches, basically are not significant to clients. *Getter/setter* methods of a Service or a DAO should be put at the end of the class implementation because of the low significance.

16\. **【推荐】** For a *setter* method, the argument name should be the same as the field name. Implementations of business logics in *getter/setter* methods, which will increase difficulties of the troubleshooting, are not recommended.  
 > <font color="#FF4500">反例：</font>
 ```java
 public Integer getData() {
     if (true) {
         return data + 100;
     } else {
         return data - 100;
     }
 }
 ```  

17\. **【推荐】** Use the `append` method in `StringBuilder` inside a loop body when concatenating multiple strings.  
 > <font color="#FF4500">反例：</font>  
```java
String str = "start";
for (int i = 0; i < 100; i++) {
    str = str + "hello";
}
```  
> <font color="#977C00">注意：</font>According to the decompiled bytecode file, for each loop, it allocates a `StringBuilder` object, appends a string, and finally returns a `String` object via the `toString` method. This is a tremendous waste of memory.

18\. **【推荐】** Keyword *final* should be used in the following situations:    
&emsp;&emsp;1) A class which is not allow to be inherited, or a local variable not to be reassigned.  
&emsp;&emsp;2) An argument which is not allow to be modified.   
&emsp;&emsp;3) A method which is not allow to be overridden.

19\. **【推荐】** Be cautious to copy an object using the `clone` method in `Object`.  
 > <font color="#977C00">注意：</font>The default implementation of `clone` in `Object` is a shallow (not deep) copy, which copies fields as pointers to the same objects in memory.

20\. **【推荐】** Define the access level of members in class with severe  restrictions:    
&emsp;&emsp;1) Constructor methods must be `private` if an allocation using `new` keyword outside of the class is forbidden.   
&emsp;&emsp;2) Constructor methods are not allowed to be `public` or `default` in a utility class.   
&emsp;&emsp;3) Nonstatic class variables that are accessed from inheritants must be `protected`.  
&emsp;&emsp;4) Nonstatic class variables that no one can access except the class that contains them must be `private`.  
&emsp;&emsp;5) Static variables that no one can access except the class that contains them must be `private`.       
&emsp;&emsp;6) Static variables should be considered in determining whether they are `final`.  
&emsp;&emsp;7) Class methods that no one can access except the class that contains them must be `private`.  
&emsp;&emsp;8) Class methods that are accessed from inheritants must be `protected`.   
 > <font color="#977C00">注意：</font> We should strictly control the access for any classes, methods, arguments and variables. Loose access control causes harmful coupling of modules. Imagine the following situations. For a `private` class member, we can remove it as soon as we want. However, when it comes to a `public` class member, we have to think twice before any updates happen to it.

### <font color="green">Collection</font>
1\. **【强制】** The usage of *hashCode* and *equals* should follow:   
&emsp;&emsp;1) Override *hashCode* if *equals* is overridden.  
&emsp;&emsp;2) These two methods must be overridden for elements of a `Set` since they are used to ensure that no duplicate object will be inserted in `Set`.  
&emsp;&emsp;3) These two methods must be overridden for any object that is used as the key of `Map`.  
> <font color="#977C00">注意：</font> `String` can be used as the key of `Map` since `String` defines these two methods.

2\. **【强制】** Do not add elements to collection objects returned by `keySet()`/`values()`/`entrySet()`, otherwise `UnsupportedOperationException` will be thrown.

3\. **【强制】** Do not add nor remove to/from immutable objects returned by methods in `Collections`, e.g. `emptyList()`/`singletonList()`.    
> <font color="#FF4500">反例：</font>Adding elements to `Collections.emptyList()` will throw `UnsupportedOperationException`.

4\. **【强制】** Do not cast *subList* in class `ArrayList`, otherwise  `ClassCastException` will be thrown: `java.util.RandomAccessSubList` cannot be cast to `java.util.ArrayList`.  
 > <font color="#977C00">注意：</font>`subList` of `ArrayList` is an inner class, which is a view of `ArrayList`. All operations on the `Sublist` will affect the original list.

5\. **【强制】** When using *subList*, be careful when modifying the size of original list. It might cause `ConcurrentModificationException` when performing traversing, adding or deleting on the *subList*.   

6\. **【强制】** Use `toArray(T[] array)` to convert a list to an array. The input array type should be the same as the list whose size is `list.size()`.    
 > <font color="#FF4500">反例：</font> Do not use `toArray` method without arguments. Since the return type is `Object[]`, `ClassCastException` will be thrown when casting it to a different array type.  
> <font color="#019858">正例：</font>
```java
		List<String> list = new ArrayList<String>(2);
		list.add("guan");
		list.add("bao");		
		String[] array = new String[list.size()];
		array = list.toArray(array);
```  
> <font color="#977C00">注意：</font>When using `toArray` method with arguments, pass an input with the same size as the list.  If input array size is not large enough, the method will re-assign the size internally, and then return the address of new array. If the size is larger than needed, extra elements (`index[list.size()]` and later) will be set to *null*.

7\. **【强制】** Do not use methods which will modify the list after using `Arrays.asList` to convert array to list, otherwise methods like *add/remove/clear* will throw `UnsupportedOperationException`.   
> <font color="#977C00">注意：</font>The result of `asList` is the inner class of `Arrays`, which does not implement methods to modify itself. `Arrays.asList` is only a transferred interface, data inside which is stored as an array.
```java
String[] str = new String[] { "a", "b" };
List<String> list = Arrays.asList(str);
```   
 Case 1: `list.add("c");` will throw a runtime exception.  
 Case 2: `str[0]= "gujin";` `list.get(0)` will be modified.

8\. **【强制】** Method `add` cannot be used for generic wildcard with `<? Extends T>`, method `get` cannot be used with `<? super T>`, which probably goes wrong.   
> <font color="#977C00">注意：</font>About PECS (Producer Extends Consumer Super) principle:  
1) `Extends` is suitable for frequently reading scenarios.   
2) `Super` is suitable for frequently inserting scenarios.

9\. **【强制】** Do not remove or add elements to a collection in a *foreach* loop. Please use `Iterator` to remove an item. `Iterator` object should be synchronized when executing concurrent operations.  
 > <font color="#FF4500">反例：</font>
```java
List<String> a = new ArrayList<String>();
a.add("1");
a.add("2");
for (String temp : a) {
    if ("1".equals(temp)){
        a.remove(temp);
    }
}
```
> <font color="#977C00">注意：</font> If you try to replace "1" with "2", you will get an unexpected result.
> <font color="#019858">正例：</font>
```java
Iterator<String> it = a.iterator();
while (it.hasNext()) {    
    String temp =  it.next();             
    if (delete condition) {              
        it.remove();       
    }
}    
```  

10\. **【强制】** In JDK 7 and above version, `Comparator` should meet the three requirements listed below, otherwise `Arrays.sort` and `Collections.sort` will throw `IllegalArgumentException`.    
> <font color="#977C00">注意：</font>  
&emsp;&emsp;1) Comparing x,y and y,x should return the opposite result.  
&emsp;&emsp;2) If x>y and y>z, then x>z.  
&emsp;&emsp;3) If x=y, then comparing x with z and comparing y with z should return the same result.    
> <font color="#FF4500">反例：</font>The program below cannot handle the case if o1 equals to o2, which might cause an exception in a real case:  
```java
new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
        return o1.getId() > o2.getId() ? 1 : -1;
    }
}
```

11\. **【推荐】** Set a size when initializing a collection if possible.    
> <font color="#977C00">注意：</font>Better to use `ArrayList(int initialCapacity)` to initialize `ArrayList`.

12\. **【推荐】** Use `entrySet` instead of `keySet` to traverse KV maps.   
> <font color="#977C00">注意：</font>Actually, `keySet` iterates through the map twice, firstly convert to `Iterator` object, then get the value from the `HashMap` by key. `EntrySet` iterates only once and puts keys and values in the entry which is more efficient. Use `Map.foreach` method in JDK8.  
 > <font color="#019858">正例：</font>`values()` returns a list including all values, `keySet()` returns a set including all values, `entrySet()` returns a k-v combined object.

13\. **【推荐】** Carefully check whether a *k/v collection* can store *null* value, refer to the table below:

 | Collection | Key | Value | Super | Note |
  | --- | --- | --- | --- | --- |
 | Hashtable | <font color="#FF4500">Null is not allowed</font>| <font color="#FF4500">Null is not allowed</font>| Dictionary| Thread-safe |
 | ConcurrentHashMap | <font color="#FF4500">Null is not allowed</font>| <font color="#FF4500">Null is not allowed</font>| AbstractMap| Segment lock |
 | TreeMap | <font color="red">Null is not allowed</font>| <font color="blue">Null is allowed</font>| AbstractMap| Thread-unsafe |
 | HashMap | <font color="blue">Null is allowed</font>| <font color="blue">Null is allowed</font>|  AbstractMap| Thread-unsafe |

> <font color="#FF4500">反例：</font>Confused by `HashMap`, lots of people think *null* is allowed in `ConcurrentHashMap`. Actually,  `NullPointerException` will be thrown when putting in *null* value.

14\. **【参考】** Properly use sort and order of a collection to avoid negative influence of unsorted and unordered one.  
> <font color="#977C00">注意：</font>*Sorted* means that its iteration follows  specific sorting rule. *Ordered* means the order of elements in each traverse is stable. e.g. `ArrayList` is ordered and unsorted, `HashMap` is unordered and unsorted, `TreeSet` is ordered and sorted.

15\. **【参考】** Deduplication operations could be performed quickly since set stores unique values only. Avoid using method *contains* of `List` to perform traverse, comparison and de-duplication.  


### <font color="green">Concurrency</font>
1\. **【强制】** Thread-safe should be ensured when initializing singleton instance, as well as all methods in it.
> <font color="#977C00">注意：</font>Resource driven class, utility class and singleton factory class are all included.

2\. **【强制】** A meaningful thread name is helpful to trace the error information, so assign a name when creating threads or thread pools.   
 > <font color="#019858">正例：</font>
 ```java
 public class TimerTaskThread extends Thread {
		public TimerTaskThread() {
			super.setName("TimerTaskThread");
		    …
		}
 }
 ```  

3\. **【强制】** Threads should be provided by thread pools. Explicitly creating threads is not allowed.
 > <font color="#977C00">注意：</font>Using thread pool can reduce the time of creating and destroying thread and save system resource. If we do not use thread pools, lots of similar threads will be created which lead to "running out of memory" or over-switching problems.

4\. **【强制】** A thread pool should be created by `ThreadPoolExecutor` rather than `Executors`. These would make the parameters of the thread pool understandable. It would also reduce the risk of running out of system resource.  
 > <font color="#977C00">注意：</font>Below are the problems created by usage of `Executors` for thread pool creation:   
&emsp;&emsp;1) `FixedThreadPool` and `SingleThreadPool`:  
&emsp;&emsp;Maximum request queue size `Integer.MAX_VALUE`. A large number of requests might cause OOM.   
&emsp;&emsp;2) `CachedThreadPool` and `ScheduledThreadPool`:  
&emsp;&emsp;The number of threads which are allowed to be created is `Integer.MAX_VALUE`. Creating too many threads might lead to OOM.

5\. **【强制】** `SimpleDateFormat` is unsafe, do not define it as a *static* variable. If have to, lock or `DateUtils` class must be used.  
 > <font color="#019858">正例：</font>Pay attention to thread-safety when using `DateUtils`. It is recommended to use as below:
 ```java
private static final ThreadLocal<DateFormat> df = new ThreadLocal<DateFormat>() {  
    @Override  
    protected DateFormat initialValue() {  
        return new SimpleDateFormat("yyyy-MM-dd");  
    }  
};  
 ```
 > <font color="#977C00">注意：</font>In JDK8, `Instant` can be used to replace `Date`, `Calendar` is replaced by `LocalDateTime`, `Simpledateformatter` is replaced by `DateTimeFormatter`.

6\. **【强制】** `remove()` method must be implemented by `ThreadLocal` variables, especially when using thread pools in which threads are often reused. Otherwise, it may affect subsequent business logic and cause unexpected problems such as memory leak.    
> <font color="#019858">正例：</font>
```java
objectThreadLocal.set(someObject);
try {
    ...
} finally {
    objectThreadLocal.remove();
}
```

7\. **【强制】** In highly concurrent scenarios, performance of `Lock` should be considered in synchronous calls. A block lock is better than a method lock. An object lock is better than a class lock.

8\. **【强制】** When adding locks to multiple resources, tables in the database and objects at the same time, locking sequence should be kept consistent to avoid deadlock.    
> <font color="#977C00">注意：</font> If thread 1 does update after adding lock to table A, B, C accordingly, the lock sequence of thread 2 should also be A, B, C, otherwise deadlock might happen.

9\. **【强制】** A lock needs to be used to avoid update failure when modifying one record concurrently. Add lock either in application layer, in cache, or add optimistic lock in the database by using version as update stamp.  
 > <font color="#977C00">注意：</font>If access confliction probability is less than 20%, recommend to use optimistic lock, otherwise use pessimistic lock. Retry number of optimistic lock should be no less than 3.

10\. **【强制】** Run multiple `TimeTask` by using `ScheduledExecutorService` rather than `Timer` because `Timer` will kill all running threads in case of failing to catch exceptions.

11\. **【推荐】** When using `CountDownLatch` to convert asynchronous operations to synchronous ones, each thread must call `countdown` method before quitting. Make sure to catch any exception during thread running, to let `countdown` method be  executed. If main thread cannot reach `await` method, program will return until timeout.  
> <font color="#977C00">注意：</font>Be careful, exception thrown by sub-thread cannot be caught by main thread.

12\. **【推荐】** Avoid using `Random` instance by multiple threads. Although it is safe to share this instance, competition on the same seed will damage performance.  
> <font color="#977C00">注意：</font>`Random` instance includes instances of `java.util.Random` and `Math.random()`.  
> <font color="#019858">正例：</font>After JDK7, API `ThreadLocalRandom` can be used directly. But before JDK7, instance needs to be created in each thread.

13\. **【推荐】** In concurrent scenarios, one easy solution to optimize the  lazy initialization problem by using double-checked locking  (referred to The Double-checked locking is broken Declaration), is to declare the object type as `volatile`.    
> <font color="#FF4500">反例：</font>
```java
class Foo {
    private Helper helper = null;
    public Helper getHelper() {
        if (helper == null) {
            synchronized(this) {
                if (helper == null)
                helper = new Helper();
            }   
        }
        return helper;
    }
    // other functions and members...
}
```

14\. **【参考】** `volatile` is used to solve the problem of invisible memory in multiple threads. *Write-Once-Read-Many* can solve variable synchronization problem. But *Write-Many* cannot settle thread safe problem. For `count++`, use below example:　    
```java
AtomicInteger count = new AtomicInteger();
count.addAndGet(1);
```  
> <font color="#977C00">注意：</font>In JDK8, `LongAdder` is recommended which reduces retry times of optimistic lock and has better performance than `AtomicLong`.

15\. **【参考】** Resizing `HashMap` when its capacity is not enough might cause dead link and high CPU usage because of high-concurrency. Avoid this risk in development.  

16\. **【参考】** `ThreadLocal` cannot solve update problems of shared object. It is recommended to use a *static* `ThreadLocal` object which is shared by all operations in the same thread.

### <font color="green">Flow Control Statements</font>
1\. **【强制】** In a `switch` block, each case should be finished by *break/return*. If not, a note should be included to describe at which case it will stop. Within every `switch` block, a default statement must be present, even if it is empty.

2\. **【强制】** Braces are used with *if*, *else*, *for*, *do* and *while* statements, even if the body contains only a single statement. Avoid using the following example:
```java
    if (condition) statements;
```

3\. **【推荐】** Use `else` as less as possible, `if-else` statements could be replaced by:
```java
if (condition) {
    ...
    return obj;  
}  
// other logic codes in else could be moved here
```    
> <font color="#977C00">注意：</font> If statements like `if()...else if()...else...` have to be used to express the logic, **【强制】** nested conditional level should not be more than three.   
> <font color="#019858">正例：</font> `if-else` code with over three nested conditional levels can be replaced by *guard statements* or *State Design Pattern*. Example of *guard statement*:

```java
public void today() {
    if (isBusy()) {
        System.out.println("Change time.");
        return;
    }

    if (isFree()) {
        System.out.println("Go to travel.");
        return;
    }

    System.out.println("Stay at home to learn Alibaba Java Coding Guidelines.");
    return;
}
```

4\. **【推荐】** Do not use complicated statements in conditional statements (except for frequently used methods like *getXxx/isXxx*). Use *boolean* variables to store results of complicated statements temporarily will increase the code's readability.  
> <font color="#977C00">注意：</font>Logic within many `if` statements are very complicated. Readers need to analyze the final results of the conditional expression to decide what statement will be executed in certain conditions.    
> <font color="#019858">正例：</font>
```java
// please refer to the pseudo-code as follows
boolean existed = (file.open(fileName, "w") != null) && (...) || (...);
if (existed) {
    ...
}  
```  
 > <font color="#FF4500">反例：</font>  
```java
if ((file.open(fileName, "w") != null) && (...) || (...)) {
    ...
}
```

5\. **【推荐】** Performance should be considered when loop statements are used. The following operations are better to be processed outside the loop statement, including object and variable declaration, database connection, `try-catch` statements.

6\. **【推荐】** Size of input parameters should be checked, especially for batch operations.  

7\. **【参考】** Input parameters should be checked in following scenarios:     
&emsp;&emsp;1) Low-frequency implemented methods.  
&emsp;&emsp;2) Overhead of parameter checking could be ignored in long-time execution methods, but if illegal parameters lead to exception, the loss outweighs the gain. Therefore, parameter checking is still recommended in long-time execution methods.  
&emsp;&emsp;3) Methods that needs extremely high stability or availability.    
&emsp;&emsp;4) Open API methods, including RPC/API/HTTP.  
&emsp;&emsp;5) Authority related methods.

8\. **【参考】** Cases that input parameters do not require validation:  
&emsp;&emsp;1) Methods very likely to be implemented in loops. A note should be included informing that parameter check should be done externally.   
&emsp;&emsp;2) Methods in bottom layers are very frequently called so generally do not need to be checked. e.g. If *DAO* layer and *Service* layer is deployed in the same server, parameter check in *DAO* layer can be omitted.    
&emsp;&emsp;3) *Private* methods that can only be implemented internally, if all parameters are checked or manageable.

### <font color="green">Code Comments</font>
1\. **【强制】** *Javadoc* should be used for classes, class variables and methods. The format should be '/\*\* comment \*\*/', rather than '// xxx'.   
> <font color="#977C00">注意：</font> In IDE, *Javadoc* can be seen directly when hovering, which is a good way to improve efficiency.

2\. **【强制】** Abstract methods (including methods in interface) should be commented by *Javadoc*. *Javadoc* should include method instruction, description of parameters, return values and possible exceptions.  

3\. **【强制】** Every class should include information of author(s) and date.

4\. **【强制】** Single line comments in a method should be put above the code to be commented, by using `//` and multiple lines by using `/* */`. Alignment for comments should be noticed carefully.

5\. **【强制】** All enumeration type fields should be commented as *Javadoc* style.

6\. **【推荐】** Local language can be used in comments if English cannot describe the problem properly. Keywords and proper nouns should be kept in English.  
 > <font color="#FF4500">反例：</font>To explain "TCP connection overtime" as "Transmission Control Protocol connection overtime" only makes it more difficult to understand.

7\. **【推荐】** When code logic changes, comments need to be updated at the same time, especially for the changes of parameters, return value, exception and core logic.

8\. **【参考】** Notes need to be added when commenting out code.  
> <font color="#977C00">注意：</font> If the code is likely to be recovered later, a reasonable explanation needs to be added. If not, please delete directly because code history will be recorded by *svn* or *git*.

9\. **【参考】** Requirements for comments:   
&emsp;&emsp;1) Be able to represent design ideas and code logic accurately.   
&emsp;&emsp;2) Be able to represent business logic and help other programmers understand quickly. A large section of code without any comment is a disaster for readers. Comments are written for both oneself and other people. Design ideas can be quickly recalled even after a long time. Work can be quickly taken over by other people when needed.

10\. **【参考】** Proper naming and clear code structure are self-explanatory. Too many comments need to be avoided because it may cause too much work on updating if code logic changes.
 > <font color="#FF4500">反例：</font>  
```java
// put elephant into fridge
put(elephant, fridge);  
```  
The name of method and parameters already represent what does the method do, so there is no need to add extra comments.

11\. **【参考】** Tags in comments (e.g. TODO, FIXME) need to contain author and time. Tags need to be handled and cleared in time by scanning frequently. Sometimes online breakdown is caused by these unhandled tags.    
&emsp;&emsp;1) TODO: TODO means the logic needs to be done, but not finished yet. Actually,  TODO is a member of *Javadoc*, although it is not realized in *Javadoc* yet, but has already been widely used. TODO can only be used in class, interface and methods, since it is a *Javadoc* tag.   
&emsp;&emsp;2) FIXME: FIXME is used to represent that the code logic is not correct or does not work, should be fixed in time.

### <font color="green">Other</font>
1\. **【强制】** When using regex, precompile needs to be done in order to increase the matching performance.  
> <font color="#977C00">注意：</font> Do not define `Pattern pattern = Pattern.compile(.);` within method body.

2\. **【强制】** When using attributes of POJO in velocity, use attribute names directly. Velocity engine will invoke `getXxx()` of POJO automatically. In terms of *boolean* attributes, velocity engine will invoke `isXxx()` (Do not use *is* as prefix when naming boolean attributes).  
> <font color="#977C00">注意：</font>For wrapper class `Boolean`, velocity engine will invoke `getXxx()` first.

3\. **【强制】** Variables must add exclamatory mark when passing to velocity engine from backend, like \$!{var}.  
 > <font color="#977C00">注意：</font>If attribute is *null* or does not exist, \${var} will be shown directly on web pages.

4\. **【强制】** The return type of `Math.random()` is double, value range is *0<=x<1* (<font color="blue">0</font> is possible). If a random integer is required, do not multiply x by 10 then round the result. The correct way is to use `nextInt` or `nextLong` method which belong to Random Object.

5\. **【强制】** Use `System.currentTimeMillis()` to get the current millisecond. Do not use `new Date().getTime()`.
> <font color="#977C00">注意：</font>In order to get a more accurate time, use `System.nanoTime()`. In JDK8, use `Instant` class to deal with situations like time statistics.

6\. **【推荐】** It is better not to contain variable declaration, logical symbols or any complicated logic in velocity template files.

7\. **【推荐】** Size needs to be specified when initializing any data structure if possible, in order to avoid memory issues caused by unlimited growth.

8\. **【推荐】** Codes or configuration that is noticed to be obsoleted should be resolutely removed from projects.   
> <font color="#977C00">注意：</font>Remove obsoleted codes or configuration in time to avoid code redundancy.  
 > <font color="#019858">正例：</font>For codes which are temporarily removed and likely to be reused, use **`///`** to add a reasonable note.
```java
public static void hello() {
    /// Business is stopped temporarily by the owner.
    // Business business = new Business();
    // business.active();
    System.out.println("it's finished");
}
```

## <font color="green">2. Exception and Logs</font>

### <font color="green">Exception</font>
1\. **【强制】** Do not catch *Runtime* exceptions defined in *JDK*, such as `NullPointerException` and `IndexOutOfBoundsException`. Instead, pre-check is recommended whenever possible.  
> <font color="#977C00">注意：</font>Use try-catch only if it is difficult to deal with pre-check, such as `NumberFormatException`.  
> <font color="#019858">正例：</font>```if (obj != null) {...}```  
> <font color="#FF4500">反例：</font> ```try { obj.method() } catch(NullPointerException e){…}```

2\. **【强制】** Never use exceptions for ordinary control flow. It is ineffective and unreadable.

3\. **【强制】** It is irresponsible to use a try-catch on a big chunk of code. Be clear about the stable and unstable code when using try-catch. The stable code that means no exception will throw. For the unstable code, catch as specific as possible for exception handling.

4\. **【强制】** Do not suppress or ignore exceptions. If you do not want to handle it, then re-throw it. The top layer must handle the exception and translate it into what the user can understand.

5\. **【强制】** Make sure to invoke the rollback if a method throws an Exception.

6\. **【强制】** Closeable resources (stream, connection, etc.) must be handled in *finally* block. Never throw any exception from a *finally* block.  
> <font color="#977C00">注意：</font>Use the *try-with-resources* statement to safely handle closeable resources (Java 7+).

7\. **【强制】** Never use *return* within a *finally* block. A *return* statement in a *finally* block will cause exceptions or result in a discarded return value in the *try-catch* block.

8\. **【强制】** The *Exception* type to be caught needs to be the same class or superclass of the type that has been thrown.

9\. **【推荐】** The return value of a method can be *null*. It is not mandatory to return an empty collection or object. Specify in *Javadoc* explicitly when the method might return *null*. The caller needs to make a *null* check to prevent `NullPointerException`.   
> <font color="#977C00">注意：</font>It is caller's responsibility to check the return value, as well as to consider the possibility that remote call fails or other runtime exception occurs.

10\. **【推荐】** One of the most common errors is `NullPointerException`. Pay attention to the following situations:  
&emsp;&emsp;1) If the return type is primitive, return a value of wrapper class may cause `NullPointerException`.  
&emsp;&emsp;&emsp;&emsp;<font color="#FF4500">反例：</font>`public int f() { return Integer }` Unboxing a *null* value will throw a `NullPointerException`.   
&emsp;&emsp;2) The return value of a database query might be *null*.    
&emsp;&emsp;3) Elements in collection may be *null*, even though `Collection.isEmpty()` returns *false*.   
&emsp;&emsp;4) Return values from an RPC might be *null*.    
&emsp;&emsp;5) Data stored in sessions might by *null*.  
&emsp;&emsp;6) Method chaining, like `obj.getA().getB().getC()`, is likely to cause `NullPointerException`.  
&emsp;&emsp;&emsp;&emsp;<font color="#019858">正例：</font>Use `Optional` to avoid null check and NPE (Java 8+).  

11\. **【推荐】** Use "throw exception" or "return error code". For HTTP or open API providers, "error code" must be used. It is recommended to throw exceptions inside an application. For cross-application RPC calls, <font color="blue">result</font> is preferred by encapsulating *isSuccess*, *error code* and brief error messages.  
> <font color="#977C00">注意：</font>Benefits to return Result for the RPC methods:  
&emsp;&emsp;1) Using the 'throw exception' method will occur a runtime error if the exception is not caught.  
&emsp;&emsp;2) If stack information it not attached, allocating custom exceptions with simple error message is not helpful to solve the problem. If stack information is attached, data serialization and transmission performance loss are also problems when frequent error occurs.

12\. **【推荐】** Do not throw `RuntimeException`, `Exception`, or `Throwable` directly. It is recommended to use well defined custom exceptions such as `DAOException`, `ServiceException`, etc.

13\. **【参考】** Avoid duplicate code (Do not Repeat Yourself, also known as DRY principle).  
> <font color="#977C00">注意：</font>Copying and pasting code arbitrarily will inevitably lead to duplicated code. If you keep logic in one place, it is easier to change when needed. If necessary, extract common codes to methods, abstract classes or even shared modules.    
> <font color="#019858">正例：</font>For a class with a number of public methods that validate parameters in the same way, it is better to extract a method like:
```java
private boolean checkParam (DTO dto) {
    ...
}
```

### <font color="green">Logs</font>
1\. **【强制】** Do not use API in log system (Log4j, Logback) directly. API in log framework SLF4J is recommended to use instead, which uses *Facade* pattern and is conducive to keep log processing consistent.
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
private static final Logger logger = LoggerFactory.getLogger(Abc.class);
```

2\. **【强制】** Log files need to be kept for at least 15 days because some kinds of exceptions happen weekly.

3\. **【强制】** Naming conventions of extended logs of an Application (such as RBI, temporary monitoring, access log, etc.):     
*appName\_logType\_logName.log*   
*logType*: Recommended classifications are *stats*, *desc*, *monitor*, *visit*, etc.    
*logName*: Log description.  
Benefits of this scheme: The file name shows what application the log belongs to, type of the log and what purpose is the log used for. It is also conducive for classification and search.   
> <font color="#019858">正例：</font>Name of the log file for monitoring the timezone conversion exception in *mppserver* application: *mppserver_monitor_timeZoneConvert.log*  
> <font color="#977C00">注意：</font>It is recommended to classify logs. Error logs and business logs should be stored separately as far as possible. It is not only easy for developers to view, but also convenient for system monitoring.

4\. **【强制】** Logs at *TRACE / DEBUG / INFO* levels must use either conditional outputs or placeholders.
> <font color="#977C00">注意：</font>`logger.debug ("Processing trade with id: " + id + " symbol: " + symbol);` If the log level is warn, the above log will not be printed. However, it will perform string concatenation operator. `toString()` method of *symbol* will be called, which is a waste of system resources.   
> <font color="#019858">正例：</font>
```java
if (logger.isDebugEnabled()) {
    logger.debug("Processing trade with id: " + id + " symbol: " + symbol);
}     
```  
> <font color="#019858">正例：</font>
```java
logger.debug("Processing trade with id: {} and symbol : {} ", id, symbol);
```  

5\. **【强制】** Ensure that additivity attribute of a Log4j logger is set to *false*, in order to avoid redundancy and save disk space.  
> <font color="#019858">正例：</font>
`<logger name="com.taobao.ecrm.member.config" additivity="false" \>`


6\. **【强制】** The exception information should contain two types of information: the context information and the exception stack. If you do not want to handle the exception, re-throw it.  
 > <font color="#019858">正例：</font>
```java
logger.error(various parameters or objects toString + "_" + e.getMessage(), e);
```

7\. **【推荐】** Carefully record logs. Use *Info* level selectively and do not use *Debug* level in production environment. If *Warn* is used to record business behavior, please pay attention to the size of logs. Make sure the server disk is not over-filled, and remember to delete these logs in time.  
> <font color="#977C00">注意：</font> Outputting a large number of invalid logs will have a certain impact on system performance, and is not conducive to rapid positioning problems. Please think about the log: do you really have these logs? What can you do to see this log? Is it easy to troubleshoot problems?

8\. **【推荐】** Level *Warn* should be used to record invalid parameters, which is used to track data when problem occurs. Level *Error* only records the system logic error, abnormal and other important error messages.

## <font color="green">3. MySQL Rules</font>
### <font color="green">Table Schema Rules</font>
1\. **【强制】** Columns expressing the concept of *True* or *False*, must be named as *is_xxx*, whose data type should be *unsigned tinyint* (1 is *True*, 0 is *False*).  
> <font color="#977C00">注意：</font> All columns with non-negative values must be *unsigned*.

2\. **【强制】** Names of tables and columns must consist of lower case letters, digits or underscores. Names starting with digits and names which contain only digits (no other characters) in between two underscores are not allowed. Columns should be named cautiously, as it is costly to change column names and cannot be released in pre-release environment.  
> <font color="#019858">正例：</font>*getter\_admin, task\_config, level3_name*   
> <font color="#FF4500">反例：</font> *GetterAdmin, taskConfig, level\_3\_name*

3\. **【强制】** Plural nouns are not allowed as table names.

4\. **【强制】** Keyword, such as *desc*, *range*, *match*, *delayed*, etc., should not be used. It can be referenced from MySQL official document.

5\. **【强制】** The name of primary key index should be prefixed with *pk\_*, followed by column name; Unique index should be named by prefixing its column name with *uk\_*; And normal index should be formatted as *idx_[column_name]*.  
> <font color="#977C00">注意：</font> *pk* means primary key, *uk* means  unique key, and *idx* is short for index.

6\. **【强制】** Decimals should be typed as *decimal*. *float* and *double* are not allowed.  
> <font color="#977C00">注意：</font> It may have precision loss when float and double numbers are stored, which in turn may lead to incorrect data comparison result. It is recommended to store integral and fractional parts separately when data range to be stored is beyond the range covered by decimal type.

7\. **【强制】** Use *char* if lengths of information to be stored in that column are almost the same.

8\. **【强制】** The length of *varchar* should not exceed 5000, otherwise it should be defined as `text`. It is better to store them in a separate table in order to avoid its effect on indexing efficiency of other columns.  

9\. **【强制】** A table must include three columns as following: *id*, *gmt_create* and *gmt_modified*.  
> <font color="#977C00">注意：</font> *id* is the primary key, which is *unsigned bigint* and self-incrementing with step length of 1. The type of *gmt_create* and *gmt_modified* should be *DATE_TIME*.

10\. **【推荐】** It is recommended to define table name as *[table_business_name]_[table_purpose]*.  
 > <font color="#019858">正例：</font>  *tiger_task*  /  *tiger_reader*  / *mpp_config*   

11\. **【推荐】** Try to define database name same with the application name.

12\. **【推荐】** Update column comments once column meaning is changed or new possible status values are added.

13\. **【推荐】** Some appropriate columns may be stored in multiple tables redundantly to improve search performance, but consistency must be concerned. Redundant columns should not be:     
&emsp;&emsp;1) Columns with frequent modification.  
&emsp;&emsp;2) Columns typed with very long *varchar* or *text*.  
> <font color="#019858">正例：</font> Product category names are short, frequently used and with almost never changing/fixed values. They may be stored redundantly in relevant tables to avoid joined queries.

14\. **【推荐】** Database sharding may only be recommended when there are more than 5 million rows in a single table or table capacity exceeds 2GB.  
> <font color="#977C00">注意：</font>Please do not shard during table creation if anticipated data quantity is not to reach this grade.  


15\. **【参考】** Appropriate *char* column length not only saves database and index storing space, but also improves query efficiency.  
> <font color="#019858">正例：</font> Unsigned types could avoid storing negative values mistakenly, but also may cover bigger data representative range.

| Object | Age | Recommended data type | Range |
| --- | --- | --- | --- |
| human | within 150 years old | unsigned tinyint| unsigned integers: 0 to 255 |
| turtle | hundreds years old | unsigned smallint | unsigned integers: 0 to 65,535 |
| dinosaur <br> fossil | tens of millions years old | unsigned int | unsigned integers: <br> 0 to around 4.29 billion |
| sun | around 5 billion years old | unsigned bigint | unsigned integers: 0 to around 10^19 |

### <font color="green">Index Rules</font>
1\. **【强制】** Unique index should be used if business logic is applicable.    
   > <font color="#977C00">注意：</font> Negative impact of unique indices on insert efficiency is neglectable, but it improves query speed significantly. Additionally, even if complete check is done at the application layer, as per Murphy's Law, dirty data might still be produced, as long as there is no unique index.

2\. **【强制】** *JOIN* is not allowed if more than three tables are involved. Columns to be joined must be with absolutely similar data types. Make sure that  columns to be joined are indexed.   
> <font color="#977C00">注意：</font>Indexing and SQL performance should be considered even if only 2 tables are joined.  

3\. **【强制】** Index length must be specified when adding index on *varchar* columns. The index length should be set according to the distribution of data.  
> <font color="#977C00">注意：</font>Normally for *char* columns, an index with the length of 20 can distinguish more than 90% data, which is calculated by *count(distinct left(column_name, index_length)) / count(*)*.

4\. **【强制】** *LIKE '%...'* or *LIKE '%...%'* are not allowed when searching with pagination. Search engine can be used if it is really needed.  
 > <font color="#977C00">注意：</font>Index files have B-Tree's *left most prefix matching* characteristic. Index cannot be applied if left prefix value is not determined.

5\. **【推荐】** Make use of the index order when using *ORDER BY* clauses. The last columns of *ORDER BY* clauses should be at the end of a composite index. The reason is to avoid the *file_sort* issue, which affects the query performance.    
> <font color="#019858">正例：</font> *where a=? and b=? order by c;* Index is: *a\_b\_c*      </br>
> <font color="#FF4500">反例：</font>The index order will not take effect if the query condition contains a range, e.g., *where a>10 order by b;* Index *a_b* cannot be activated.

6\. **【推荐】** Make use of *Covering Index* for query to avoid additional query after searching index.   
> <font color="#977C00">注意：</font> If we need to check the title of Chapter 11 of a book, do we need turn to the page where Chapter 11 starts? No, because the table of contents actually includes the title, which serves as a covering index.  
> <font color="#019858">正例：</font>Index types include *primary key index*, *unique index* and *common index*. *Covering index* pertains to a query effect. When refer to explain result, *using index* may appear in extra columns.

7\. **【推荐】** Use *late join* or *sub-query* to optimize scenarios with many pages.  
> <font color="#977C00">注意：</font>Instead of bypassing *offset* rows, MySQL retrieves totally *offset+N* rows, then drops off offset rows and returns N rows. It is very inefficient when offset is very big. The solution is either limiting the number of pages to be returned, or rewriting SQL statement when page number exceeds a predefined threshold.  
> <font color="#019858">正例：</font>Firstly locate the required *id* range quickly, then join:  
   select a.* from table1 a, (select id from table1 where *some_condition* LIMIT 100000, 20) b where a.id=b.id;  

8\. **【推荐】** The target of SQL performance optimization is that the result type of *EXPLAIN* reaches *REF* level, or *RANGE* at least, or CONSTS if possible.   
> <font color="#FF4500">反例：</font>Pay attention to the type of *INDEX* in *EXPLAIN* result because it is very slow to do a full scan to the database index file, whose performance nearly equals to an all-table scan.  
<font color="green">CONSTS: </font>There is at most one matching row, which is read by the optimizer. It is very fast.   
<font color="green">REF:</font> The normal index is used.   
<font color="green">RANGE:</font> A given range of index are retrieved, which can be used when a key column is compared to a constant by using any of the =, <>, >, >=, <, <=, IS NULL, <=>, BETWEEN, or IN() operators.   

9\. **【推荐】** Put the most discriminative column to the left most when adding a composite index.  
> <font color="#019858">正例：</font>For the sub-clause *where a=? and b=?*, if data of column *a* is nearly unique, adding index *idx_a* is enough.  
> <font color="#977C00">注意：</font>When *equal* and *non-equal* check both exist in query conditions, put the column in *equal* condition first when adding an index. For example, *where a>? and b=?*, *b* should be put as the 1st column of the index, even if column *a* is more discriminative.

10\. **【参考】** Avoid listed below misunderstandings when adding index:  
&emsp;&emsp;1) It is false that each query needs one index.   
&emsp;&emsp;2) It is false that index consumes story space and degrades *update*, *insert* operations significantly.  
&emsp;&emsp;3) It is false that *unique index* should all be achieved from application layer by "check and insert".

### <font color="green">SQL Rules</font>
1\. **【强制】** Do not use COUNT(column_name) or COUNT(constant_value) in place of COUNT(\*). COUNT(\*) is *SQL92* defined standard syntax to count the number of rows. It is not database specific and has nothing to do with *NULL* and *non-NULL*.   
> <font color="#977C00">注意：</font>COUNT(\*) counts NULL row in, while  COUNT(column_name) does not take *NULL* valued row into consideration.

2\. **【强制】** COUNT(distinct column) calculates number of rows with distinct values in this column, excluding *NULL* values. Please note that  COUNT(distinct column1, column2) returns *0* if all values of one of the columns are *NULL*, even if the other column contains distinct *non-NULL* values.

3\. **【强制】** When all values of one column are *NULL*, COUNT(column) returns *0*, while SUM(column) returns *NULL*, so pay attention to `NullPointerException` issue when using SUM().    
> <font color="#019858">正例：</font>NPE issue could be avoided in this way:   
*SELECT IF(ISNULL(SUM(g)), 0, SUM(g)) FROM table;*

4\. **【强制】** Use *ISNULL()* to check *NULL* values. Result will be *NULL* when comparing *NULL* with any other values.   
> <font color="#977C00">注意：</font>  
&emsp;&emsp;1) *NULL<>NULL* returns *NULL*, rather than *false*.  
&emsp;&emsp;2) *NULL=NULL* returns *NULL*, rather than *true*.  
&emsp;&emsp;3) *NULL<>1* returns *NULL*, rather than *true*.

5\. **【强制】** When coding on DB query with paging logic, it should return immediately once count is *0*, to avoid executing paging query statement followed.

6\. **【强制】** *Foreign key* and *cascade update* are not allowed. All foreign key related logic should be handled in application layer.  
> <font color="#977C00">注意：</font> e.g. Student table has *student_id* as primary key, score table has *student_id* as foreign key. When *student.student_id* is updated, *score.student_id update* is also triggered, this is called a *cascading update*. *Foreign key* and *cascading update* are suitable for single machine, low parallel systems, not for distributed, high parallel cluster systems. *Cascading updates* are strong blocked, as it may lead to a DB update storm. *Foreign key* affects DB insertion efficiency.

7\. **【强制】** Stored procedures are not allowed. They are difficult to debug, extend and not portable.

8\. **【强制】** When correcting data, delete and update DB records, *SELECT* should be done first to ensure data correctness.

9\. **【推荐】** *IN* clause should be avoided. Record set size of the *IN* clause should be evaluated carefully and control it within 1000, if it cannot be avoided.

10\. **【参考】** For globalization needs, characters should be represented and stored with *UTF-8*, and be cautious of character number counting.  
> <font color="#977C00">注意：</font>
 SELECT LENGTH("轻松工作"); returns *12*.   
 SELECT CHARACTER_LENGTH("轻松工作"); returns *4*.   
Use *UTF8MB4* encoding to store emoji if needed, taking into account of its difference from *UTF-8*.

11\. **【参考】** *TRUNCATE* is not recommended when coding, even if it is faster than *DELETE* and uses less system, transaction log resource. Because *TRUNCATE* does not have transaction nor trigger DB *trigger*, problems might occur.  
> <font color="#977C00">注意：</font>In terms of Functionality, *TRUNCATE TABLE* is similar to *DELETE* without *WHERE* sub-clause.

### <font color="green">ORM Rules</font>
1\. **【强制】** Specific column names should be specified during query, rather than using \*.    
 > <font color="#977C00">注意：</font>
1) \* increases parsing cost.  
2) It may introduce mismatch with *resultMap* when adding or removing query columns.  

2\. **【强制】** Name of *Boolean* property of *POJO* classes cannot be prefixed with *is*, while DB column name should prefix with *is*. A mapping between properties and columns is required.  
> <font color="#977C00">注意：</font>Refer to rules of *POJO* class and DB column definition, mapping is needed in *resultMap*. Code generated by *MyBatis Generator* might need to be adjusted.

3\. **【强制】** Do not use *resultClass* as return parameters, even if all class property names are the same as DB columns, corresponding DO definition is needed.  
> <font color="#977C00">注意：</font><resultMap>Mapping configuration is needed, to decouple DO definition and table columns, which in turn facilitates maintenance.

4\. **【强制】** Be cautious with parameters in xml configuration. Do not use `${}` in place of `#{}`, `#param#`. SQL injection may happen in this way.

5\. **【强制】** *iBatis* built in *queryForList(String statementName, int start, int size)* is not recommended.  
> <font color="#977C00">注意：</font>It may lead to *OOM* issue because its implementation is to retrieve all DB records of statementName's corresponding SQL statement, then start, size subset is applied through subList.   
> <font color="#019858">正例：</font>Use *#start#*, *#size#* in *sqlmap.xml*.
```java
Map<String, Object> map = new HashMap<String, Object>();  
map.put("start", start);  
map.put("size", size);  
```

6\. **【强制】** Do not use *HashMap* or *HashTable* as DB query result type.

7\. **【强制】** *gmt_modified* column should be updated with current timestamp simultaneously with DB record update.

8\. **【推荐】** Do not define a universal table updating interface, which accepts POJO as input parameter, and always update table set *c1=value1, c2=value2, c3=value3, ...* regardless of intended columns to be updated. It is better not to update unrelated columns, because it is error prone, not efficient, and increases *binlog* storage.

9\. **【参考】** Do not overuse *@Transactional*. Because transaction affects QPS of DB, and relevant rollbacks may need be considered, including cache rollback, search engine rollback, message making up, statistics adjustment, etc.

10\. **【参考】** *compareValue* of *\<isEqual>* is a constant (normally a number) which is used to compared with property value. *\<isNotEmpty>* means executing corresponding logic when property is not empty and not null. *\<isNotNull>* means executing related logic when property is not null.

## <font color="green">4. Project Specification</font>
### <font color="green">Application Layers</font>

1\. **【推荐】** The upper layer depends on the lower layer by default. Arrow means direct dependent. For example: Open Interface can depend on Web Layer, it can also directly depend on Service Layer, etc.:

![](layers.png)

- **Open Interface:** In this layer service is encapsulate to be exposed as RPC interface, or HTTP interface through Web Layer; The layer also implements gateway security control, flow control, etc.  
- **View:** In this layer templates of each terminal render and execute. Rendering approaches include velocity rendering, JS rendering, JSP rendering, and mobile display, etc.  
- **Web Layer:** This layer is mainly used to implement forward access control, basic parameter verification, or  non-reusable services.   
- **Service Layer:** In this layer concrete business logic is implemented.   
- **Manager Layer:** This layer is the common business process layer, which contains the following features:  
&emsp;&emsp;1) Encapsulates third-party service, to preprocess return values and exceptions;  
&emsp;&emsp;2) The precipitation of general ability of Service Layer, such as caching solutions, middleware general processing;  
&emsp;&emsp;3) Interacts with the *DAO layer*, including composition and reuse of multiple *DAO* classes.  
- **DAO Layer**: Data access layer, data interacting with MySQL, Oracle and HBase.  
- **External interface or third-party platform:** This layer includes RPC open interface from other departments or companies.   


2\. **【参考】** Many exceptions in the *DAO Layer* cannot be caught by using a fine-grained exception class. The recommended way is to use `catch (Exception e)`, and `throw new DAOException(e)`. In these cases, there is no need to print the log because the log should have been caught and printed in *Manager Layer/Service Layer*.  
&emsp;&emsp; Logs about exception in *Service Layer* must be recorded with as much information about the parameters as possible to make debugging simpler.   
&emsp;&emsp; If *Manager Layer* and *Service Layer* are deployed in the same server, log logic should be consistent with *DAO Layer*. If they are deployed separately, log logic should be consistent with each other.  
 &emsp;&emsp; In *Web Layer* Exceptions cannot be thrown, because it is already on the top layer and there is no way to deal with abnormal situations. If the exception is likely to cause failure when rendering the page, the page should be redirected to a friendly error page with the friendly error information.  
&emsp;&emsp; In *Open Interface* exceptions should be handled by using *error code* and *error message*.

3\. **【参考】** Layers of Domain Model:  

- **DO (Data Object):** Corresponding to the database table structure, the data source object is transferred upward through *DAO* Layer.
- **DTO (Data Transfer Object):** Objects which are transferred upward by *Service Layer* and Manager Layer.
- **BO (Business Object):** Objects that encapsulate business logic, which can be outputted by *Service Layer*.
- **Query**: Data query objects that carry query request from upper layers. 注意：Prohibit the use of `Map` if there are more than 2 query conditions.
- **VO (View Object):** Objects that are used in *Display Layer*, which is normally transferred from *Web Layer*.


### <font color="green">Library Specification</font>
1\. **【强制】** Rules of defining GAV:     
&emsp;&emsp;1) **<font color="blue">G</font>**roupID: `com.{company/BU}.{business line}.{sub business line}`, at most 4 levels   
> <font color="#977C00">注意：</font>{company/BU} for example: such business unit level as Alibaba, Taobao, Tmall, Aliexpress and so on; sub-business line is optional.   
> <font color="#019858">正例：</font>com.taobao.tddl &emsp;&emsp;com.alibaba.sourcing.multilang   

&emsp;&emsp;2) **<font color="blue">A</font>**rtifactID: Product name - module name.  
> <font color="#019858">正例：</font>`tc-client` / `uic-api` / `tair-tool`  

&emsp;&emsp;3) **<font color="blue">V</font>**ersion: Please refer to below  

2\. **【强制】** Library naming convention: `prime version number`.`secondary version number`.`revision number`  
&emsp;&emsp;1) **prime version number**: Need to change when there are incompatible API modification, or new features that can change the  product direction.   
&emsp;&emsp;2) **secondary version number**: Changed for backward compatible modification.  
&emsp;&emsp;3) **revision number**: Changed for fixing bugs or adding features that do not modify the method signature and maintain API compatibility.  
 > <font color="#977C00">注意：</font>The initial version must be <font color="blue">1.0.0</font>, rather than <font color="red">0.0.1</font>.

3\. **【强制】** Online applications should not depend on *SNAPSHOT* versions (except for security packages); Official releases must be verified from central repository, to make sure that the RELEASE version number has continuity. Version numbers are not allowed to be overridden.  
> <font color="#977C00">注意：</font>Avoid using *SNAPSHOT* is to ensure the idempotent of application release. In addition, it can speed up the code compilation when packaging. If the current version is *1.3.3*, then the number for the next version can be *1.3.4*, *1.4.0* or *2.0.0*.

4\. **【强制】** When adding or upgrading libraries, remain the versions of dependent libraries unchanged if not necessary. If there is a change, it must be correctly assessed and checked. It is recommended to use command *dependency:resolve* to compare the information before and after. If the result is identical, find out the differences with the command *dependency:tree* and use *\<excludes\>* to eliminate unnecessary libraries.

5\. **【强制】** Enumeration types can be defined or used for parameter types in libraries, but cannot be used for interface return types (POJO that contains enumeration types is also not allowed).

6\. **【强制】** When a group of libraries are used, a uniform version variable need to be defined to avoid the inconsistency of version numbers.  
> <font color="#977C00">注意：</font>When using `springframework-core`, `springframework-context`, `springframework-beans` with the same version, a uniform version variable \${spring.version} is recommended to be used.

7\. **【强制】** For the same *GroupId* and *ArtifactId*, *Version* must be the same in sub-projects.  
> <font color="#977C00">注意：</font>During local debugging, version number specified in sub-project is used. But when merged into a war, only one version number is applied in the lib directory. Therefore, such case might occur that offline debugging is correct, but failure occurs after online launch.

8\. **【推荐】** The declaration of dependencies in all *POM* files should be placed in *\<dependencies\>* block. Versions of dependencies should be specified in *\<dependencyManagement\>* block.   
> <font color="#977C00">注意：</font>In *\<dependencyManagement\>* versions of dependencies are specified, but dependencies are not imported. Therefore, in sub-projects dependencies need to be declared explicitly, version and scope of which are read from the parent *POM*. All declarations in *\<dependencies\>* in the main *POM* will be automatically imported and inherited by all subprojects by default.

9\. **【推荐】** It is recommended that libraries do not include configuration, at least do not increase the configuration items.

10\. **【参考】** In order to avoid the dependency conflict of libraries, the publishers should follow the principles below:  
&emsp;&emsp;1) **Simple and controllable:**  Remove all unnecessary API and dependencies, only contain Service API, necessary domain model objects, Utils classes, constants, enumerations, etc. If other libraries must be included, better to make the scope as *provided* and let users to depend on the specific version number. Do not depend on specific log implementation, only depend on the log framework instead.  
&emsp;&emsp;2) **Stable and traceable:** Change log of each version should be recorded. Make it easy to check the library owner and where the source code is. Libraries packaged in the application should not be changed unless the user updates the version proactively.

### <font color="green">Server Specification</font>
1\. **【推荐】** It is recommended to reduce the *time_wait* value of the *TCP* protocol for high concurrency servers.  
> <font color="#977C00">注意：</font> By default the operating system will close connection in *time_wait* state after 240 seconds. In high concurrent situation, the server may not be able to establish new connections because there are too many connections in *time_wait* state, so the value of *time_wait* needs to be reduced.  
> <font color="#019858">正例：</font>Modify the default value (Sec) by modifying the *_etcsysctl.conf* file on Linux servers:
`net.ipv4.tcp\_fin\_timeout = 30`  

2\. **【推荐】** Increase the maximum number of *File Descriptors* supported by the server.  
> <font color="#977C00">注意：</font>Most operating systems are designed to manage *TCP/UDP* connections as a file, i.e. one connection corresponds to one *File Descriptor*. The maximum number of *File Descriptors* supported by the most Linux servers is 1024. It is easy to make an "open too many files" error because of the lack of *File Descriptor* when the number of concurrent connections is large, which would cause that new connections cannot be established.

3\. **【推荐】** Set `-XX:+HeapDumpOnOutOfMemoryError` parameter for *JVM*, so *JVM* will output dump information when *OOM* occurs.  
> <font color="#977C00">注意：</font>*OOM* does not occur very often, only once in a few months. The dump information printed when error occurs is very valuable for error checking.

4\. **【参考】** Use *forward* for internal redirection and URL assembly tools for external redirection. Otherwise there will be problems about URL maintaining inconsistency and potential security risks.

## <font color="green">5. Security Specification</font>

1\. **【强制】** User-owned pages or functions must be authorized.  
> <font color="#977C00">注意：</font>Prevent the access and manipulation of other people's data without authorization check, e.g. view or modify other people's orders.

2\. **【强制】** Direct display of user sensitive data is not allowed. Displayed data must be desensitized.  
> <font color="#977C00">注意：</font>Personal phone number should be displayed as: 158\*\*\*\*9119. The middle 4 digits are hidden to prevent privacy leaks.

3\. **【强制】** *SQL* parameter entered by users should be checked carefully or limited by *METADATA*, to prevent *SQL* injection. Database access by string concatenation *SQL* is forbidden.  

4\. **【强制】** Any parameters input by users must go through validation check.  
> <font color="#977C00">注意：</font>Ignoring parameter check may cause:
>
 * memory leak because of excessive page size
 * slow database query because of malicious *order by*
 * arbitrary redirection
 * *SQL* injection
 * deserialize injection
 * *ReDoS*    

> <font color="#977C00">注意：</font>In Java *regular expressions* is used to validate client input. Some *regular expressions* can validate common user input without any problem, but it could lead to a dead cycle if the attacker uses a specially constructed string to verify.

5\. **【强制】** It is forbidden to output user data to *HTML* page without security filtering or proper escaping.  

6\. **【强制】** Form and *AJAX* submission must be filtered by *CSRF* security check.  
> <font color="#977C00">注意：</font>*CSRF* (Cross-site Request Forgery) is a kind of common programming flaw. For applications/websites with *CSRF* leaks, attackers can construct *URL* in advance and modify the user parameters in database as long as the victim user visits without notice.

7\. **【强制】** It is necessary to use the correct anti-replay restrictions, such as number restriction, fatigue control, verification code checking, to avoid abusing of platform resources, such as text messages, e-mail, telephone, order, payment.   
> <font color="#977C00">注意：</font>For example, if there is no limitation to the times and frequency when sending verification codes to mobile phones, users might be bothered and *SMS* platform resources might be wasted.

8\. **【推荐】** In scenarios when users generate content (e.g., posting, comment, instant messages), anti-scam word filtering and other risk control strategies must be applied.
