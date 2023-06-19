

[TOC]

### 一、Java

#### 1.1 JDK跟JRE有什么区别

- JDK是Java开发工具包，它包含了完整的Java开发环境。JDK提供了编译、调试和运行Java程序所需的工具和库。它包含了Java编译器（javac）用于将Java源代码编译为字节码文件，以及Java虚拟机（JVM）用于执行字节码文件。此外，JDK还提供了其他开发工具，如调试器（jdb）、应用程序打包工具（jar）和文档生成工具（javadoc）等。
- JRE是Java运行时环境，它是在运行Java程序时所需的最小环境。JRE包含了Java虚拟机（JVM）和Java类库，可以执行Java字节码并提供程序运行所需的基本功能。它不包含开发工具，因此无法进行Java程序的编译和调试。
- JDK**包含了JRE**，因此如果你安装了JDK，就已经包含了JRE的功能。如果你只是想运行Java程序而不进行开发，那么安装JRE就足够了。

#### 1.2 String类的常用方法

1. equals(str): 比较字符串是否相等（内容相同）。

2. substring(beginIndex, endIndex): 返回从指定开始索引到结束索引之间的子字符串。

3. charAt(index): 返回指定索引处的字符。

4. split(String regex): 根据正则表达式来分割字符串，并返回分割后的字符串数组。

5. 如果你只需要将字符串拆分为单个字符，可以使用String类的toCharArray()方法：

   ```java
   String str = "Hello";
   char[] chars = str.toCharArray(); // 返回['H', 'e', 'l', 'l', 'o']
   ```

6. replace(char oldChar, char newChar): 将字符串中所有的指定字符 oldChar 替换为新的字符 newChar。

   ```java
   String str = "Hello World";
   String newStr = str.replace('o', 'a'); // 返回 "Hella Warld"
   ```

7. replace(CharSequence target, CharSequence replacement): 将字符串中所有的指定字符序列 target 替换为新的字符序列replacement。

   ```java
   String str = "Hello World";
   String newStr = str.replace("World", "Universe"); // 返回 "Hello Universe"
   ```

8. toLowerCase(): 将字符串转换为小写形式。

9. toUpperCase(): 将字符串转换为大写形式。

10. trim()方法来去除字符串首尾的空格。

11. 如果你想去除字符串内部的所有空格，可以使用replaceAll()方法配合正则表达式来实现,在正则表达式中，"\s" 是用来匹配空白字符的特殊字符组合，包括空格、制表符、换行符等。

    ```java
    String str = "   Hello   World   ";
    String trimmedStr = str.replaceAll("\\s", ""); // 返回 "HelloWorld"
    
    ```

#### 1.3 有没有用过BigDecimal类型? 什么场景? 加减乘除比较?

BigDecimal 类型在处理精确的十进制数值计算时非常有用。它提供了高精度的数值运算和控制，适用于需要精确计算的场景，如金融、货币计算、税务计算等。

- 加法（add）：

  ```java
  BigDecimal num1 = new BigDecimal("10.25");
  BigDecimal num2 = new BigDecimal("5.75");
  BigDecimal sum = num1.add(num2); // 返回 16.00
  ```

- 减法（subtract）

- 乘法（multiply）

- 除法（divide）

- 比较（compareTo）：

  ```java
  BigDecimal num1 = new BigDecimal("10.25");
  BigDecimal num2 = new BigDecimal("5.75");
  int comparison = num1.compareTo(num2); // 返回 1（num1 > num2）
  ```

#### 1.4 为什么会出现浮点数的舍入误差

浮点数的舍入误差是由于二进制和十进制之间的转换以及浮点数的内部表示方式引起的。浮点数在计算机中以二进制形式表示，而十进制数无法精确地转换为二进制表示，因此在进行浮点数运算时可能会引起舍入误差。

例如：0.1 + 0.2 不等于 0.3

```java
double result = 0.1 + 0.2;
System.out.println(result); // 输出 0.30000000000000004
```

#### 1.5 ==和equals区别，使用equals的注意事项?

