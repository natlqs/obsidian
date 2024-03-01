# 第1章 从零认识Csharp
	- ## 1.1 C# 简介
	  collapsed:: true
	  
	  collapsed:: true
		- C# 是Microsoft公司于2000年7月发布的，它是运行于.NET Framework之上的一种**简单、现代、安全、面向对象的高级程序设计语言。**它使得程序员可以快速地编写各种基于Microsoft .NET平台的应用程序，Microsoft .NET提供了一系列的工具和服务来最大程度地开发利用计算与通信领域。
		- 正是由于C\#面向对象的卓越设计，使它成为构建各类组件的理想之选——无论是高级的商业对象还是系统级的应用程序。使用简单的C# 语言结构，这些组件可以方便地转化为XML网络服务，从而使它们可以由任何语言在任何操作系统上通过Internet进行调用。最重要的是，C# 使得C++程序员可以高效地开发程序，而绝不损失C/C++原有的强大的功能。因为这种继承关系，C# 与C/C++具有极大的相似性，熟悉类似语言的开发者可以很快地转向C# 。
		- C# 具有以下突出的特点。
		  background-color:: green
		  collapsed:: true
			- 语法间接。不允许直接操作内存，去掉了指针操作。
			  logseq.order-list-type:: number
			- 彻底的面向对象设计。C# 具有面向对象语言所应有的一切特性——封装、继承和多态。
			  logseq.order-list-type:: number
			- 与Web紧密结合。C# 支持绝大多数的Web标准，如HTML、XML、SOAP等。
			  logseq.order-list-type:: number
			- 强大的安全机制。可以消除软件开发中的常见错误，.NET提供的垃圾回收器能够帮助开发者有效地管理内存资源。
			  logseq.order-list-type:: number
			- 兼容性。因为C# 遵循.NET的公共语言规范（Common Language Specification，CLS），从而保证能够与其他语言开发的组件兼容。
			  logseq.order-list-type:: number
			- 灵活的版本处理技术。因为C# 语言本身内置了版本控制功能，使得开发人员可以更容易地开发和维护。
			  logseq.order-list-type:: number
			- 完善的错误、异常处理机制。C# 提供了完善的错误和异常处理机制，使程序在交付应用时能够更加健壮。
			  logseq.order-list-type:: number
			- 强大的类库支持。C# 有着数量庞大、功能齐全的.NET类库的支持，从而可以轻易地完成复杂的加密操作、网络应用操作等。使用C# 可以轻松构建功能强大、开发快捷、运用方便的应用程序。
			  logseq.order-list-type:: number
	- ## 1.2 .NET概述
	  collapsed:: true
		- .NET是微软的新一代技术平台----Microsoft XML Web Service平台，XML Web Services允许应用程序通过Internet进行通信和共享数据，而不管所采用的是哪种操作系统、设备或编程语言。
		- Microsoft .NET平台提供创建XML Web Services，并将这些服务集成在一起，为敏捷商务构建互连互通的应用系统，这些系统是基于标准的，连通的，适应变化的，稳定的和高性能的。从技术的角度，一个.NET应用是一个运行于.NET Framework之上的应用程序。
	- ## 1.3 C# 与.NET的关系
	  collapsed:: true
	  
	  collapsed:: true
		- C# 是一种相当新的编程语言，C# 的重要性体现在以下两个方面。
		  collapsed:: true
			- 它是专门为与Microsoft的.NET Framework一起使用而设计的。（.NET Framework是一个功能非常丰富的平台，可开发、部署和执行分布式应用程序。
			  logseq.order-list-type:: number
<<<<<<< HEAD
			- 它是一种基于现代面向对象设计方法的语言，在设计它时，Microsoft还吸取了其他类似语言的经验，这些语言是近二十年来面向对象规则得到广泛应用后才开发出来的。有一个很重要的问题要弄明白：C# 就其本身而言只是一种语言，尽管它是用于生成面向.NET环境的代码，但它本身不是.NET的一部分。.NET支持的一些特性，C# 并不支持。而C# 语言支持的另一些特性，.NET却不支持。但是，因为C# 语言是和.NET一起使用的，所以如果要使用C# 高效地开发应用程序，理解Framework就非常重要了，所以下面将介绍.NET的核心——.NET Framework的体系结构。
