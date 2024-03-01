Image也算是独门独户的控件，因为它也是直接继承于FrameworkElement基类。Image控件，顾名思义，就是图像显示控件。Image类能够加载显示的图片格式有.bmp、.gif、.ico、.jpg、.png、.wdp 和 .tiff。要注意的是，加载.gif动画图片时，仅显示第一帧。如果要显示gif图片，可以在nuget服务器中下载WpfAnimatedGif组件。

**一、Image类的定义**

```cs
public class Image : FrameworkElement, IUriContext, IProvidePropertyFallback
{
    public static readonly DependencyProperty SourceProperty;
    public static readonly RoutedEvent DpiChangedEvent;
    public static readonly DependencyProperty StretchProperty;
    public static readonly DependencyProperty StretchDirectionProperty;
    public static readonly RoutedEvent ImageFailedEvent;
 
    public Image();
 
    public StretchDirection StretchDirection { get; set; }
    public Stretch Stretch { get; set; }
    public ImageSource Source { get; set; }
    protected virtual Uri BaseUri { get; set; }
 
    public event DpiChangedEventHandler DpiChanged;
    public event EventHandler<ExceptionRoutedEventArgs> ImageFailed;
 
    protected override Size ArrangeOverride(Size arrangeSize);
    protected override Size MeasureOverride(Size constraint);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi);
    protected override void OnRender(DrawingContext dc);
 
}
```

**二、属性成员**

|   |   |
|---|---|
|属性名称|说明|
|StretchDirection|枚举型，表示图像缩放的条件，UpOnly表示内容仅在小于父级时缩放；DownOnly表示内容仅大于父级时缩放；Both表示兼容前面两种缩放条件。|
|Stretch|枚举型，表示图像缩放的模式，None表示内容保持其原始大小；Fill表示调整内容大小以填充目标尺寸，且不保留纵横比；Uniform表示在保留纵横比基础上缩放；UniformToFill表示在保留纵横比基础上缩放，同时具有裁剪功能。|
|Source|图像源，其类型为ImageSource。|
|BaseUri|获取或设置基 统一资源标识符 (URI) 为 System.Windows.Controls.Image。|

**三、事件成员**

|   |   |
|---|---|
|事件名称|说明|
|DpiChanged|显示图像的屏幕的 DPI 发生更改后触发。|
|ImageFailed|在图像中失败时触发。|

**四、Image控件分析**

Image控件最关键的就是Source属性——即ImageSource类型。ImageSource是一个抽象类，表示具有高度、宽度及ImageMetadata对象的图像数据源。ImageSource有多个子类，如BitmapFrame、BitmapSource和DrawingImage。所以，我们如果要显示一张图片，需要将图片转化成BitmapSource或DrawingImage实例，赋值给Image控件的Source属性就行了。

**4.1常规图片加载**

例如：http://www.wpfsoft.com/wp-content/uploads/2023/08/2023080309592548.png

我们将上面这张图片下载后，导到入HelloWorld项目的Images目录中，然后就可以在前端代码中显示。

![[assets/WPF从小白到大佬：内容控件：：Image图像控件/3c38744e3bb93d6d66b7dc013a78800f_MD5.jpg]]

```cs
<Image Source="/Images/logo.png" Width="120" Height="120"/>
```

或者

> 统一资源标识Uri
> 
> WPF引入了统一资源标识Uri(Unified Resource Identifier)来标识和访问资源。其中较为常见的情况是用Uri加载图像。Uri表达式的一般形式为：协议+授权+路径，协议：pack://，授权：有两种。  
> 一种用于访问编译时已经知道的文件，用application:///  
> 一种用于访问编译时不知道、运行时才知道的文件，用siteoforigin:///  
> 一般用逗号代替斜杠，也就是改写作application:,和pack:,  
> 路径：分为绝对路径和相对路径。一般选用相对路径，普适性更强

```cs
<Image Source="pack://application:,,,/Images/logo.png" Width="120" Height="120"/>
```

![[assets/WPF从小白到大佬：内容控件：：Image图像控件/c6a33ba0fa70ece2ccd559cff34db748_MD5.jpg]]

明明Source是ImageSource类型，为什么可以接受一个代表图片路径的字符串呢？因为ImageSource类中有一个ToString()重载成员。

**4.2本地图片加载**

接下来，我们把这个张图片放到Debug目录中，试试用另一种方式加载本地图片。

![[assets/WPF从小白到大佬：内容控件：：Image图像控件/97a4d670bbde9fb546fbea057cb9794d_MD5.jpg]]

此时，这张图片并没有导入到项目中，我们来看一下如何加载本地图片。

前端代码

```cs
    <WrapPanel>
        <Image Source="/Images/logo.png" Width="120" Height="120"/>
        <Image x:Name="image2" Width="120" Height="120"/>
    </WrapPanel>
```

后端代码

```cs
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            var path = Environment.CurrentDirectory + "\\" + "logo.png";
            var imageSource = BitmapFrame.Create(new Uri(path), BitmapCreateOptions.None, BitmapCacheOption.OnLoad);
            image2.Source = imageSource;
        }
    }
```

在主窗体的构造函数中，我们获取了图片的完整地址，然后利用BitmapFrame类Create方法成员将本地图片加载进来并返回一个BitmapFrame对象，BitmapFrame对象继承于BitmapSource，所以我们可以将这个实例通过C#代码的方式赋值给image2的Source属性。

![[assets/WPF从小白到大佬：内容控件：：Image图像控件/cc534e4195c351c6b8f549d35aa497e3_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：027-《Image图像控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月29日