- "==" 运算符用于比较两个对象的引用是否相等。它比较的是对象在内存中的地址，即对象是否指向同一个内存位置。例如：

  ```java
  String str1 = "Hello";
  String str2 = "Hello";
  String str3 = new String("Hello");
  System.out.println(str1 == str2); // 输出 true
  System.out.println(str1 == str3); // 输出 false
  //在上述示例中，str1 和 str2 引用的是同一个字符串常量，所以它们的地址相同，而 str3 引用的是通过 new 创建的新对象，所以其地址与 str1 不同。
  ```

- "equals()" 方法用于比较两个对象的内容是否相等。它比较的是对象的内容，即根据对象的具体实现来判断是否相等。通常情况下，equals() 方法被子类重写以提供自定义的相等比较逻辑。例如：

  ```java
  String str1 = "Hello";
  String str2 = "Hello";
  String str3 = new String("Hello");
  System.out.println(str1.equals(str2)); // 输出 true
  System.out.println(str1.equals(str3)); // 输出 true
  //在上述示例中，虽然 str1 和 str2 引用的是不同的对象，但它们的内容是相同的，所以通过 equals() 方法比较返回 true。同样，str1 和 str3 的内容也相同，所以 equals() 方法比较同样返回 true。
  ```

- 注意事项：

  重写 equals() 方法时通常需要同时重写 hashCode() 方法，以保持 hashCode() 和 equals() 之间的一致性。如果两个对象根据 equals() 方法比较相等，则它们的 hashCode() 方法应返回相同的哈希码。

#### 1.6 什么是哈希码，哈希表？

对象的哈希码是一个整数值，用于标识对象的散列值。它是根据对象的内部状态计算而来的，用于快速定位对象在哈希表等数据结构中的位置。对象的哈希码在创建对象时计算，并在对象的生命周期中保持不变（前提是对象的内部状态不发生变化）。

哈希表是一种常见的数据结构，用于实现快速查找和存储。它基于哈希函数（hash function）将键（key）映射到特定的存储位置，称为哈希桶（hash bucket）或槽（slot）。哈希表使用哈希码来确定对象在哈希桶中的位置，从而实现高效的数据访问。

例：考虑一个存储学生信息的系统，每个学生对象包含姓名和学号两个属性。为了快速查找学生信息，可以使用哈希表来存储学生对象。

1. 哈希码的计算：
   - 假设学生对象的哈希码计算规则为将学号转换为整数作为哈希码。
   - 学生对象 "Alice"，学号为 "1001"，则它的哈希码为 1001。
   - 学生对象 "Bob"，学号为 "2002"，则它的哈希码为 2002。
   - 以此类推，根据学号计算出每个学生对象的哈希码。
2. 哈希表的使用：
   - 假设使用一个长度为 100 的哈希表来存储学生对象。
   - 将每个学生对象的哈希码与哈希表的长度取模，得到一个索引值。
   - 将学生对象存储在对应索引值的哈希桶中。
   - 当需要查找学生信息时，根据学号计算出哈希码，根据哈希码找到对应的哈希桶，从而快速定位到学生对象。

#### 1.7 throw 和 throws 的区别?

throw：

- "throw" 关键字用于在程序中手动抛出一个异常对象。

- 当使用 throw 关键字时，需要提供一个异常对象作为参数，该异常对象可以是 Java 内置的异常类（如 IllegalArgumentException、NullPointerException 等），也可以是自定义的异常类。

- throw 关键字通常用于在方法内部检测到错误或异常情况，并主动抛出异常，使得程序的执行流程中断，并将控制权交给异常处理机制。

  ```java
  public void divide(int dividend, int divisor) {
      if (divisor == 0) {
          throw new ArithmeticException("Divisor cannot be zero");
      }
      int result = dividend / divisor;
      System.out.println("Result: " + result);
  }
  ```

throws：

- "throws" 关键字用于在方法声明中指定该方法可能抛出的异常类型。

- 当一个方法可能抛出异常时，可以使用 throws 关键字在方法声明中列出可能的异常类型，以通知调用者需要处理这些异常。

- throws 关键字后面跟着一个异常类或多个异常类，用逗号分隔。

