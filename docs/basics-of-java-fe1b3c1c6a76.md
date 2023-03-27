# Java 基础

> 原文：<https://medium.com/swlh/basics-of-java-fe1b3c1c6a76>

Java 入门时的一些基本概念！

0.印刷材料

```
System.out.println(statement);
```

将语句输出到控制台。这是 puts/prints/console.log 的 Java 版本。

1.  变量

变量使用 camel case 编写。

```
variable
variableTwo
```

当声明一个变量时，你应该包括它的数据类型。有 8 种基本的数据类型，一个字节(介于-128 到 127 之间的 8 位有符号整数)，一个短整型(介于-32，768 到 32，767 之间的 16 位有符号整数)，一个整型(介于-2 到 2 -1 之间的 32 位有符号整数)，一个长整型(介于-2⁶到 2⁶ -1 之间的 64 位有符号整数)，一个浮点型(十进制数)，一个双精度型(浮点型的较大版本)，一个布尔型(真/假)和一个字符型(单个字符)

```
int number = 10;
float secondNumber = 2.5f;
boolean result = true;
char letter = "C";
String sentence = "This is a sentence!";
```

*   Float 还要求数字末尾有一个 f
*   String 实际上不是原始数据类型之一，但是有来自 String 类的特殊支持。字符串变量是不可变的(声明后不能更改

声明变量时，如果声明时没有静态字段，则该变量是实例变量。

```
int speed = 5;
```

实例变量是属于一个类的一个特定实例的属性，在这种情况下，一个 sprinter 的速度可能是 5。

如果声明了 static，那么这个变量就是一个类变量。

```
static int allSprinters = 8;
```

类变量是属于整个类的变量，而不是属于单个实例的变量。在这种情况下，sprinter 类中的 sprinter 总数属于该类。

关于什么是类的更多内容将在后面介绍。

在声明一个变量时，可以包含 final 这个词，将它变成一个不可更改或重新分配的常量变量。

```
final int legs = 2;
```

*   (我的意思是，我想你可以从技术上改变这一点……)

2.数组

当你声明一个数组时，你必须声明它将包含的数据类型，然后你必须指定它的长度，这个长度是固定的。

```
int[] = newArray;
# declare newArray as an array of integersnewArray = new int[10];
# set the length of newArray to 10# can combine both steps
int[] newArray = new int[10];
```

数组中的每个元素都有一个从 0 开始的索引，可以通过键入 arrayName[indexNumber]来访问，也可以通过键入 arrayName[indexNumber] = value 来赋值。

所以在声明了一个数组和它的长度之后，你可以通过单独给每个索引号赋值来填充它。如果您最初知道这些值，就可以在声明数组时包含它们。

```
int[] = newArray;
int[] newArray = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};# or all at onceint[] newArray = new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
```

数组方法

*   arrayName.length 返回元素总数
*   要复制数组，请使用:

```
public static void arraycopy(array1, startPosition,
                             array2, startPosition2, copyAmount);int array1 = {1, 2, 3, 4, 5, 6, 7, 8}
int array2 = {10, 11, 12, 13, 14, 15, 16}public static void arraycopy(array1, 3, array2, 1, 2);
# would return {10, 4, 5, 13, 14, 15, 16}
```

3.经营者

算术运算符

标准=、+、-、*、/和%。

```
int number = 5;
1 + 1 = 2;
2 - 1 = 1;
2 * 2 = 4;
4 / 2 = 2;
5 % 2 = 1;% gives the remainder after diving the first number by the second
```

一元运算符

-, ++, --, !

```
int number = 5;
boolean variable2 = true;-number is -5
number++ is 6
number-- is 4!variable2 is false
```

你可以把++和-放在某物的前面或后面。如果您将它放在前面，则在值被其所属的表达式/公式计算之前，将会执行加法/减法，而如果您将它放在后面，则它将在计算之后递增。

等式和关系运算符

==(等于)，！=(不等于)、>(大于)、≥(大于或等于)、

```
1 == 1;
#true
1 == 5;
#false1 != 5;
#true
1 != 1;
#false5 > 1;
#true
1 > 5;
#false1 >= 1;
#true
1 >= 5;
#false1 < 5;
#true
5 < 1;
#false1 <= 5;
#true
5 <= 1;
#false
```

条件运算符

&&(和)，||(或)，？:(三元运算符)

```
(5 > 3) && (5 > 1)
#true
(5 > 3) && (5 > 7)
#false(5 > 3) || (5 > 1)
#true
(5 > 3) ||(5 > 7)
#true
(5 > 9) || (5 > 1)
#true
(5 > 9) ||(5 > 7)
#falseif (condition) ? result1 : result2
# the equivalent of if condition is true return result1, else return result2
```

4.表达式、语句、块

表达式非常宽泛，它只是数字、变量和运算符的组合。

```
1 + 2a * 5 / 2
```

语句基本上是一个带有。在它的结尾，表示它是一个完整的单位。

```
1 + 2;a * 2;
```

块是某个{}内部的语句

```
if (condition) {
   a * 2;
} else {
   block2;
}
```

5.控制流

控制流是决策语句(if、then、else、switch)、循环语句(for、while、do-while)和分支语句(break、continue、return)的使用。

我们在前面遇到了一个控制流的例子。

```
int number = 76
String resultif (number == 100) {
   result = "Genius!";
} else if (90) {
   result = "Good job!";
} else if (80) {
   result = "OK I guess";
} else if (65) {
   result = "Close call...";
} else {
   result = "That's a paddlin";
}
```

从上到下，将对每个条件进行评估。如果满足条件，将执行它后面的块。如果没有，它将继续前进，并评估下一个条件。如果指定的条件都不满足，它将执行“else”旁边的任何操作。

开关类似，但格式略有不同。

```
int number = 76
String result switch (number) {
   case 100: result = "Genius!";
             break;
   case 90: result = "Good job!";
             break;
   case 80: result = "OK I guess";
             break;
   case 65: result = "Close call...";
             break;
   default: result = "That's a paddlin";
             break;
}
```

将根据每种情况对()中的变量进行评估，如果值与该情况匹配，将执行“操作”(切换块)。即使在匹配了一个案例之后，所有后续案例都将被评估，直到遇到中断，因此中断在所有案例中都存在。默认情况下，当其他 case 语句都不匹配时，它类似于其他格式中的“else”。

While 循环

```
while (condition) {
   statement
}
```

当条件评估为真时，将运行该块。

Do-while 循环

```
do {
   statement
} while (expression)
```

类似于 while 循环，但 while 表达式是在语句运行后计算的，因此它总是至少运行一次。

For 循环

```
for (int i = 0; i < 10; i++) {
   statement
}
```

该循环用于执行该语句 10 次。I 是一个计数器变量，从 0 开始，旁边是何时停止执行循环的条件(本例中 i < 10)，再旁边是每次迭代后做什么(本例中增量 I)。这使得在每一次循环后，I 将增加 1，当它达到 10 时，i < 10 将为假，它将停止运行。

“break”将中断循环，“continue”将跳到计算运行循环的表达式的部分，而“return”将退出当前方法并返回到被调用时的状态。

6.班级

声明一个类是:

```
modifier class ClassName extends ParentClass implements Interface{
   field, constructor, methods
}
```

修饰符是 public(其他类可访问的字段)或 private(只有自己的类中的字段可访问)如果当前类从不同的类继承，父类将被包括在内，并且您可以让它实现一个接口(这将在后面介绍)。

一个基本类是:

```
class Hero {
}
```

类中的成员变量称为字段，它们是类的任何实例都可以访问的变量。

```
modifier dataType name
```

他们将被列为班级的第一名。

```
public class Hero {
   public String name
   public int rating
   public int power
}
```

通常，成员变量被设置为 private，并添加 public 方法以便可以访问它们。

```
public class Hero {
   private String name;
   private int rating;
   private int power; public Hero(String name, int rating, int power) {
        name = name;
        rating = rating;
        power = power;
   } public string getName() {
      return name;
   } public int getRating() {
      return rating;
   } public int getPower() {
      return power;
   } public void increaseRating(number) {
      rating ++ number;
   } public void strengthen(number) {
      strengthen ++ number;
   }
}
```

字段变量之后的部分是构造函数。

```
public Hero(String name, int rating, int power) {
     name = name;
     rating = rating;
     power = power;
}
```

这允许我们创建一个新的英雄，它接受一个名字，一个等级和一个能力，用来创建一个具有这些属性的英雄的新实例。

```
Hero allMight = new Hero("All Might", 10, 10)
```

方法是用一个修饰符和它们返回的数据类型来声明的，如果它们不返回任何东西，就放 void。这里我们不能直接访问英雄的属性，但是我们有方法可以让我们获得和修改它们。

如果您不知道将要传入多少个参数，您可以使用 elipses(称为 varargs)。

```
public printTeam(teamName ...Heroes) {
   System.out.print("The team" + teamName + "consists of" + Heroes)printTeam("Justice League", "Superman")
# this will print out "The team Justice League consists of ['Superman']"printTeam("Justice League", "Superman", "Spiderman")
# this will print out "The team Justice League consists of ['Superman', 'Spiderman']"
```

Heroes 在 teamName 之后传递了所有参数，作为 Heroes 数组的一部分。

创建一个对象有三个部分。

*   声明(将变量名与对象类型相关联)

```
className variableNameHero allMight
```

*   实例化(使用 new 关键字为对象分配内存)，在初始化之后(调用构造函数)，

```
Hero allmight = new Hero("All Might", 10, 10)
```

您可以通过调用来访问对象字段

```
object.fieldNameallMight.name returns "All Might"
```

您可以使用以下方法调用对象上的方法:

```
object.methodName(arguments)allMight.strengthen(100)
# this will increase allMight's power value to 110allMight.power will now return 110
```

7.这

“这个”是什么？

*   在实例方法或构造函数中，这是指调用该方法的对象。

前面我们将构造函数写成:

```
public Hero(String name, int rating, int power) {
     name = name;
     rating = rating;
     power = power;
}
```

但是你也可以这样写:

```
public Hero(String name, int rating, int power) {
     this.name = name;
     this.rating = rating;
     this.power = power;
}
```

这里指的是那些参数中传递的对象，它的属性被设置为传递的参数。

*   在一个构造函数中，这可以用来调用同一个类中的另一个构造函数。这被称为“显式构造函数调用”。

```
public class hero {
   private String name;
   private int rating;
   private int power;

    public Hero() {
        **this("Unnamed Scrub", 0, 0);**
    } public Hero(name) {
        **this(name, 0, 0);**
    } public Hero(String name, int rating, int power) {
       this.name = name;
       this.rating = rating;
       this.power = power;
   }
}
```

在这里，构造函数内部的“this”调用带有默认参数的最终构造函数时，第一个构造函数不把它们作为参数(“未命名的 scrub”和 0，0)，第二个构造函数不把 name 参数作为参数。

8.类别变量

虽然成员变量可以是类的每个实例都拥有的属性，比如超级英雄案例中的名称或等级，但类成员变量是与类相关联的变量。使用单词“static”声明类变量。

```
public class Hero {
   private static int totalHeroes = 0 public Hero(String name, int rating, int power) {
       this.name = name;
       this.rating = rating;
       this.power = power;
       totlHeroes ++
   }

   public int getTotalHeroes() {
      return totalHeroes;
   }
}
```

现在，每当创建一个新英雄时，构造函数都会递增 totalHeroes 类变量。类方法可以通过调用

```
Class.methodHero.totalHeroes returns the totalHeroes class variable
```

除了通过直接将类字段设置为某个值来初始化它们之外，还可以用块来初始化它们，或者将它们设置为某个方法的返回值。

```
private static String initializeName() { 
   block 
}private static = String methodName
```

9.嵌套类

嵌套类是在另一个类中定义的类。

```
class Hero {
    ...
    static class sClassHero{
        ...
    }
}
```

嵌套类可以是静态的(静态嵌套类)或非静态的(内部类)。内部类可以访问其封闭类的成员，而静态嵌套类则不能。

10.接口

接口是一种引用类型(基于类的数据类型，而不是原始数据类型)，用于具有共享相似性的类。一个接口可以包含方法签名(方法名和参数)、默认方法、静态方法(类方法)和常量定义。例如，一个英雄职业的成员和一个恶棍职业的成员将共享出拳的能力。从技术上来说，你可以有一个更大的职业，比如拳法中的战士，英雄和恶棍都是一个子类，但是这有点奇怪，因为战士这个职业并没有什么需要或者重点。因此，您可以创建一个包含英雄类和恶棍类都实现的方法名称的接口。

```
public interface Fighter {
    string setNewRating(target, rating);
}
```

会是接口。编写了方法及其参数，但没有编写它将运行的实际代码。

```
public class hero implements Fighter {
   private String name;
   private int rating;
   private int power;

    string setNewRating(target, rating) {
       target.rating = rating       
    }
}
```

它所做的是在实现接口的类中定义的。

11.扩展接口

如果你有一个接口

```
public interface Fight {
   void punch(puncher, punchee);
   void kick(kicker, kickee);
}
```

并且您希望向它添加一行，那么实现您的旧接口的任何东西都将中断。为了防止这种情况，您需要创建一个扩展旧接口的新接口

```
public interface SuperFight extends Fight{
   void kamehameha(blaster, futureDeadGuy);
}
```

这允许某人使用你的旧界面，或者升级到你的新界面。您还可以添加新方法作为默认方法，这些方法不需要编译

```
public interface Fight {
   void punch(puncher, punchee);
   void kick(kicker, kickee);
   default void kamehameha(blaster, futureDeadGuy){
      method
   }
}
```

请注意，对于默认方法，您需要编写的不仅仅是方法名和属性。

12.遗产

一个类可以从另一个类继承字段和方法。继承类称为子类，另一个称为超类。

```
public class SClassHero extends Hero {

}
```

子类从其超类继承所有公共字段、方法和嵌套类。私有成员不被继承，尽管它们可以通过公共方法访问。对于一个子类，你可以定义新的字段和方法，也可以通过给它相同的签名(方法名和参数)来覆盖超类的方法，还可以添加一个构造函数来调用超类的构造函数。

具有相同签名的实例方法将覆盖超类的方法。如果它是一个共享签名的类方法，那么子类中的方法会隐藏超类中的方法。当一个实例方法从一个对象上的新类中运行(覆盖)时，将查看新对象的类，并使用该对象的类中的方法。对于静态方法(隐藏的),编译器将从声明的类型运行静态方法，即使它是错误的。如果静态方法和非静态方法具有相同的签名，结果将是错误的。

实例方法重写接口默认方法。由于覆盖，子类的实例可以具有与父类的实例或具有相同父类的另一个子类不同的行为。

当方法被重写时，可以通过调用

```
super.methodName
```

您可以通过调用来调用超类的构造函数

```
public SuperClass(parameter1, parameter2, parameter 7) {
   super(parameter1, parameter2);
   attributeName = parameter7;
}
```

超类构造函数的调用必须是子类构造函数的第一行。

```
super(parameter list)
```

方法重载是拥有多个同名函数的过程，但这些函数的区别在于它们接受的参数数量。

```
public printMethod(parameter1) {
    System.out.println(parameter1)
}public2 printMethod(parameter1, parameter2) {
    System.out.println("SORRY SIR THIS PROGRAM ONLY PRINTS ONE THING AT A TIME!")
}
```

在这种情况下，这两种方法都被称为“printMethod ”,但是一个有一个参数，另一个有两个参数。所以如果你用一个参数调用这个方法，它会知道你调用的是第一个参数并打印你的参数，但是如果你用两个参数调用它，它会调用第二个参数，取而代之的是“对不起，先生，这个程序一次只打印一个东西！”

object 类位于所有对象的顶部，它们都继承自它。这允许您在对象的任何实例上运行以下方法。

```
object1.clone()
# Creates and returns a copy of this object.object1.equals(object)
# indicates whether another object is equal to this oneobject1.finalize()
# when there are no more references to an object this calls it to the big object yard in the skyobject1.getClass()
# returns the class of an objectobject1.hashCode()
# returns the hash code value for the objectobject1.toString()
# returns a string representation of the object (useful for debugging)
```

Final 可以添加到方法中，以防止它被子类覆盖。您也可以声明整个类为 final，在这种情况下，它不能获得子类。

13.数字和字符串

大多数时候，数字都是基本类型(比如 int)，但是你可以用包装器类型(Byte、Double、Float、Integer、Long、Short)把它们包装成对象。您可以通过将它创建为您希望在其中使用它的类的一部分来做到这一点。

```
Integer i = new Integer(num)# instead ofint i = 57
```

以下是可以在数字和字符串上调用的各种方法。

```
byte byteValue()
short shortValue()
int intValue()
long longValue()
float floatValue()
double doubleValue()#Converts the value of a number object back into primitive form.num1.compareTo(num2)
#If num2 is bigger it will return -1, if they are equal it will return 0, and if num1 is bigger it will return 1.num1.equals(num2)
#Similar to ==, but == checks if two things point to the same location in memory, while .equals compares the values.parseInt(stringNumber)
#Returns value of string as an integer.Integer.valueOf(stringNum)
#Returns value of string as an integer.Integer.valueOf(numObj)
#Returns value of number object as an integer.String.toString(num)
#Returns string object of value of num.String.valueOf(stringNum)
#Returns value of string as an integer.String.valueOf(numObj)
#Returns value of number object as a string.Math.abs(num)
#Returns the absolute value of num.Math.ceil(num)
#Returns smallest integer greater than or equal to num.Math.floor(num)
#Returns greatest integer less than or equal to num.Math.round(num)
#Returns closest long or round, depending on the data type of num.Math.max(num1 num2)
#Returns larger of two arguments.Math.min(num1 num2)
#Returns smaller of two arguments.Math.log(num)
#Returns the log of num.Math.sqrt(num)
#Returns the square root of num.Math.sin(num)
Math.cos(num)
Math.tan(num)
#Returns sin/cosine/tangent of num.Math.random()
#Returns a number between 0.0 and 1.0\. You will generally multiply the result of this by something to get an integer, such as multiplying the result by 10 to get a number between 1 and 10.
```

14.茶

大多数情况下，char 是原始 char 类型。像前面的数字一样，您可以将它包装在 char 类中，这样它就变成了一个对象。

```
Character ch = new Character('a')
```

字符类是不可变的。

```
Character.isLetter(char)
# determines if char is a letterCharacter.isDigit(char)
# determines if char is a digitCharacter.isWhitespace(char)
# determines if char is white spaceCharacter.isUpperCase(char)
# determines if char is a upper caseCharacter.isLowerCase(char)
# determines if char is a lower caseCharacter.toUpperCase(char)
# Transforms char to upper caseCharacter.toLowerCase(char)
# Transforms char to lower caseCharacter.toString(char)
# Transforms char to string
```

在很多转义序列中，键入“\”后跟一个字母会在引号之间插入一些内容。

```
\t
# insert tab\b
# insert backspace\n
# insert new line\r
# insert carriage\f
# insert formfeed\'
# insert single quote\"
# insert double quote\\
# insert backslash
```

15.用线串

String 是 String 类的一部分。

```
string1.length()
# returns amount of characters in a stringstring1.concat(string2)
# used to add string2 to the end of string1
# can also be done with just string1 + string2Integer.parseInt(stringNumber)
Integer.valueOf(stringNumber)
# both returns stringNumber as an integerString.valueOf(num)
Integer.toString(num)
# both returns num as a stringstring1.charAt(num)
# returns string form index of num (index starts at 0)string.substring(num1, num2)
# returns a substring starting at num1 and lasting to num2string.substring(num1)
# returns a substring starting at num1 and lasting until the endstring1.split(expression)
# splits string into an array of strings seperated by the expressionstring1.split(expression, num)
# splits string into an array of strings seperated by the expression with num being the amount length of the arraystring1.subsequence(num1, num2)
# returns a charsequence starting at num1 and lasting to num2string1.trim()
# returns a string1 with no white spaces in front or backstring1.toLowerCase()
# returns string1 converted to all lower casestring1.toUpperCase()
# returns string1 converted to all upper casestring1.indexOf(char)
#returns index of first charstring1.lastIndexOf(char)
#returns index of last charstring1.indexOf(string2)
#returns index of first instance of first char of string2string1.lastIndexOf(string2)
#returns index of last instance of first of string2string1.contains(string2)
#returns true if string1 contains string2, otherwise returns falsestring1.replace(char1, char2)
#returns string with all of char1 replaced by char2string1.replace(string2, string3)
#returns string with all of string2 replaced by string3string1.replace(regex, string2)
#returns string with anything that matches the regular expression replaced with string2string1.replaceFirst(regex, string2)
#returns string with first instance that matches the regular expression replaced with string2string1.endsWith(string2)
#returns true if string ends with string2string1.startsWith(string2)
#returns true if string starts with string2string1.compareTo(string2)
#returns 1 is lexographically greater than string 2 returns a positive integer, a negative if its less, and 0 if its the samestring1.compareToIgnoreCase(string2)
#same as above but ignores casestring1.equals(string2)
#returns true if same sequence of charactersstring1.matches(regex)
#returns true if string mathces regular expression
```

16.Stringbuilder 类

这里的对象被视为包含字符序列的数组。

```
new StringBuilder
# creates empty string builder with capacity to hold 16 elementsStringbuilder(string1)
# creates a stringbuilder object with the same characters as string1 + empty 16 elements trailingstring1.charAt(num)
#returns char at index num for string1string1.append(string2)
# converts string2 to string and adds to the end of string1string1.delete(num1 num2)
# deletes subsequence from num1 to num2-1 (inclusive)string1.deleteCharAt(num2)
# deletes subsequence at index numstring1.insert(num1, string2)
# inserts string2 into string1 after index num1string1.replace(num1, num2, string2)
# replaces subsequence from num1 to num2 with string2string1.setCharAt(num, char)
# replaces character at index num with charstring1.reverse()
# reverses sequence of string1string1.toString()
# returns a string with the same character sequence as string1
```

17.包裹

包是组织一组相关类和接口的名称空间。要创建包，请选择一个名称并包括

```
package packageName;
```

位于每个文件的顶部，该文件具有您想要包含在包中的类型。

要使用整个包，请使用

```
import packageName.*;
```

从包中导入一个特定的类

```
import packageName.ClassName;
```