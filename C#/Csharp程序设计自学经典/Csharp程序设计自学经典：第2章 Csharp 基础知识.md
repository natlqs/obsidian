- # 第2章 Csharp 基础知识
	- ## 2.1 变量与常量
	  collapsed:: true
		- ### 2.1.1 C# 中的变量
		- ### 2.1.2 C# 中的常量
		- ### 2.1.3 变量与常量的初始化
	- ## 2.2 数据类型的分类
	  collapsed:: true
		- C# 数据类型分为值类型和引用类型两种。
		- 值类型存储的是变量本身的数据，而引用类型则存储实际数据的引用。下面详细介绍值类型和引用类型的特性。
		  collapsed:: true
			- **1．值类型**
			  collapsed:: true
				- 值类型变量都存储在堆栈中。
				  logseq.order-list-type:: number
				- 访问值类型变量时，一般都是直接访问其实例。
				  logseq.order-list-type:: number
				- 每个值类型变量都有自己的数据副本，因此对一个值类型的变量的操作不会影响其他的变量。
				  logseq.order-list-type:: number
				- 复制值类型变量时，复制的是变量的值，而不是变量的地址。
				  logseq.order-list-type:: number
				- 值类型变量不能为null，必须具有一个确定的值。
				  logseq.order-list-type:: number
			- **2．引用类型**
			  collapsed:: true
				- 必须在托管堆中为引用类型变量分配内存。
				  logseq.order-list-type:: number
				- 必须使用new关键字来创建引用类型变量。
				  logseq.order-list-type:: number
				- 在托管堆中分配的每个对象都有与之相关联的附加成员，这些成员必须被初始化。
				  logseq.order-list-type:: number
				- 引用类型变量是由垃圾回收机制来管理的。
				  logseq.order-list-type:: number
				- 多个引用类型变量都可以引用同一个对象，这种情形下，对一个变量的操作会影响另一个变量所引用的同一对象。
				  logseq.order-list-type:: number
				- 引用类型被赋值之前的值都是null。
				  logseq.order-list-type:: number
		- 值类型又可分为简单值类型和复合值类型。
		- 简单值类型就是组成应用程序中基本构件的类型，包括整数类型、字符类型、实数类型、布尔类型等。复合值类型包括结构类型、枚举类型。引用类型包括类、接口、委托和数组。本节只介绍简单值类型以及比较特殊的引用类型——字符串类型，复合值类型以及其他引用类型将在后续章节进行介绍
		- 表2-1为C# 中的数据类型表。一些类型名称前面的“u”是unsigned的缩写，表示不能在这些类型的变量中存储负数。表2-1为C# 中的数据类型表。一些类型名称前面的“u”是unsigned的缩写，表示不能在这些类型的变量中存储负数。
		- *表2-1 C# 数据类型表*
		  background-color:: green
		  collapsed:: true
			- |类　型|别　名|允许的值|示　例|
			  |bool|System.Boolean|布尔值：true或false|bool isDog=true;|
			  |byte|System.Byte|无符号8位整数|byte myByte=5;|
			  |sbyte|System.SByte|有符号8位整数|sbyte mySbyte=-101;|
			  |char|System.Char|16位Unicode字符|char myChar='N';|
			  |decimal|System.Decimal|128位浮点数，精确到小数点后28或29位|decimal money=789.12M;|
			  |double|System.Double|64位浮点数，精确到小数点后15或16位|double num=22.32D;|
			  |float|System.Single|32位浮点数，精确到小数点后7位|float score=89.5F;|
			  |int|System.Int32|有符号32位整数|int num=1;|
			  |uint|System.UInt32|无符号32位整数|uint num=111;|
			  |long|System.Int64|有符号64位整数|long num=123456789;|
			  |ulong|System.UInt64|无符号64位整数|ulong num=1111111111;|
			  |short|System.Int16|有符号64位整数|short num=1010;|
			  |ushort|System.U Int16|无符号64位整数|ushort=1234;|
			  |string|System.String|Unicode字符串，引用类型|string name="zhangsan";|
	- ## 2.3 运算符和表达式
	  collapsed:: true
		- ### 2.3.1 运算符的分类
		- ### 2.3.2 运算符的优先级
	- ## 2.4 字符与字符串的处理
	  collapsed:: true
		- ### 2.4.1 char的使用
		  collapsed:: true
			- **1．char简介**
			  collapsed:: true
				- char关键字用于声明.NET Framework中使用Unicode字符表示System.Char结构的实例。Char对象的值是16位数字（序号值）。
				- Unicode字符是16位字符，用于表示世界上大多数已知的书面语言。与C++中的char不一样，C# 中的char的长度不是一个字节，而是两个字节。char表示一个16位的Unicode字符。
				- char的声明与初始化：
				- ```c#
				  char char1 = 't';
				  char char2 = '0';
				  ```
			- **2. char 类的应用**
			  collapsed:: true
				- Char类提供了丰富的操作字符的方法，Char类的方法及说明如表2-7所示。
				  collapsed:: true
					- *表2-7 Char类的方法及说明*
					  background-color:: green
					  collapsed:: true
						- |方　法|说　明|
						  |CompareTo(Char)|将此实例与指定的Char对象进行比较，并指示此实例在排序顺序中是位于指定的Char对象之前、之后还是与其出现在同一位置|
						  |CompareTo(Object)|将此实例与指定的对象进行比较，并指示此实例在排序顺序中是位于该指定的Object之前、之后还是与其出现在同一位置|
						  |ConvertFromUtf32|将指定的Unicode码位转换为UTF-16编码字符串|
						  |ConvertToUtf32(Char, Char)|将UTF-16编码的代理项对的值转换为Unicode码位|
						  |ConvertToUtf32(String, Int32)|将字符串中指定位置的UTF-16编码字符或代理项对的值转换为Unicode码位|
						  |Equals(Char)|返回一个值，该值指示此实例是否与指定的Char对象相等|
						  |Equals(Object)|返回一个值，该值指示此实例是否与指定的对象相等（重写ValueType.Equals(Object)）|
						  |GetHashCode|返回此实例的哈希代码（重写ValueType.GetHashCode()）|
						  |GetNumericValue(Char)|将指定的数字Unicode字符转换为双精度浮点数|
						  |GetNumericValue(String, Int32)|将指定字符串中位于指定位置的数字Unicode字符转换为双精度浮点数|
						  |GetType|获取当前实例的Type（继承自Object）|
						  |GetTypeCode|返回值类型Char的TypeCode|
						  |GetUnicodeCategory(Char)|将指定的Unicode字符分类到由某个UnicodeCategory值标识的组中|
						  |GetUnicodeCategory(String, Int32)|将指定字符串中位于指定位置的字符分类到由一个UnicodeCategory值标识的组中|
						  |IsControl(Char)|指示指定的Unicode字符是否属于控制字符类别|
						  |IsControl(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于控制字符类别|
						  |IsDigit(Char)|指示指定的Unicode字符是否属于十进制数字类别|
						  |IsDigit(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于十进制数字类别|
						  |IsHighSurrogate(Char)|指示指定的Char对象是否为高代理项|
						  |IsHighSurrogate(String, Int32)|指示字符串中指定位置处的Char对象是否为高代理项|
						  |IsLetter(Char)|指示指定的Unicode字符是否属于Unicode字母类别|
						  |IsLetter(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于Unicode字母类别|
						  |IsLetterOrDigit(Char)|指示指定的Unicode字符是否属于字母或十进制数字类别|
						  |IsLetterOrDigit(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于字母或十进制数字类别|
						  |IsLower(Char)|指示指定的Unicode字符是否属于小写字母类别|
						  |IsLower(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于小写字母类别|
						  |IsLowSurrogate(Char)|指示指定的Char对象是否为低代理项|
						  |IsLowSurrogate(String, Int32)|指示字符串中指定位置处的Char对象是否为低代理项|
						  |IsNumber(Char)|指示指定的Unicode字符是否属于数字类别|
						  |IsNumber(String, Int32)|指示指定字符串中位于指定位置的字符是否属于数字类别|
						  |IsPunctuation(Char)|指示指定的Unicode字符是否属于标点符号类别|
						  |IsPunctuation(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于标点符号类别|
						  |IsSeparator(Char)|指示指定的Unicode字符是否属于分隔符类别|
						  |IsSeparator(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于分隔符类别|
						  |IsSurrogate(Char)|指示指定的字符是否具有代理项码单元|
						  |IsSurrogate(String, Int32)|指示指定字符串中位于指定位置的字符是否具有代理项码单元|
						  |IsSurrogatePair(Char, Char)|指示两个指定的Char对象是否形成代理项对|
						  |IsSurrogatePair(String, Int32)|指示字符串中指定位置处的两个相邻Char对象是否形成代理项对|
						  |IsSymbol(Char)|指示指定的Unicode字符是否属于符号字符类别|
						  |IsSymbol(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于符号字符类别|
						  |IsUpper(Char)|指示指定的Unicode字符是否属于大写字母类别|
						  |IsUpper(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于大写字母类别|
						  |IsWhiteSpace(Char)|指示指定的Unicode字符是否属于空白类别|
						  |IsWhiteSpace(String, Int32)|指示指定字符串中位于指定位置处的字符是否属于空白类别|
						  |Parse|将指定字符串的值转换为它的等效Unicode字符|
						  |ToLower(Char)|将Unicode字符的值转换为它的小写等效项|
						  |ToLower(Char, CultureInfo)|使用指定的区域性特定格式设置信息将指定Unicode字符的值转换为它的小写等效项|
						  |ToLowerInvariant|使用固定区域性的大小写规则，将Unicode字符的值转换为其小写等效项|
						  |ToString()|将此实例的值转换为其等效的字符串表示形式（重写ValueType. ToString()）|
						  |ToString(Char)|将指定的Unicode字符转换为它的等效字符串表示形式|
						  |ToString(IFormatProvider)|使用指定的区域性特定格式信息将此实例的值转换为它的等效字符串表示形式|
						  |ToUpper(Char)|将Unicode字符的值转换为它的大写等效项|
						  |ToUpper(Char, CultureInfo)|使用指定的区域性特定格式设置信息将指定Unicode字符的值转换为它的大写等效项|
						  |ToUpperInvariant|使用固定区域性的大小写规则，将Unicode字符的值转换为其大写等效项|
						  |TryParse|将指定字符串的值转换为它的等效Unicode字符。返回一个指示转换是否成功的代码|
				- 使用这些方法可以方便地操作字符，下面通过一个小例子演示如何使用Char类提供的这些方法。
				  collapsed:: true
					- 例子
					  collapsed:: true
						- ```c#
						  using System;
						  using System.Collections.Generic;
						  using System.Linq;
						  using System.Text;
						  using System.Threading.Tasks;
						      
						  namespace ConsoleCharClass
						  {
						      class Program
						      {
						  	    static void Main(string[] args)
						  	    {
						  		//声明字符a到g
						  		char a = 'a';
						  		char b = 'M';
						  		char c = '6';
						  		char d = '.';
						  		char e = '☆';
						  		char f = '|';
						  		char g = ' ';
						  
						  		//使用IsLetter方法判断变量a是否为字母
						  		Console.WriteLine("IsLetter方法判断变量a是否为字母：{0}", Char. 
						  		IsLetter(a));
						  
						  		//使用IsLetterOrDigit方法判断变量b是否为字母或数字
						  		Console.WriteLine("IsLetterOrDigit方法判断变量b是否为字母或数字：
						  		{0}", Char.IsLetterOrDigit(b));
						  
						  		//使用IsDigit方法判断变量c是否为十进制数字
						  		Console.WriteLine("IsDigit方法判断变量c是否为十进制数字：{0}", 
						  		Char.IsDigit(c));
						  
						  		//使用IsLower方法判断变量a是否为小写字母
						  		Console.WriteLine("IsLower方法判断变量a是否为小写字母：{0}", Char. 
						  		IsLower(a));
						  
						  		//使用IsUpper方法判断变量b是否为大写字母
						  		Console.WriteLine("IsUpper方法判断变量b是否为大写字母：{0}", Char. 
						  		IsUpper(b));
						  
						  		//使用IsPunctuation方法判断变量d是否为标点符号
						  		Console.WriteLine("IsPunctuation方法判断变量d是否为标点符号：{0}", 
						  		Char.IsPunctuation(d));
						  
						  		//使用IsSymbol方法判断变量e是否为符号
						  		Console.WriteLine("IsSymbol方法判断变量e是否为符号：{0}", Char. 
						  		IsSymbol(e));
						  
						  		//使用IsSeparator方法判断变量f是否为分隔符
						  		Console.WriteLine("IsSeparator方法判断变量f是否为分隔符：{0}", 
						  		Char.IsSeparator(f));
						  
						  		//使用IsWhiteSpace方法判断变量g是否为空白
						  		Console.WriteLine("IsWhiteSpace方法判断变量g是否为空白：{0}", 
						  		Char.IsWhiteSpace(g));
						  		Console.ReadLine();
						              }
						          }
						      }
						  ```
			- **3. 转义字符**
			  collapsed:: true
				- 转义字符是一种特殊的字符常量；C# 中采用字符“\”作为转义字符。它具有特定的含义，不同于字符原有的意义，故称为“转义”字符。转移字符具有以下特点。
				  collapsed:: true
					- 以反斜线“\”开头，后跟一个或多个字符。
					  logseq.order-list-type:: number
					- 转义字符主要用来表示那些用一般字符不便于表示的控制代码。
					  logseq.order-list-type:: number
					- 它的作用是消除紧随其后的字符的原有含义。
					  logseq.order-list-type:: number
					- 用可以看见的字符表示那些不可以看见的字符，如'\n'表示换行。
					  logseq.order-list-type:: number
					- *表2-8列出了一些常用的转义字符及其含义。*
					  background-color:: green
					  collapsed:: true
						- |转义字符|意　义|
						  |\\'|单引号符|
						  |\\"|双引号符|
						  |\\\|反斜线符“\”|
						  |\\0|空字符（null）|
						  |\\a|鸣铃|
						  |\\b|退格|
						  |\\f|走纸换页|
						  |\\n|换行|
						  |\\r|回车|
						  |\\t|横向跳到下一制表位置|
						  |\\v|竖向跳格|
					- *例2-4  转义符的应用（ConsoleEscapeCharater）*
					  collapsed:: true
						- ```c#
						  using System;
						      using System.Collections.Generic;
						      using System.Linq;
						      using System.Text;
						      using System.Threading.Tasks;
						      
						      namespace ConsoleEscapeCharacter
						      {
						          class Program
						          {
						              static void Main(string[] args)
						              {
						                  string name, address, age;
						                  Console.Write("请输入您的姓名：");
						                  name = Console.ReadLine();
						   				Console.Write("请输入您的年龄：");
						                  age = Console.ReadLine();
						                  Console.Write("请输入您的出生地：");
						                  address = Console.ReadLine();
						                  Console.WriteLine("您的基本信息如下：");
						                  Console.WriteLine("姓名\t年龄\t出生地");
						                  Console.WriteLine("{0}\t{1}\t{2}", name, age, address);
						                  Console.ReadLine();
						              }
						          }
						      }
						  ```
		- ### 2.4.2 字符串类String的使用
		  collapsed:: true
			- 字符串是应用程序中最常用的一种数据类型。C# 中专门处理字符串的String类，位于System命名空间中，我们一直使用的string只不过是String类的一个别名。
			- **1．String类的使用**
			  collapsed:: true
				- C# 中string与C++一样，都是由多个char组成。也就是说，C# 中的string也是由多个16位的Unicode字符组成。
				- 字符串是Unicode字符的有序集合，用于表示文本。String对象是System.Char对象的有序集合，用于表示字符串。String对象的值是该有序集合的内容，并且该值是不可变的。正是因为字符构成了字符串，根据字符在字符串中的不同位置，字符在字符串中有一个索引值，可以通过索引值获取字符串中的某个字符。字符在字符串中的索引从零开始。例如，字符串“hellow”中的第一个字符为h，而“h”在字符串中的索引顺序为0。需要注意的是，String类所定义的变量是一个引用类型，可以对String类型的变量进行null赋值。
				- 例2-5 字符串中字符的获取，代码中采取在字符串变量后加一个方括号，并在方括号里面给出索引值的方法获取相应的字符。这是数组变量通用的索引方法。str[1]获取的是字符串变量str的第二个字符。
				  collapsed:: true
					- ```c#
					  using System;
					  using System.Collections.Generic;
					  using System.Linq;
					  using System.Text;
					  using System.Threading.Tasks;
					      
					      namespace ConsoleGetCharByString
					      {
					          class Program
					          {
					              static void Main(string[] args)
					              {
					                  //声明一个字符串变量
					                  string str = "Hello World！";
					                  //获取字符串str的第2个字符
					                  char char1 = str[1];
					                  //获取字符串str的第3个字符
					                  char char2 = str[2];
					                  //获取字符串str的第7个字符
					              }
					          }
					      }
					  
					  ```
			- **2. 常用的字符串处理方法**
			  collapsed:: true
				- C# 中提供了比较丰富的字符串处理方法，表2-9中列出了一些常用的字符串处理方法和每个方法接收的参数和返回值及其说明。
				- *表2-9 常用字符串处理方法*
				  background-color:: green
				  collapsed:: true
					- |方　法|说　明|
					  |bool Equals(string value)|比较一个字符串与另一个字符串value的值是否相等。如果二者相等返回true；如果不相等返回false。该方法的作用与运算符“==”相同|
					  |int Compare(string str1,string str2)|比较两个字符串的大小关系，返回一个整数。如果str1小于str2，返回值小于0；如果str1等于str2，返回值为0；如果str1大于str2，返回值大于0|
					  |int IndexOf(string value)|获取指定的value字符串在当前字符串中的第一个匹配项的位置。如果找到了value，就返回它的位置；如果没有找到，就返回–1|
					  |int LastIndexOf(string value)|获取指定的字符串value在当前字符串中最后一个匹配项的位置。如果找到了value，就返回它的位置；如果没有找到，就返回–1|
					  |string Join(string separator,string[] value)|把字符串数组value中的每个字符串用指定的分隔符separator连接，返回连接后的字符串|
					  |string Split(char separator)|用指定的分隔符separator分割字符串，返回分割后的字符串数组|
					  |string Sunstring(int startIndex,int length)|从指定的位置startIndex开始检索长度为length的子字符串|
					  |string ToLower()|获得字符串的小写形式|
					  |string ToUpper()|获得字符串的大写形式|
					  |string Trim()|去掉字符串前后两端多余的空格|
				- **比较字符串**
				  logseq.order-list-type:: number
				  collapsed:: true
					- C# 中最常见的比较字符串的方法有Equals、Compare、CompareTo和CompareOrdinal，这些方法都属于String类。下面就分别对这4种方法进行详细介绍。
					- **Equals方法**
					  collapsed:: true
						- Equals方法通常用来比较两个对象的值是否相等，如果相同返回true，否则返回false。其常用的两种语法如下：
						  collapsed:: true
							- ```c#
							  public bool Equals(string value)
							  public static bool Equals(string a,string b)
							  ```
						- 其中，value是与实例比较的字符串，a和b是要进行比较的两个字符串。
						- *例2-6：使用Equals比较字符串（ConsoleEquals）*
						  collapsed:: true
							- ```c#
							  string str1 = "Hello World!";
							  string str2 = "HelloWorld！";
							  Console.WriteLine(str1.Equals(str2));
							  Console.WriteLine(String.Equals(str1, str2));
							  Console.ReadLine();
							  ```
						- #+BEGIN_NOTE
						  运算符==和String类方法Equals()的区别如下。
						  ① ==通常用来比较int、double等数值类型的数据是否相等。
						  ② Equals()通常用来比较两个对象的值是否相等。
						  #+END_NOTE
						- #+BEGIN_NOTE
						  “”和String.Empty()的区别如下。
						  ① “”为String对象分配长度为0的内存空间。
						  ② String.Empty()不会为对象分配内存空间。
						  #+END_NOTE
					- **Compare方法**
					  collapsed:: true
						- Compare方法用来比较两个字符串是否相等，它有很多个重载方法，其中最常用的方法如下。
						  collapsed:: true
							- ```c#
							  static Int Compare(string a, string b)
							  static Int COmpare(string a, string b, bool ignoreCare)
							  ```
						- 其中，a和b代表要比较的两个字符串。
						- ignorCase如果是true，那么在比较字符串时就忽略大小写的差别。
						- 下面通过一个小实例来演示Compare方法的使用。
						  collapsed:: true
							- ```c#
							  string str1 = "Hello World!";
							  string str2 = "HelloWorld！";
							  Console.WriteLine(string.Compare(str1, str2));
							  Console.WriteLine(string.Compare(str2, str1));
							  Console.WriteLine(String.Compare(str1, str1));
							  Console.ReadLine();
							  ```
						- #+BEGIN_NOTE
						  关键点解析：
						  比较字符串并非比较字符串长度的大小，而是比较字符串在英文字典中的位置。比较字符串按照字典排序规则判断两个字符串的大小，在英文字典中，前面的单词小于后面的单词。
						  #+END_NOTE
					- **CompareTo方法**
					  collapsed:: true
						- CompareTo方法与Compare方法类似，都可以比较两个字符串是否相等，不同的是CompareTo方法以实例对象本身指定的字符串做比较，其语法如下：
						  collapsed:: true
							- ```c#
							  public  int CompareTo(string a)
							  ```
						- 下面通过一个小实例来演示CompareTo方法的使用。
						- 例2-8：使用CompareTo比较字符串（ConsoleCompareTo）
						  collapsed:: true
							- ```c#
							  string str1 = "你好啊";
							  string str2 = "你好";
							  Console.WriteLine(str1.CompareTo(str2));
							  Console.ReadLine();
							  ```
					- **CompareOrdinal方法**
					  collapsed:: true
						- CompareOrdinal方法对两个字符串进行比较，不考虑本地化语言和文化。其中常用的语法如下：
						  collapsed:: true
							- ```c#
							   public static CompareOrdinal(string a,string b)
							  ```
						- 下面通过一个小实例来演示CompareOrdinal方法的使用。
						- 例2-9：使用CompareOrdinal比较字符串（ConsoleCompareOrdinal）
						  collapsed:: true
							- ```c#
							  string str1 = "Hello Worlda";
							  string str2 = "Hello WorldA";
							  Console.WriteLine(string.CompareOrdinal(str1, str2)); 
							  Console.ReadLine();
							  ```
				- **截取字符串**
				  logseq.order-list-type:: number
				  collapsed:: true
					- String类提供了Substring方法，该方法用于截取字符串中指定位置和指定长度的字符。其语法格式如下：
					  collapsed:: true
						- ```c#
						  public string Substring(int startIndex, int lenght)
						  ```
					- 其中，startIndex代表子字符串的起始位置的索引，length代表要截取的子字符串的字符串。
					- 例2-10：使用Substring截取字符串（ConsoleSubstring）
					  collapsed:: true
						- ```c#
						  string str1 = "HelloWorld！";
						  string str2 = str1.Substring(5, 6);
						  Console.WriteLine(str2);
						  Console.ReadLine();
						  ```
				- **IndexOf方法取索引**
				  logseq.order-list-type:: number
				  collapsed:: true
					- IndexOf方法报告指定的字符在此实例中的第一个匹配项的索引。搜索从指定字符位置开始，并检查指定数量的字符位置。其语法格式如下:
					  collapsed:: true
						- ```c#
						  public int IndexOf(value, [startIndex], [count])
						  ```
						- 其中，value：要查找的Unicode字符。对value的搜索区分大小写。startIndex（Int32）：可选项，搜索起始位置。不设置则从0开始。count（Int32）：可选项，要检查的字符位数。
						- 下面通过一个实例来演示IndexOf的使用。
						- 例2-11：使用IndexOf获取指定字符串索引（ConsoleIndexOf）
						  collapsed:: true
							- ```c#
							  string str1 = "HelloWorld！";
							  string str2 = "o";
							  Console.WriteLine(str1.IndexOf(str2));
							  Console.WriteLine(str1.IndexOf(str2, 5));
							  Console.WriteLine(str1.IndexOf(str2, 0, 4));
							  Console.ReadLine();
							  ```
				- **连接字符串**
				  logseq.order-list-type:: number
				  collapsed:: true
					- C# 中连接字符串使用String类的Join()方法。其语法格式如下：
					  collapsed:: true
						- ```c#
						  public string Join(string value,string[] args)
						  ```
					- 其中，value为连接字符串的连接符，args为要连接的string数组对象。在指定的args数组的每个元素之间串联指定的分隔符value，从而产生单个串联的字符串。
					- 例2-12：使用Join连接字符串（ConsoleJoin）
					  collapsed:: true
						- ```c#
						  string str1 = "-";
						  string[] strs = new string[] { "hi", "china", "hello", "world" };
						  Console.WriteLine(string.Join(str1, strs));
						  Console.ReadLine();
						  ```
				- **分割字符串**
				  logseq.order-list-type:: number
				  collapsed:: true
					- String类提供了一个用于分割字符串的Split方法，该方法的返回值是包含所有分割子字符串的数组对象。其语法格式如下：
					  collapsed:: true
						- ```c#
						  public string[] Split(params char[] separator)
						  public string[] Split(char[] separator, int count)
						  public string[] Split(char[] separator, StringSplitOptions options)
						  public string[] Split(string[] separator, StringSplitOptions options)
						  public string[] Split(char[] separator, int count, StringSplitOptions options)
						  public string[] Split(string[] separator, int count, StringSplitOptions options)
						  ```
					- 例2-13：使用Split分割字符串（ConsoleSplit）
					  collapsed:: true
						- ```c#
						  string str1 = "a,b.c,,d,e";
						  //1. public string[] Split(params char[] separator)
						  string[] split1 = str1.Split(new Char[] { ',' });
						  string[] split2 = str1.Split(new Char[] { ',', '.' });
						  foreach (string s in split1)
						  {
						  	Console.Write(s + "#");
						  }
						  Console.WriteLine();
						  foreach (string s in split2)
						  {
						  	Console.Write(s + "#");
						  }
						  //2. public string[] Split(char[] separator, int count)
						  
						  string[] split3 = str1.Split(new Char[] { ',', '.' }, 2);
						  string[] split4 = str1.Split(new Char[] { ',', '.' }, 6);
						  Console.WriteLine();
						  foreach (string s in split3)
						  {
						  Console.Write(s + "#");
						  }
						  Console.WriteLine();
						  foreach (string s in split4)
						  {
						  Console.Write(s + "#");
						  }
						  //3. public string[] Split(char[] separator, StringSplitOptions 
						  
						  string[] split5 = str1.Split(new Char[] { ',', '.' }, 
						  StringSplitOptions.RemoveEmptyEntries);
						  string[] split6 = str1.Split(new Char[] { ',', '.' }, 
						  StringSplitOptions.None);
						  Console.WriteLine();
						  foreach (string s in split5)
						  {
						  	Console.Write(s + "#");
						  }
						  Console.WriteLine();
						  foreach (string s in split6)
						  {
						  	Console.Write(s + "#");
						  }
						  //4. public string[] Split(string[] separator, StringSplitOptions options)
						  string[] split7 = str1.Split(new string[] { ",", "." }, 
						  StringSplitOptions.RemoveEmptyEntries);
						  //返回:{"1","2","3","4"} 不保留空元素
						  string[] split8 = str1.Split(new string[] { ",", "." }, 
						  StringSplitOptions.None);//返回:{"1","2","3","","4"} 保留空元素
						  Console.WriteLine();
						  foreach (string s in split7)
						  {
						  	Console.Write(s + "#");
						  }
						  Console.WriteLine();
						  foreach (string s in split8)
						  {
						  	Console.Write(s + "#");
						  }
						  //5. public string[] Split(char[] separator, int count, StringSplitOptions options)
						  
						  string[] split9 = str1.Split(new Char[] { ',', '.' }, 2, 
						  StringSplitOptions.RemoveEmptyEntries);
						  string[] split10 = str1.Split(new Char[] { ',', '.' }, 6, 
						  StringSplitOptions.None);
						  Console.WriteLine();
						  foreach (string s in split9)
						  {
						  	Console.Write(s + "#");
						  }
						  Console.WriteLine();
						  foreach (string s in split10)
						  {
						  	Console.Write(s + "#");
						  }
						  
						  //6. public string[] Split(string[] separator, int count, StringSplitOptions options)
						  
						  string[] split11 = str1.Split(new string[] { ",", "." }, 2, 
						  StringSplitOptions.RemoveEmptyEntries);
						  string[] split12 = str1.Split(new string[] { ",", "." }, 6, 
						  StringSplitOptions.None);
						  Console.WriteLine();
						  foreach (string s in split11)
						  {
						  	Console.Write(s + "#");
						  }
						  Console.WriteLine();
						  foreach (string s in split12)
						  {
						  	Console.Write(s + "#");
						  }
						  
						  Console.ReadLine();
						  ```
					- #+BEGIN_NOTE
					  代码中使用foreach循环输出数组的每个元素，并用# 号连接，目的是使读者能更清晰地看到执行结果。foreach语句将在后续章节进行介绍，在此不作为重点内容。
					  需要注意的是没有重载函数public string[] Split(string[] separator)，所以不能像VB.NET那样使用words.Split(",")，而只能使用words.Split(',')。很多读者都很奇怪为什么把双引号改为单引号就可以了？看了上边的重载函数就应该知道答案了。
					  #+END_NOTE
				- **替换字符串**
				  logseq.order-list-type:: number
				  collapsed:: true
					- String类提供了一个Replace的方法，该方法用于将字符串中的某个字符或字符串替换成其他的字符或字符串。其语法格式如下：
					  collapsed:: true
						- ```c#
						  public string Replace(char oldChar,char newChar)
						  public string Replace(string oldStr,string newStr)
						  ```
					- 第一个参数为待替换的字符或字符串，第二个参数为替换后的新字符或新字符串。
					- 第一种语法格式主要用于替换字符串中指定的字符，第二种语法格式主要用于替换字符串中指定的字符串。
					- 下面通过一个小实例来演示Replace的使用。
					- 例2-14：使用Replace替换字符串（ConsoleReplace）
					  collapsed:: true
						- ```c#
						  string str1 = "hello world.";
						  string str2 = str1.Replace('.', '!');
						  Console.WriteLine(str2);
						  string str3 = str1.Replace("world", "Tom");
						  Console.WriteLine(str3);
						  Console.ReadLine();
						  
						  ```
				- **格式化字符串**
				  logseq.order-list-type:: number
				  collapsed:: true
					- 在C# 中，String类提供了一个静态的Format方法，用于将字符串数据格式化成指定的格式。其语法格式如下：
					  collapsed:: true
						- ```c#
						  public static string Format(string format,object obj)
						  ```
					- 其中，format是用来指定字符串所要格式化的形式，obj则是要被格式化的对象。
					- 例2-15：使用Format格式化字符串（ConsoleFormat）
					  collapsed:: true
						- ```c#
						  string str1 = "你好";
						  string str2 = "中国";
						  string newStr = string.Format("{0},{1}!!", str1, str2);
						  Console.WriteLine(newStr);
						  Console.ReadLine();
						  
						  ```
					- 表2-10列出了Format()方法的格式字符串中各种格式化定义字符和示例。
					  collapsed:: true
						- *表2-10　格式化数值结果*
						  collapsed:: true
							- |字　符|说　明|示　例|输出结果|
							  |C|货币格式|String.Format("{0:C3}",5555)|￥5555.000|
							  |D|十进制格式|String.Format("{0:D3}",5555)|5555|
							  |F|小数点后的位数固定|String.Format("{0:F3}",5555)|5555.000|
							  |N|用逗号隔开的数字|String.Format("{0:N}",5555)|5,555.00|
							  |P|百分比记数法|String.Format("{0:P3}",0.2345)|23.45|
							  |X|十六进制格式|String.Format("{0:X000}",11)|B|
					- 如果需要按某种格式输出日期时间，那么可以使用Format方法将日期时间格式化后再输出，表2-11列出了格式化日期时间的格式规范。
					  collapsed:: true
						- *表2-11　格式化日期时间*
						  collapsed:: true
							- |字　符|说　明|
							  |d|简短日期格式（YYYY-MM-dd）|
							  |D|完整日期格式（YYYY年MM月dd日）|
							  |t|简短时间格式（hh:mm）|
							  |T|完整时间格式（hh:mm:ss）|
							  |f|简短日期/时间格式（YYYY年MM月dd日hh:mm）|
							  |F|完整日期/时间格式（YYYY年MM月dd日hh:mm:ss）|
							  |g|简短的可排序的日期/时间格式（YYYY-MM-dd hh:mm）|
							  |G|完整的可排序的日期/时间格式（YYYY-MM-dd hh:mm:ss）|
							  |M或m|月/日格式（MM月dd日）|
							  |Y或y|年/月格式（YYYY年MM月）|
					- 例2-16：使用Format格式化日期时间（ConsoleFormatDateTime）
					  collapsed:: true
						- ```c#
						  DateTime dt = DateTime.Now;
						  string str1 = String.Format("{0:t}", dt);
						  Console.WriteLine(str1);
						  Console.ReadLine();
						  
						  ```
			- **3. 类型转换**
			  collapsed:: true
				- **简单的类型转换**
				  logseq.order-list-type:: number
				  collapsed:: true
					- 在C# 中简单的类型转换包括隐式类型转换和显示类型转换。
					- *隐士类型转换*
					  logseq.order-list-type:: number
					  collapsed:: true
						- 对于任何数值类型A来说，只要其取值范围完全包含在类别B的取值范围，就可以隐式转换为类型B。也就是说，int类型可以隐式转换为float类型或double类型，float类型也可以隐式转换为double类型。
					- *显示类型转换*
					  logseq.order-list-type:: number
					  collapsed:: true
						- 显示类型转换与隐式类型转换相反，当要把取值范围大 的类型转换为取值范围小的类型时，就需要执行显示转换了。
					- *例2-17：显式类型转换（ConsoleConvert）*
					  logseq.order-list-type:: number
					  collapsed:: true
						- ```c#
						  double score= 73.5;
						  int bonus = 43;
						  int sum = (int)score + bonuss;
						  Console.WriteLine("文化课成绩： {0}分", score);
						  Console.WriteLine("艺术课加分：{0}分", bonus);
						  Console.WriteLine("总分：{0}分", sum);
						  Console.ReadLine();
						  ```
					- 从运行结果中不难看出，变量score的值仍然是73.5，但sum的值却变成了116。这是因为在计算加法时，将score的值转换为73进行计算，丢失了精度所致。尽管对变量score进行了强制类型转换，但实际上score的值并没有改变，只是在计算时临时转换成整数参与表达式的计算。
					  logseq.order-list-type:: number
				- **数值类型与字符串类型的转换**
				  logseq.order-list-type:: number
				  collapsed:: true
					- 隐式类型转换和显示类型转换一般都用在数值之间，并不适用于数值类型以及字符串之间的转换。
					- *字符串转换为数值类型*
					  logseq.order-list-type:: number
					  collapsed:: true
						- C# 中提供了各种类型的Parse()方法来进行类型转换。例如:
						  collapsed:: true
							- 将字符串转为整型代码为：`int.Parse(strValue);`
							- 将字符串转为float型代码为：`float.Parse(strValue);`
							- 将字符串转为double型代码为：`double.Parse(strValue);`
						- #+BEGIN_NOTE
						  此处的strValue必须是数字的有效表示形式。简单地讲就是看起来是对应的数字，但实际上是string类型。例如，可以把“5555”转换为整数，而不能把“zhangsan”等转换为整数。 
						  #+END_NOTE
					- *数值类型转换为字符串*
					  logseq.order-list-type:: number
					  collapsed:: true
						- 在C# 中，数值类型转换为字符串非常简单，只要调用其ToString()方法就可以了。例如：
						- ```c#
						  int num=521;
						  string strNum=num.ToString();
						  ```
						- 除此之外，还可以使用“+”连接符拼接一个空的字符串实现相同的效果，例如：
						- ```c#
						  int num=521;
						  string strNum=num+"";
						  ```
				- **使用Convert类进行转换**
				  logseq.order-list-type:: number
				  collapsed:: true
					- 除了使用Parse()方法外，还可使用Convert类进行转换。Convert类可以在各种基本类型之间执行数据类型的互相转换，它为每种类型转换都提供了一个对应的方法。表2-12为Convert类的常用方法。
					- *表2-12 Convert类的常用方法*
					  collapsed:: true
						- |方　法|说　明|
						  |Convert.ToInt32()|转换为int类型|
						  |Convert.ToChar()|转换为char类型|
						  |Convert.ToDecimal()|转换为decimal类型|
						  |Convert.ToSingle()|转换为float类型|
						  |Convert.ToString()|转换为string类型|
						  |Convert.ToDateTime()|转换为DateTime类型|
						  |Convert.ToDouble()|转换为double类型|
						  |Convert.ToBoolean()|转换为bool类型|
						- 方法中的参数就是需要进行转换的数值。下面通过一个小例子来了解下如何使用Convert类进行转换。
					- *例2-18：使用Convert类进行转换（ConsoleConvertClass）*
					  collapsed:: true
						- ```c#
						  double dNum = 76.74;
						  int iNum;
						  decimal decimalNum;
						  float fNum;
						  String SNum;
						  bool flag;
						  iNum = Convert.ToInt32(dNum);
						  decimalNum = Convert.ToDecimal(dNum);
						  fNum = Convert.ToSingle(dNum);
						  sNum = Convert.ToString(dNum);
						  flag = Convert.ToBoolean(dNum);
						  
						  Console.WriteLine("原数值为double类型：" + dNum);
						  
						  Console.WriteLine("转换后为：");
						  Console.WriteLine("int类型：\t" + iNum);
						  Console.WriteLine("decimal类型：\t" + decimalNum);
						  Console.WriteLine("float类型：\t" + fNum);
						  Console.WriteLine("string类型：\t" + sNum);
						  Console.WriteLine("bool类型：\t" + flag);
						  
						  Console.ReadLine();
						  ```
						- #+BEGIN_NOTE
						  从运行结果中可以看出，转换为int类型时，进行了四舍五入的计算，所以结果为77，这与显式类型转换有所不同，如果使用显式类型转换将76.74转换为int类型的结果是76，它是直接将小数点后的数值舍弃掉了。转换为bool类型的时候，只要转换的值不为0，那么结果就是true，否则为false。
						  #+END_NOTE
		- ### 2.4.3 可变字符串类StringBuilder的使用
		  collapsed:: true
			- **String 类占系统开销**
			  collapsed:: true
				- 我们知道String类有很多实用的处理字符串的方法，但是在使用String类时常常存在这样一个问题：当每次为同一个字符串重新赋值时，都会在内存中创建一个新的字符串对象，需要为该对象分配新的内存空间，这样会加大系统的开销。因为System.String类是一个不可变的数据类型，一旦对一个字符串对象进行初始化后，该字符串对象的值就不能改变了。当对该字符串的值做修改时，实际上是又创建了一个新的字符串对象。比如下面的小例子：
				- ```c#
				  String str1 = "hello";
				  str1 += "world";
				  Console.WriteLine(str1);
				  ```
				- 上面这段代码的输出结果是"HelloWorld"。但是，这段代码创建了几个对象呢？下面逐句分析一下。
				  collapsed:: true
					- 首先，String str1="Hello";创建了一个对象，值为"Hello"。
					- str1+="World"通过+=赋值运算符为该对象赋值。
					- 执行此句时，从表面上看str1的值更新了，实际上内存中又创建了两个新对象，其值分别为"World"和"HelloWorld!"。
					- 此时内存中存在三个对象：Hello、World和HelloWorld。而str1所引用的是HelloWorld对象，其他对象没有实际作用。如此反复的操作会使系统内存占用很大，造成不必要的浪费，大大降低了程序运行性能。如何解决此问题呢？
			- **StringBuilder**
			  collapsed:: true
				- 为了解决以上问题，Microsoft提供了`System.Text.StringBuilder`类，此类表示可变字符串。虽然StringBuilder类像String类一样具有很多处理字符串的方法，但是在添加、删除或替换字符串时，StringBuilder类对象的执行速度要远比String类快得多。
				- 下面来看一下StringBuilder类的定义。StringBuilder类有6种不同的构造方法。在此只讲解比较常用的两种：
				- `StringBuilder sbValue=new StringBuilder();` 此种方法声明了一个空的StringBuilder对象。
				- `StringBuilder sbValue=new StringBuilder("HelloWorld");` 此种方法声明一个StringBuilder对象，其值为“HelloWorld”。
				- 在使用StringBuilder类时先要引用其命名空间System.Text。表2-13为StringBuilder类的常用属性、方法及说明。
				- *表2-13 StringBuilder类的常用属性、方法及说明*
				  collapsed:: true
					- |属　性|说　明|
					  |Capacity|获取或设置可包含在当前对象所分配的内存空间中的最大字符个数|
					  |Length|获取或设置当前对象的长度|
					  |方　法|说　明|
					  |Append()|在结尾追加|
					  |AppendLine ()|将默认的行终止符追加到当前对象的末尾|
					  |AppendFormat()|添加特定格式的字符串|
					  |Insert()|将字符串或对象添加到当前StringBuilder对象中的指定位置上|
					  |Remove()|从当前StringBuilder对象中移除指定范围的字符|
					  |Replace()|将StringBuilder对象内的特定字符用另一个指定的字符来替换|
					  |ToString()|将StringBuilder类对象转换为String类对象|
				- *例2-19：使用StringBuilder类（ConsoleStringBuilder）*
				  collapsed:: true
					- ```c#
					  StringBuilder sb = new StringBuilder();
					      
					  sb.AppendLine("明月几时有？");
					  sb.AppendLine("把酒问青天。");
					  sb.AppendLine("不知天上宫阙,");
					  sb.AppendLine("今夕是何年？");
					  sb.AppendLine("我欲乘风归去，");
					  sb.AppendLine("又恐琼楼玉宇，");
					  sb.AppendLine("高处不胜寒。");
					  sb.AppendLine("起舞弄清影，");
					  sb.AppendLine("何似在人间？");
					  sb.AppendLine("转朱阁，");
					  sb.AppendLine("低绮户，");
					  sb.AppendLine("照无眠。");
					  sb.AppendLine("不应有恨、");
					  sb.AppendLine("何事长向别时圆？");
					  sb.AppendLine("人有悲欢离合，");
					  sb.AppendLine("月有阴晴圆缺，");
					  sb.AppendLine("此事古难全。");
					  sb.AppendLine("但愿人长久，");
					  sb.AppendLine("千里共婵娟。");
					  sb.Insert(0, "《水调歌头·中秋》\n\n");
					  int count = sb.Capacity;
					  sb.Insert(sb.Length, "\t\t——苏轼");
					  sb.Append("\t(字符数为：" + count + ")");
					  Console.WriteLine(sb.ToString());
					  Console.ReadLine();
					  ```
				- 关键点解析：
				  collapsed:: true
					- `sb.Insert(0, "《水调歌头·中秋》\n\n");` 此句在所有字符前面插入“《水调歌头·中秋》\n\n”字符串。
					- `sb.Insert(sb.Length, "\t\t——苏轼");` 此句得到当前StringBuilder对象的长度并在此长度后将字符串“\t\t——苏轼”添加到当前对象中的sb.Length位置上。
				- 最后通过ToString方法把StringBuilder对象转成String类型。
-
-
-
-
-
-
-