=======
			- 它是一种基于现代面向对象设计方法的语言，在设计它时，Microsoft还吸取了其他类似语言的经验，这些语言是近二十年来面向对象规则得到广泛应用后才开发出来的。有一个很重要的问题要弄明白：C# 就其本身而言只是一种语言，尽管它是用于生成面向.NET环境的代码，但它本身不是.NET的一部分。.NET支持的一些特性，C# 并不支持。而C# 语言支持的另一些特性，.NET却不支持。但是，因为C#  语言是和.NET一起使用的，所以如果要使用C# 高效地开发应用程序，理解Framework就非常重要了，所以下面将介绍.NET的核心——.NET Framework的体系结构。
>>>>>>> 30f479c74294551e969ef96ce35487abcb3ed8f9
			  logseq.order-list-type:: number
	- ## 1.4　.NET Framework的体系结构
	  collapsed:: true
	  
	  collapsed:: true
		- C# 程序在.NET Framework上运行，它是Windows的一个必要组件，如图1-1所示为.NET Framework的体系结构。**.NET Framework包括两大组件：公共语言运行库（Common Language Runtime，CLR）和.NET Framework类库（Framework Class Library，FCL）。**
		- ![image.png](../assets/image_1687238965212_0.png){:height 425, :width 748}
		- 图1-1　.NET Framework的体系结构
		- ### 1.4.1 公共语言运行库
		  collapsed:: true
			- 公共语言运行库提供了异常处理、安全、调试以及任何语言的版本支持等功能。它可以使用各种程序设计语言，并提供跨语言的公共工具集，从而确保了代码之间的互用性。如图1-2所示为.NET框架的组件。
			- ![image.png](../assets/image_1687239178371_0.png)
			- 图1-2　.NET框架组件
			- CTS（Common Type System，通用类型系统）和CLS（Common Language Specification，公共语言规范）是CLR的子集。
			- CTS定义了在IL中的数据类型，如C# 中的int类型和VB.NET中的Integer类型都被编译成Int32。
			- CLS是CLR支持的语言功能的子集，它包括几种面向对象的编程语言的通用功能。
			- 公共语言运行库是Microsoft的公共语言基础结构（Common Language Infrastructure，CLI）的一个商业实现。CLI是一种国际标准，是用于创建语言和库在其中无缝协同工作的执行和开发环境的基础。可以将运行时看作一个在执行时管理代码的代理，它提供内存管理、线程管理和远程处理等核心服务，并且还强制实施严格的类型安全以及可提高安全性和可靠性的其他形式的代码准确性。事实上，代码管理的概念是运行时的基本原则。以运行时为目标的代码称为托管代码，而不以运行时为目标的代码称为非托管代码。类库是一个综合性的面向对象的可重用类型集合，可以使用它开发多种应用程序，这些应用程序包括传统的命令行或图形用户界面（Graphical User Interface，GUI）应用程序，也包括基于ASP.NET所提供的最新创新的应用程序。
			- .NET Framework可由非托管组件承载，这些组件将公共语言运行库加载到它们的进程中并启动托管代码的执行，从而创建一个可以同时利用托管和非托管功能的软件环境。.NET Framework不但提供若干个运行时宿主，而且还支持第三方运行时宿主的开发。
			- 用C# 编写的源代码被编译为一种符合CLI规范的中间语言（Intermediate Language，IL）。IL代码与资源（如位图和字符串）一起作为一种称为程序集的可执行文件存储在磁盘上，通常具有的扩展名为.exe或.dll。程序集包含清单，它提供关于程序集的类型、版本、区域性和安全要求等信息。
			- 执行C# 程序时，程序集将加载到CLR中，这可能会根据清单中的信息执行不同的操作。然后，如果符合安全要求，CLR执行实时（Just In Time，JIT）编译以将IL代码转换为本机机器指令。CLR还提供与自动垃圾回收、异常处理和资源管理有关的其他服务。由CLR执行的代码有时称为“托管代码”，它与编译为面向特定系统的本机机器语言的“非托管代码”相对应。
			- 语言互操作性是.NET Framework的一个关键功能。因为由C# 编译器生成的IL代码符合CTS的规范，因此从C# 生成的IL代码可以与从Visual Basic、Visual C++、Visual J# 的.NET版本或者其他二十多种符合CTS的语言中的任何一种生成的代码进行交互。单一程序集可能包含用不同.NET语言编写的多个模块，并且类型可以相互引用，就像它们是用同一种语言编写的。
		- ### 1.4.2 .NET Framework类库
		  collapsed:: true
			- 除了运行时服务外，.NET Framework还包含一个由四千多个类组成的和任何.NET语言协同工作的内容详尽的库——.NET Framework类库，这些类被组织为命名空间，为从文件输入和输出到字符串操作、到XML解析、到Windows窗体控件的所有内容提供多种有用的功能。比如对数据库支持的类库以及线程的类库等。典型的C# 应用程序使用.NET Framework类库广泛地处理常见的“日常”任务。
	- ## 1.5 Visual Studio 2012 简介
	- ## 1.6 第一个C# 程序
	  collapsed:: true
	  
	  collapsed:: true
		- ### 1.6.3 编译和运行C# 控制台应用程序
		  collapsed:: true
			- C# 语言运行时要经过两次编译，第一次编译是将源代码编译为MSIL（Microsoft Intermediate Language，微软中间语言）。
			- 当程序运行时MSIL代码载入内存时会进行第二次编译，中间语言会编译为机器语言以供计算机调用，第二次编译只在载入内存时发生，编译的结果被储存起来以备重复利用。编译时是按需编译，即只编译所用到的代码，而不是全部程序，称为JIT（即时编译）。
			- 如图1-17所示，在Visual Studio 2012的菜单栏中依次选择“生成”→“生成解决方案”选项（快捷键为F6）。如果Visual Studio的状态栏中显示“生成成功”，就表示代码没有编译错误。
			- ![image.png](../assets/image_1687240246114_0.png)
			- 图1-17　编译程序
			- 在Visual Studio 2012菜单栏中依次选择“调试”→“开始执行（不调试）”选项（快捷键为Ctrl+F5），或选择“启动调试”选项（快捷键为F5）。运行后结果如图1-18所示，在控制台输出了“Hello World！”。
			- 至此，第一个C# 程序就编译并运行成功了！
	- ## 1.7 应用程序结构
	  collapsed:: true
		- ### 1.7.1　控制台应用程序文件夹结构
		  collapsed:: true
			- 以上一例子来说，在建立项目时，Visual Studio已经在“F:\项目\练习小程序\C# \”文件夹下创建了一个与ConsoleHelloWorld项目同名的文件，此文件夹被叫作解决方案文件夹，解决方案和项目都是Visual Studio提供的有效管理应用程序的容器，一个解决方案可以包含一个或多个项目。在ConsoleHelloWorld文件夹下有一个名为ConsoleHelloWorld.sln的文件，此文件为Visual Studio的解决方案文件，还有一个与解决方案同名的文件夹，此文件夹为项目文件夹。在项目文件夹中包含比较重要的文件夹——bin文件夹，此文件夹下用于存放Visual Studio编译后生成的可执行文件等。此外，还有一个名为Program.cs的文件，该文件是项目的入口文件（启动文件），该文件中定义了项目的启动入口（Main()方法），在C# 中程序的源文件以.cs作为扩展名。
			- Visual Studio提供了一个叫“解决方案资源管理器”的窗口，在这里可以管理解决方案中包含的所有文件。如图1-19所示，单击解决方案资源管理器中的“显示所有文件”按钮，就可以看到解决方案下的程序结构了。
			- ![image.png](../assets/image_1687240439299_0.png)
			- 图1-19　解决方案资源管理器
		- ### 1.7.2　C# 程序结构
		  collapsed:: true
			- C# 程序结构大致可分为注释、命名空间、类、Main方法等。还以上述例子来说，下面就由外向内分析上述代码。
			- **1．using关键字与namespace关键字**
			  collapsed:: true
				- C# 中using关键字用来引入其他命名空间，它的作用与Java中的import类似。最上面5条using语句在模板生成时Visual Studio就已经自动添加了。
				- C# 程序中组织代码是用namespace（命名空间）的形式来实现的，通过命名空间来分类，以区别不同的代码功能，同时也是VS.NET中所有类的完全名称的一部分。如果要调用某个命名空间中的类或方法，首页需要使用using指令引入相应的命名空间，从而可以直接使用每个被导入的类型的标识符，而不必加上它们的完全限定名。
			- **2．class关键字**
			  collapsed:: true
				- class关键字表示类，类是一种数据结构，C# 中所有的语句都必须位于类内。所以，类是C# 语言的核心和基本构成模块。类要包含在一个命名空间中，在创建项目时Visual Studio自动创建了一个名为Program的类。在Main方法中使用的Console类表示控制台应用程序的标准输入流、输出流和错误流。此类不能被继承。它的属性、方法与事件如表1-1所示。
				- 表1-1 Console类的属性、方法和事件
				  background-color:: blue
				  collapsed:: true

					
					  |----|----|
					  |**属性** |
					  |名称|说明|
					  |BackgroundColor|获取或设置控制台的背景色|
					  |BufferHeight|获取或设置缓冲区的高度|
					  |BufferWidth|获取或设置缓冲区的宽度|
					  |CapsLock|获取一个值，该值指示Caps Lock键盘切换键是打开的还是关闭的|
					  |CursorLeft|获取或设置光标在缓冲区中的列位置|
					  |CursorSize|获取或设置光标在字符单元格中的高度|
					  |CursorTop|获取或设置光标在缓冲区中的行位置|
					  |CursorVisible|获取或设置一个值，用以指示光标是否可见|
					  |Error|获取标准错误输出流|
					  |ForegroundColor|获取或设置控制台的前景色|
					  |In|获取标准输入流|
					  |InputEncoding|获取或设置控制台用于读取输入的编码|
					  |IsErrorRedirected|获取指示错误输出流是否已经从标准错误流被再定位的值|
					  |IsInputRedirected|获取指示输入是否已从标准输入流中重定向的值|
					  |IsOutputRedirected|获取指示输出是否已从标准输入流中重定向的值|
					  |KeyAvailable|获取一个值，该值指示按键操作在输入流中是否可用|
					  |LargestWindowHeight|根据当前字体和屏幕分辨率获取控制台窗口可能具有的最大行数|
					  |LargestWindowWidth|根据当前字体和屏幕分辨率获取控制台窗口可能具有的最大列数|
					  |NumberLock|获取一个值，该值指示Num Lock键盘切换键是打开的还是关闭的|
					  |Out|获取标准输出流|
					  |OutputEncoding|获取或设置控制台用于写入输出的编码|
					  |Title|获取或设置要显示在控制台标题栏中的标题|
					  |TreatControlCAsInput|获取或设置一个值，该值指示是将Ctrl+C键视为普通输入，还是视为由操作系统处理的中断|
					  |WindowHeight|获取或设置控制台窗口区域的高度|
					  |WindowLeft|获取或设置控制台窗口区域的最左边相对于屏幕缓冲区的位置|
					  |WindowTop|获取或设置控制台窗口区域的最顶部相对于屏幕缓冲区的位置|
					  |WindowWidth|获取或设置控制台窗口的宽度|
					  |**方法**|
					  |名称|说明|
					  |Beep()|通过控制台扬声器播放提示音|
					  |Beep(Int32, Int32)|通过控制台扬声器播放具有指定频率和持续时间的提示音|
					  |Clear|清除控制台缓冲区和相应的控制台窗口的显示信息|
					  |MoveBufferArea(Int32, Int32, Int32, Int32, Int32, Int32)|将屏幕缓冲区的指定源区域复制到指定的目标区域|
					  |MoveBufferArea(Int32, Int32, Int32, Int32, Int32, Int32, Char, ConsoleColor, ConsoleColor)|将屏幕缓冲区的指定源区域复制到指定的目标区域|
					  |OpenStandardError()|获取标准错误流|
					  |OpenStandardError(Int32)|获取设置为指定缓冲区大小的标准错误流|
					  |OpenStandardInput()|获取标准输入流|
					  |OpenStandardInput(Int32)|获取设置为指定缓冲区大小的标准输入流|
					  |OpenStandardOutput()|获取标准输出流|
					  |OpenStandardOutput(Int32)|获取设置为指定缓冲区大小的标准输出流|
					  |Read|从标准输入流读取下一个字符|
					  |ReadKey()|获取用户按下的下一个字符或功能键。按下的键显示在控制台窗口中|
					  |ReadKey(Boolean)|获取用户按下的下一个字符或功能键。按下的键可以选择显示在控制台窗口中|
					  |ReadLine|从标准输入流读取下一行字符|
					  |ResetColor|将控制台的前景色和背景色设置为默认值|
					  |SetBufferSize|将屏幕缓冲区的高度和宽度设置为指定值|
					  |SetCursorPosition|设置光标位置|
					  |SetError|将Error属性设置为指定的TextWriter对象|
					  |SetIn|将In属性设置为指定的TextReader对象|
					  |SetOut|将Out属性设置为指定的TextWriter对象|
					  |SetWindowPosition|设置控制台窗口相对于屏幕缓冲区的位置|
					  |SetWindowSize|将控制台窗口的高度和宽度设置为指定值|
					  |Write(Boolean)|将指定的布尔值的文本表示形式写入标准输出流|
					  |Write(Char)|将指定的Unicode字符值写入标准输出流|
					  |Write(Char[])|将指定的Unicode字符数组写入标准输出流|
					  |Write(Decimal)|将指定的Decimal值的文本表示形式写入标准输出流|
					  |Write(Double)|将指定的双精度浮点值的文本表示形式写入标准输出流|
					  |Write(Int32)|将指定的32位有符号整数值的文本表示写入标准输出流|
					  |Write(Int64)|将指定的64位有符号整数值的文本表示写入标准输出流|
					  |Write(Object)|将指定对象的文本表示形式写入标准输出流|
					  |Write(Single)|将指定的单精度浮点值的文本表示形式写入标准输出流|
					  |Write(String)|将指定的字符串值写入标准输出流|
					  |Write(UInt32)|将指定的32位无符号整数值的文本表示写入标准输出流|
					  |Write(UInt64)|将指定的64位无符号整数值的文本表示写入标准输出流|
					  |Write(String, Object)|使用指定的格式信息将指定对象的文本表示形式写入标准输出流|
					  |Write(String, Object[])|使用指定的格式信息将指定的对象数组的文本表示形式写入标准输出流|
					  |Write(Char[], Int32, Int32)|将指定的Unicode字符子数组写入标准输出流|
					  |Write(String, Object, Object)|使用指定的格式信息将指定对象的文本表示形式写入标准输出流|
					  |Write(String, Object, Object, Object)|使用指定的格式信息将指定对象的文本表示形式写入标准输出流|
					  |Write(String, Object, Object, Object, Object)|使用指定的格式信息将指定的对象和可变长度参数列表的文本表示形式写入标准输出流|
					  |WriteLine()|将当前行终止符写入标准输出流|
					  |WriteLine(Boolean)|将指定布尔值的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(Char)|将指定的Unicode字符值（后跟当前行终止符）写入标准输出流|
					  |WriteLine(Char[])|将指定的Unicode字符数组（后跟当前行终止符）写入标准输出流|
					  |WriteLine(Decimal)|将指定的Decimal值的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(Double)|将指定的双精度浮点值的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(Int32)|将指定的32位有符号整数值的文本表示（后跟当前行的结束符）写入标准输出流|
					  |WriteLine(Int64)|将指定的64位有符号整数值的文本表示（后跟当前行的结束符）写入标准输出流|
					  |WriteLine(Object)|将指定对象的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(Single)|将指定的单精度浮点值的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(String)|将指定的字符串值（后跟当前行终止符）写入标准输出流|
					  |WriteLine(UInt32)|将指定的32位无符号的整数值的文本表示（后跟当前行的结束符）写入标准输出流|
					  |WriteLine(UInt64)|将指定的64位无符号的整数值的文本表示（后跟当前行的结束符）写入标准输出流|
					  |WriteLine(String, Object)|使用指定的格式信息，将指定对象（后跟当前行终止符）的文本表示形式写入标准输出流|
					  |WriteLine(String, Object[])|使用指定的格式信息，将指定的对象数组（后跟当前行终止符）的文本表示形式写入标准输出流|
					  |WriteLine(Char[], Int32, Int32)|将指定的Unicode字符子数组（后跟当前行终止符）写入标准输出流|
					  |WriteLine(String, Object, Object)|使用指定的格式信息，将指定对象的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(String, Object, Object, Object)|使用指定的格式信息，将指定对象的文本表示形式（后跟当前行终止符）写入标准输出流|
					  |WriteLine(String, Object, Object, Object, Object)|使用指定的格式信息，将指定的对象和可变长度参数列表（后跟当前行终止符）的文本表示形式写入标准输出流|
					  |**事　件**|
					  |名　称|说　明|
					  |CancelKeyPress|当Ctrl键和C键或Break键同时按住（Ctrl+C或Ctrl+Break）|
			- **3. Main方法**
			  collapsed:: true
				- Main方法是程序的入口方法，C# 控制台程序中必须包含且只能包含一个Main方法，并且Main方法必须为**静态方法（即必须使用static修饰符）**，C# 是面向对象的编程语言，**静态方法可以不依赖于类的实例对象而执行，这样才能在程序启动还没有创建类的对象时被执行了。**在该方法中可以创建对象和调用其他方法等。
				- Main方法有如下4种形式。
				- ```c#
				  static void Main (string[] args){}
				  static int Main (string[] args){}
				  static void Main (){}
				  static int Main (){}
				  ```
				- 可以根据自己的需要进行选择使用。
			- **4. 注释**
			  collapsed:: true
				- 注释是用来对某行或某段代码进行说明，方便日后对代码的维护。编译器编译程序时不执行注释的代码和文字。由于软件的复杂性以及不可预知性，所以在程序当中添加注释是一个非常明智的选择，尤其是在团队开发当中，可以使自己的程序更加适于阅读。
				- C# 中注释分为如下三种。
				  collapsed:: true
					- （1）单选注释：使用“//”进行注释，只可注释一行。
					- （2）多行注释：以“/\*”开始，“\*/”为结束，中间部分为要注释的内容。
					- （3）文档注释：使用“///”进行注释，此注释在类或方法前面，连续输入三个“/”，用于对类和方法进行注释。
				- 为上例代码添加注释，添加注释后的代码如下。
				- ```c#
				  /*
				       此程序演示了使用Console输出Hello World！字符串
				       此程序的重点在于Console类的使用
				       */
				      using System;
				      using System.Collections.Generic;
				      using System.Linq;
				      using System.Text;
				      using System.Threading.Tasks;
				      
				      namespace ConsoleHelloWorld
				      {
				          class Program
				          {
				              /// <summary>
				              /// 我是程序的入口方法
				              /// </summary>
				   			/// <param name="args"></param>
				              static void Main(string[] args)
				              {
				                  Console.WriteLine("Hello World！");  //输出Hello World！
				                  Console.ReadLine();                 
				              }
				          }
				      }
				  ```
				- 运行效果与未添加注释时效果相同。
-