- 如果一个方法声明使用了 throws 关键字，那么在方法的实现中，必须要处理或进一步传递这些异常。

  ```java
  public void readFile() throws FileNotFoundException, IOException {
      // 读取文件的代码逻辑
      // 可能抛出 FileNotFoundException 或 IOException 异常
  }
  ```

  总结：

  - throw 关键字用于手动抛出一个异常对象，用于在方法内部主动触发异常。
  - throws 关键字用于在方法声明中指定可能抛出的异常类型，用于通知调用者需要处理这些异常。
  - throw 关键字是在方法内部使用，而 throws 关键字是在方法声明中使用。
  - throw 关键字后面跟着一个异对象，而 throws 关键字后面跟着异常类或多个异常类。

#### 1.8 try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗?

在 try-catch-finally 块中，当 catch 块中执行了 return 语句时，finally 块仍然会执行。不论是否发生异常，finally 块中的代码都会执行，除非在执行 finally 块之前发生了 System.exit() 方法调用或虚拟机崩溃。

注意事项：

- 如果在 finally 块中也包含了 return 语句，那么最终的返回值将是 finally 块中的返回值，而不是 catch 块中的返回值。
- finally 块通常用于确保资源的释放，例如关闭打开的文件或释放数据库连接，在无论是否发生异常的情况下都能保证资源的正确释放。

#### 1.9 常见哪些异常?

1. NullPointerException（空指针异常）：当使用空对象的引用调用方法或访问属性时抛出。
2. IllegalArgumentException（非法参数异常）：当传递给方法的参数不符合方法的预期时抛出。
3. IllegalStateException（非法状态异常）：当对象的状态不适合执行特定操作时抛出。
4. IndexOutOfBoundsException（索引越界异常）：当使用无效的索引访问数组、字符串或集合时抛出。
5. ArrayIndexOutOfBoundsException（数组索引越界异常）：当使用无效的数组索引访问数组元素时抛出。
6. StringIndexOutOfBoundsException（字符串索引越界异常）：当使用无效的索引访问字符串中的字符时抛出。
7. ArithmeticException（算术异常）：当在算术运算中发生错误时抛出，例如除以零。
8. ClassCastException（类转换异常）：当尝试将一个对象强制转换为与其类型不兼容的类时抛出。
9. FileNotFoundException（文件未找到异常）：当尝试打开不存在的文件时抛出。
10. IOException（输入输出异常）：当处理输入输出操作时发生错误时抛出，例如读写文件或网络连接时。

#### 1.10 json/xml转对象方式?

