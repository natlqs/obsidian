# WPF概述
WPF，全称Windows Presentation Foundation，中文叫Windows呈现基础，WPF也被调侃为“我佩服”。

熟悉XML或HTML网页的小伙伴们，对WPF会感觉到非常的熟悉和亲切。因为它用起来，与开发Web网页前端界面实在没什么两样。

## HTML是什么？

超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。它定义了网页内容的含义和结构。HTML 元素通过“标签”（tag）将文本从文档中引出，标签由在“<”和“>”中包裹的元素名组成，HTML 标签里的元素名不区分大小写。
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>这里是网页的标题，它显示在浏览器的标题栏里</title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
</body>
</html>
```
如上述源码所示，`<html>`和`</html>`是一对标签，代表其中被“包括”起来的内容为网页源代码，`<head>`和`</head>`也是一对标签 ，代表这张网页的头部，您可以在其中定义一些基础数据，例如网页的编码格式，网页的标题等；`<body>`与`</body>`代表网页的“身体”，即展示在浏览器里面供用户阅读的内容。它们通常包含文字、图片、视频等内容。

那么以XMAL语言编写的WPF前端代码是什么样子的呢？
```html
<Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
Title="窗体标题" 
Width="250" Height="100"> 
<!-- 灰色内容表示注释内容 --> 
<Button Name="button">按钮文字</Button> 
</Window>
```

我们对比两份源码，除了关键词不同，在代码的格式和语义结构表述上，基本是一模一样的。我们这一节分享的是WPF的概述内容，故上述源码的具体含义将不在这里阐述。我们要探讨的是WPF是什么，由哪些组件构成，WPF能干什么。

### 一、WPF是什么

没错，它本质上是一个类库，为我们提供了构建Windows桌面应用程序所需的各式各样的类型(class)。当然一个类库还不能称之为框架，实际上，它的长相是这样的：
[[assets/WPF从小白到大佬：前言：：WPF概述/6d155e44fe5e9394dbf5380345b7df75_MD5.jpeg|Open: Pasted image 20240226142353.png]]
![[assets/WPF从小白到大佬：前言：：WPF概述/6d155e44fe5e9394dbf5380345b7df75_MD5.jpeg]]

### 二、WPF的构成

如图所示，WPF被分成上中下三层，处于最底层的Direct3D和User32其实跟WPF一点关系都没有，我们可以把两个组件视为Windows操作系统的一部分。

1995年2月微软买下RenderMorphics公司，于是该公司的创始人Servan Keondjian便顺理成章的成为了微软的人，从Windows95开始主持开发3D图形引擎（Direct3D)。WPF的3D渲染便是基于Direct3D组件的计算；我们再看User32，这是Windows用户界面相关应用程序接口，用于包括Windows处理，基本用户界面等特性，如创建窗口和发送消息。

中间层分别是milcore.dll和Windowcodecs.dll两个类库，其实两个类库本质上也不属于WPF框架，因为它们是属于非托管层的本地类库，也就是说还没进入到.Net Framework体系中。milcore.dll是一个叫图形托管的引擎，它是WPF的渲染核心，包含最主要的媒体集成层 Media Integration Layer (MIL) 的基础支持，作用是封装 Dx 的接口支持 2D 和 3D 的渲染；Windowcodecs.dll主要是对图片的支持，例如WPF对图片的旋转和缩放。

好，最后介绍一下最上层的三个组件，分别是PresentationFramework.dll、PresentationCore.dll和WindowsBase.dll。我们要开发过程中所使用的绝大多数类型(class)都包含在这三个类库中。

首先说一下WindowsBase.dll，它是上层中最基础的类库，如WPF框架的基类DispatcherObject和DependencyObject。需要说明的是：DependencyObject继承于DispatcherObject抽象类。

再看PresentationCore.dll，提供WPF较低层的一些功能。例如Visual和UIElement这两个类都被包含在这个dll库中，UIElement作为Visual的子类，Visual作为DependencyObject，DependencyObject作为DispatcherObject最终抽象基类。Visual主要提供了几何（Geometry）、特效（Effect）、缩放平移旋转（Transform）等功能；UIElement主要提供了一些基础路由事件（RoutedEvent）、以及定义了一些控件的显示、显示裁剪、是否具有焦点、是否启用等属性。

最后是PresentationFramework.dll，它离我们最近，WPF所有绘制在界面上的控件，全部来源于此类库，包括应用程序的窗口、页面、和用于布局的Panel ，各种控件(Control)和 Style样式， 此外还有交互的控制与动画。在这个dll库中，所有的控件都继承于FrameworkElement类，FrameworkElement类则继承于PresentationCore.dll的UIElement类。

于是我们将上述三层中所引用到的dll库合并起来，最终形成了WPF框架。WPF框架一部分引用了操作系统的本地类库，一部分属于它自身的类库，这些自身类库“运行”在.Net Framework框架之上。

### 三、WPF能干什么

WPF是一个UI（Uesr Interface 用户界面）框架，本质上就像画家的纸笔，我们可以利用微软为我们写好的各种类，在开发应用程序时，设计出更友好的界面，为用户提供更优质的软件使用体验。所以，WPF从控件的角度出发，我认为可分为三种，第一是布局类的控件，第二是数据内容控件，第三是图形；而为了更美观的界面效果，WPF提供了模板、样式、触发器、资源等内容，允许程序员像使用css一样去定义同一类型或单个控件的外观样式。

WPF框架还引入了全新的MVVM开发模式，旨在帮助程序员更方便的开发软件。以数据驱动的方式替代过去的事件驱动方式，能过引入绑定的概念，将前后端的代码进行分离，不得不说，这是WPF最赋有革命性的创举。

WPF还能干什么？

WPF提供了一组灵活的图形功能。其图形的缩放与电脑硬件和分辨率无关，意味着它在放大时将不会像jpg图片一样显示模糊，倒是像Png矢量图一样，每个像素都会自动缩放。WPF的坐标系统是双精度型(double) ，意味着控件的转换和不透明度的计算精度更高；WPF还充分利用GPU进行动画渲染绘制，尽量减少CPU的负担。

WPF对图像、音频、视频、文字和版式的支持都十分友好。例如MediaElement 控件能够播放视频和音频，并且其足够灵活，可以用作其他自定义媒体播放器的基础。

——重庆教主 2023年8月7日