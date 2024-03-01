Visual类是WPF框架中第三个父类，主要是为 WPF 中的呈现提供支持，其中包括命中测试、坐标转换和边界框计算。它位于程序集:PresentationCore.dll库文件中，它的命名空间:System.Windows.Media。

> 官方引用
> 
> Visual 类是派生每个 FrameworkElement 对象的基本抽象。 该类还用作在 WPF 中编写新控件的入口点，在 Win32 应用程序模型中，该类在许多方面可视为窗口句柄 (HWND)。Visual 对象是一个核心 WPF 对象，它的主要作用是提供呈现支持。 用户界面控件如 Button 和 TextBox）派生自 Visual 类，并使用该类来保存它们的呈现数据。 Visual 对象为以下项提供支持：  
> 输出显示：呈现视觉对象的持久、序列化的绘图内容。  
> 转换：针对视觉对象执行转换。  
> 剪裁：为视觉对象提供剪裁区域支持。  
> 命中测试：确定坐标或几何形状是否包含在视觉对象的边界内。  
> 边框计算：确定视觉对象的边框。

换句话说，将来我们要学习的Button、TextBox、CheckBox、Gird、ListBox等所有控件都继承了Visual类，控件在绘制到界面的过程中，涉及到转换、裁剪、边框计算等功能，都是使用了Visual父类的功能。我们先来看一下这个类的结构定义。

```cs
public abstract class Visual : DependencyObject, IResource
{
    protected Visual();
 
    protected DependencyObject VisualParent { get; }
    protected virtual int VisualChildrenCount { get; }
    protected internal DoubleCollection VisualYSnappingGuidelines { get; protected set; }
    protected internal Vector VisualOffset { get; protected set; }
    protected internal Geometry VisualClip { get; protected set; }
    protected internal Rect? VisualScrollableAreaClip { get; protected set; }
    protected internal CacheMode VisualCacheMode { get; protected set; }
    protected internal BitmapEffectInput VisualBitmapEffectInput { get; protected set; }
    protected internal BitmapEffect VisualBitmapEffect { get; protected set; }
    protected internal Effect VisualEffect { get; protected set; }
    protected internal Transform VisualTransform { get; protected set; }
    protected internal BitmapScalingMode VisualBitmapScalingMode { get; protected set; }
    protected internal DoubleCollection VisualXSnappingGuidelines { get; protected set; }
    protected internal double VisualOpacity { get; protected set; }
    protected internal EdgeMode VisualEdgeMode { get; protected set; }
    protected internal ClearTypeHint VisualClearTypeHint { get; set; }
    protected internal TextRenderingMode VisualTextRenderingMode { get; set; }
    protected internal TextHintingMode VisualTextHintingMode { get; set; }
    protected internal Brush VisualOpacityMask { get; protected set; }
 
    public DependencyObject FindCommonVisualAncestor(DependencyObject otherVisual);
    public bool IsAncestorOf(DependencyObject descendant);
    public bool IsDescendantOf(DependencyObject ancestor);
    public Point PointFromScreen(Point point);
    public Point PointToScreen(Point point);
    public GeneralTransform2DTo3D TransformToAncestor(Visual3D ancestor);
    public GeneralTransform TransformToAncestor(Visual ancestor);
    public GeneralTransform TransformToDescendant(Visual descendant);
    public GeneralTransform TransformToVisual(Visual visual);
    protected void AddVisualChild(Visual child);
    protected virtual Visual GetVisualChild(int index);
    protected virtual GeometryHitTestResult HitTestCore(GeometryHitTestParameters hitTestParameters);
    protected virtual HitTestResult HitTestCore(PointHitTestParameters hitTestParameters);
    protected virtual void OnDpiChanged(DpiScale oldDpi, DpiScale newDpi);
    protected void RemoveVisualChild(Visual child);
    protected internal virtual void OnVisualChildrenChanged(DependencyObject visualAdded, DependencyObject visualRemoved);
    protected internal virtual void OnVisualParentChanged(DependencyObject oldParent);
 
}
```

**代码分析**

首先，我们可以看到，Visual类继承了DependencyObject类。另外Visual类是一个抽象类，不可以被实例。Visual类提供了一系列的属性和方法。我们在这里捡一些比较重要的分析一下。

VisualParent属性：这个属性表示获取一个可视化父对象。因为XAML的代码结构就是一棵xml树，每个控件都对象几乎都有一个可视化父对象。

VisualChildrenCount属性：获取当前对象的子元素数量。

VisualOffset属性：指当前可视对象的偏移量值。需要注意的是这个属性被声明成protected internal。啥意思呢？VisualOffset属性只能由同一个程序集的其它类访问，或Visual的子类访问。

> protected internal
> 
> `protected internal` 关键字组合是一种成员访问修饰符， 表示受保护的内部成员。

VisualOpacity属性：获取或设置 Visual 的不透明度。

VisualEffect属性：获取或设置要应用于 Visual 的位图效果。

VisualTransform属性：获取或设置 Transform 的 Visual 值。

这些属性都只读为了解Visual类的基础，因为这些属性都被设计成protected internal，我们的控件虽然继承了这个Visual类，但在实际的使用过程中是感知不到这些属性的，自然也不能实操它们。

我们真正能在继承的控件中直接使用的是Visual类中被声明为public的方法成员。它们有以下几个：

- DependencyObject **FindCommonVisualAncestor**(DependencyObject otherVisual); //返回两个可视对象的公共上级。
- bool **IsAncestorOf**(DependencyObject descendant); //确定可视对象是否为后代可视对象的上级。
- bool **IsDescendantOf**(DependencyObject ancestor); //确定可视对象是否为上级可视对象的后代。
- Point **PointFromScreen**(Point point); //将屏幕坐标中的 Point 转换为表示 Point 的当前坐标系的 Visual。
- Point **PointToScreen**(Point point); //将表示 Point 的当前坐标系的 Visual 转换为屏幕坐标中的 Point。
- GeneralTransform2DTo3D **TransformToAncestor**(Visual3D ancestor); //返回一个转换，该转换可用于将 Visual 中的坐标转换为可视对象的指定 Visual3D 上级。
- GeneralTransform **TransformToAncestor**(Visual ancestor); //返回一个转换，该转换可用于将 Visual 中的坐标转换为可视对象的指定 Visual 上级。
- GeneralTransform **TransformToDescendant**(Visual descendant); //返回一个转换，该转换可用于将 Visual 中的坐标转换为指定的可视对象后代。
- GeneralTransform **TransformToVisual**(Visual visual); //返回一个转换，该转换可用于将 Visual 中的坐标转换为指定的可视对象。

由此可见，Visual类所做的事情只为控件呈现相关，但还不是去呈现控件，只是提供呈现的基础。那么，谁又去继承了Visual类，成为继Visual类之后又一个控件的基类呢？答案是UIElement类。

——重庆教主 2023年8月15日