- 将 JSON 数据转换为对象使用 Fastjson 库,使用 Fastjson 的静态方法 parseObject将 JSON 字符串转换为 Java 对象。Fastjson 还支持将对象转换为 JSON 字符串的功能，你可以使用 toJSONString`方法实现。

- 使用 JAXB 将 XML 转换为对象的示例：

  ```java
  JAXBContext jaxbContext = JAXBContext.newInstance(Person.class);
  Unmarshaller unmarshaller = jaxbContext.createUnmarshaller();
  String xmlString = "<person><name>John</name><age>30</age></person>";
  StringReader reader = new StringReader(xmlString);
  try {
      Person person = (Person) unmarshaller.unmarshal(reader);
      System.out.println(person.getName()); // 输出: John
      System.out.println(person.getAge()); // 输出: 30
  } catch (JAXBException e) {
      e.printStackTrace();
  }
  ```

#### 1.11 接口/抽象类区别?

1. 定义和用途：
   - 接口：接口是一种完全抽象的类，它只定义了方法的签名（方法名称、参数和返回类型），没有方法的实现。接口用于定义类应该具备的行为和功能，以及类与类之间的契约。
   - 抽象类：抽象类是一个类，它可以包含抽象方法（没有实现的方法）和具体方法（有实现的方法）。抽象类用于创建一个类的模板，它可以提供一些通用的实现，同时也允许子类根据需要进行扩展和实现。
2. 继承关系：
   - 接口：一个类可以实现（implements）多个接口，通过实现接口，类可以遵循接口定义的规范，并提供接口中声明的方法的实现。
   - 抽象类：一个类只能继承（extends）一个抽象类，通过继承抽象类，子类可以继承抽象类的属性和方法，并根据需要实现抽象方法。
3. 方法实现：
   - 接口：接口中的方法没有实现体，实现接口的类必须提供方法的具体实现。
   - 抽象类：抽象类中可以包含具体方法的实现，子类可以直接继承这些具体方法，无需重新实现，但必须实现抽象方法。
4. 构造函数：
   - 接口：接口不能拥有构造函数，因为接口无法实例化。
   - 抽象类：抽象类可以拥有构造函数，用于初始化抽象类的实例。
5. 成员变量：
   - 接口：接口中只能包含常量（public static final）变量，即接口中的字段都是静态和不可修改的。
   - 抽象类：抽象类可以包含各种类型的成员变量，包括实例变量和静态变量。
6. 多态性：
   - 接口：通过接口可以实现多态性，一个对象可以引用实现了同一个接口的不同类的实例。
   - 抽象类：抽象类也可以用于实现多态性，一个对象可以引用抽象类的子类的实例。

总的来说，接口用于定义类的行为规范，强调类与类之间的协作关系；而抽象类用于创建类的模板，强调类的继承关系和共享的属性和行为。在设计类和接口时，需要根据具体的需求和设计目标来选择使用接口还是抽象类。

#### 1.12 final/finally 在 Java 中有什么作用？

1. final 关键字：

   final 可以用来修饰类、方法和变量。
   当用于修饰类时，表示该类是最终的，不可被继承。
   当用于修饰方法时，表示该方法是最终的，不可被子类重写。
   当用于修饰变量时，表示该变量是一个常量，只能被赋值一次，无法修改其值。
   finally 关键字：

   finally 是一个关键字，用于定义一个代码块，在异常处理时提供一种确保某些代码总是执行的机制。
   finally 块中的代码无论是否发生异常，都会被执行。
   finally 常用于资源释放，例如关闭打开的文件、释放数据库连接等。无论是否发生异常，finally 块中的代码都能保证执行，以确保资源的正确释放。
   以下是示例代码，演示了 final 和 finally 的用法：

   ```java
   // final 关键字示例
   public final class MyClass { // final 修饰类，表示该类不可被继承
       public final int myMethod() { // final 修饰方法，表示该方法不可被重写
           final int x = 10; // final 修饰变量，表示该变量是一个常量
           // x = 20; // 编译错误，无法修改常量的值
           return x;
       }
   }
   
   // finally 关键字示例
   public class Main {
       public static void main(String[] args) {
           try {
               int result = divide(10, 0); // 调用一个除法方法，可能发生除以零的异常
               System.out.println(result);
           } catch (ArithmeticException e) {
               System.out.println("除法运算异常");
           } finally {
               System.out.println("finally 块中的代码");
           }
       }
       
       public static int divide(int dividend, int divisor) {
           try {
               return dividend / divisor;
           } finally {
               System.out.println("finally 块中的代码，无论是否发生异常都会执行");
           }
       }
   }
   //在上述代码中，MyClass 类被声明为 final，表示不可被继承。myMethod 方法被声明为 final，表示不可被重写。在 Main 类中，divide 方法中的 finally 块中的代码无论是否发生异常都会执行。
   //总结：final 关键字用于修饰类、方法和变量，提供了不可修改的特性；finally 关键字用于定义一个代码块，在异常处理时提供确保执行的机制。
   ```


#### 1.13 集合

- List集合：

  List 是 Collection 子接口，最大的特点是允许保存有重复数据

  List本身也是一个接口，对于接口要想使用一定要使用它的子类来完成具体的操作，在List 子接口有三个常用子类：ArrayList、Vector、LinkedList。

  **ArrayList 子类：**

  |                           代码演示                           |
  | :----------------------------------------------------------: |
  | ![img](https://img-blog.csdnimg.cn/006e6619a89f42aa8b6e9bc172b95284.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) |
  |                           **概述**                           |
  | ![img](https://img-blog.csdnimg.cn/e197c931b20d4c3a8fddd461d0b84226.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) |

  **LinkedList子类：**在List接口里面还有另外一个比较常用的子类：LinkedList，这个类通过名称就可以发现它的特点：基于链表的实现

  |                           代码展示                           |
  | :----------------------------------------------------------: |
  | ![img](https://img-blog.csdnimg.cn/1febb1c6912b4401895e175bda419c97.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) |
  |                           **概述**                           |
  | ![img](https://img-blog.csdnimg.cn/99d2560ecfba416c8c4d42467290b4be.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==) |

   **Vector子类：**

  

  ![img](https://img-blog.csdnimg.cn/8caad91abeb546d2a699cb3e021bf595.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

   ![img](https://img-blog.csdnimg.cn/4e0c2ce1945d4d8ea52e128c464fd983.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

  

  

  

  **set集合**

  Set集合最大的特点就是不允许保存重复元素，也是Collection的子接口。

  需要注意的是Set集合并不像List集合那样扩充了许多新的方法，所以无法使用 List 集合中提供的 get()方法，也就是说它无法实现指定索引获取数据

  

  **HashSet子类:**HashSet是Set接口里面使用最多的一个子类，最大的特点就是保存的数据是无序的

  ![img](https://img-blog.csdnimg.cn/430f9ca2e456407ab513cc2333e57d6a.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

  

  **TreeSet:**

  Set接口的另外一个子类就是 TreeSet，与HashSet最大的区别在于Treeset集合里面保存的数据是有序的。

  TreeSet 子类之中保存的数据是允许排序的，但是这个类必须要实现Comparable接口，只有实现了此接口才能确认出对象的大小关系。

  提示：TreeSet本质是利用 TreeMap子类实现的集合数据的存储，而TreeMap（树）需要根据Comparable来确定大小关系

  

  

  

  **集合输出**

  ![img](https://img-blog.csdnimg.cn/6f1c33aaea9e4d17be2c37887be37d4c.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  

  

  ![img](https://img-blog.csdnimg.cn/fd5f35fefe194bd0a4552f775723dd99.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  ![img](https://img-blog.csdnimg.cn/46bc0513f1394af183f19b03ac4c9cc5.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

  

  ![img](https://img-blog.csdnimg.cn/48b6c0b9d62f42b1bf2afcafd0a50fdd.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  

  ![img](https://img-blog.csdnimg.cn/e5ecad28f6e143d69a0eeaeaf69fdaea.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  

  

  ## Map 集合:key value

  

  ![img](https://img-blog.csdnimg.cn/5d89c826d19c4b38b3226cf65023699e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  

   **HashMap子类：**HashMap是Map接口中最为常见的一个子类，这个类主要特点就是无序存储

  ![img](https://img-blog.csdnimg.cn/9310cbbd99cd4d0388be3956be10fd3e.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

  通过 HashMap 实例化的Map接口可以针对于key和value保存null数据，同时也可以发现即便保存数据的key有重复也不会发生错误，而是出现内容的替换。

  但是对于Map接口提供的put()方法本身是提供有返回值的，那么这个返回值指的是在重复key的情况下返回旧的value。

   在设置了相同的key的内容的时候put()方法会返回原来的数据。

  ![img](https://img-blog.csdnimg.cn/7152a5fc3f6d4a46a51f3af472cad5e9.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  ![img](https://img-blog.csdnimg.cn/bb92102be13d467b8d5c7dc69637af15.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑

  

  ![img](https://img-blog.csdnimg.cn/9574116e5f61464ca3dcc98109539167.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)编辑 使![img](https://img-blog.csdnimg.cn/ef5a7cb223b844d286e5e05b8b0fb8f4.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

  ![img](https://img-blog.csdnimg.cn/1cbc29ad0b09448b88b82bcb610ae082.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### 1.14 数组的定义和使用

**定义数组：**

```java
// 一维数组
<元素类型>[] <数组名> = new <元素类型>[<数组长度>];

// 多维数组
<元素类型>[][] <数组名> = new <元素类型>[<行数>][<列数>];

```

```java
int[] numbers = new int[5];
int[][] matrix = new int[2][3];
```

**初始化数组：**

```java
// 一维数组初始化
<数组名>[<索引>] = <值>;

// 多维数组初始化
<数组名>[<行索引>][<列索引>] = <值>;
```

```java
numbers[0] = 10;
matrix[1][2] = 5;
```

**访问数组元素：**

```java
// 一维数组访问
<数组名>[<索引>];
// 多维数组访问
<数组名>[<行索引>][<列索引>];
```

```java
int firstNumber = numbers[0];
int value = matrix[1][2];
```

**数组长度：**

```
<数组名>.length;
int length = numbers.length;
```

