FrameworkElement类继承于UIElement类，继承关系是：Object->DispatcherObject->DependencyObject->Visual->UIElement->FrameworkElement，它也是WPF控件的众多父类中最核心的基类，从这里开始，继承树开始分支，分别是Shape图形类、Control控件类和Panel布局类三个方向。

![[assets/WPF从小白到大佬：控件父类：：FrameworkElement类/6fbb357e3242781aadc559b84566df13_MD5.jpg]]

FrameworkElement类本质上也是提供了一系列属性、方法和事件。同时又扩展 UIElement 并添加了以下功能：

> 官方文档
> 
> _**1.布局系统定义**： FrameworkElement 为中 UIElement定义为虚拟成员的某些方法提供特定的 WPF 框架级实现。 最值得注意的是， FrameworkElement 会密封某些 WPF 核心级布局替代，并改为提供派生类应替代的 WPF 框架级别的等效项。 例如，密封但 FrameworkElementArrangeCore 提供 ArrangeOverride。 这些更改反映了这样一个事实，即在 WPF 框架级别，有一个可以呈现任何 FrameworkElement 派生类的完整布局系统。 在 WPF 核心级别，将构建基于 WPF 的常规布局解决方案的某些成员已就位，但未定义布局系统的实际引擎。  
> **2.逻辑树**： 常规 WPF 编程模型通常以元素树的方式表示。 支持将元素树表示为逻辑树，以及支持在标记中定义该树的支持是在 级别实现的 FrameworkElement 。 但请注意， FrameworkElement 故意不定义内容模型，并将该责任留给派生类。  
> **3.对象生存期事件**： 了解何时初始化元素 (调用构造函数) 或首次将元素加载到逻辑树中时，这通常很有用。 FrameworkElement 定义多个与对象生存期相关的事件，这些事件为涉及元素的代码隐藏操作（例如添加更多子元素）提供有用的挂钩。  
> **4.支持数据绑定和动态资源引用**： 对数据绑定和资源的属性级支持由 DependencyProperty 类实现，并体现在属性系统中，但解析存储为 Expression (数据绑定和动态资源的编程构造) 中存储的成员值的能力由 FrameworkElement实现。  
> **5.风格**：FrameworkElement 定义 Style 属性。 但是， FrameworkElement 尚未定义对模板的支持或支持修饰器。 这些功能由控件类（如 和 ContentControl）Control引入。  
> **6.更多动画支持**： 某些动画支持已在 WPF 核心级别定义，但 FrameworkElement 通过实现 BeginStoryboard 和相关成员扩展了此支持。_

我们来看看这个基类的结构定义：

```cs
public class FrameworkElement : UIElement, IFrameworkInputElement, IInputElement, ISupportInitialize, IHaveResources, IQueryAmbient
{
    public static readonly DependencyProperty StyleProperty;
    public static readonly DependencyProperty MaxHeightProperty;
    public static readonly DependencyProperty FlowDirectionProperty;
    public static readonly DependencyProperty MarginProperty;
    public static readonly DependencyProperty HorizontalAlignmentProperty;
    public static readonly DependencyProperty VerticalAlignmentProperty;
    public static readonly DependencyProperty FocusVisualStyleProperty;
    public static readonly DependencyProperty CursorProperty;
    public static readonly DependencyProperty ForceCursorProperty;
    public static readonly RoutedEvent UnloadedEvent;
    public static readonly DependencyProperty ToolTipProperty;
    public static readonly DependencyProperty ContextMenuProperty;
    public static readonly RoutedEvent ToolTipOpeningEvent;
    public static readonly RoutedEvent ToolTipClosingEvent;
    public static readonly RoutedEvent ContextMenuOpeningEvent;
    public static readonly RoutedEvent ContextMenuClosingEvent;
    public static readonly DependencyProperty MinHeightProperty;
    public static readonly DependencyProperty HeightProperty;
    public static readonly RoutedEvent LoadedEvent;
    public static readonly DependencyProperty MinWidthProperty;
    public static readonly DependencyProperty MaxWidthProperty;
    public static readonly DependencyProperty OverridesDefaultStyleProperty;
    public static readonly DependencyProperty UseLayoutRoundingProperty;
    public static readonly DependencyProperty BindingGroupProperty;
    public static readonly DependencyProperty LanguageProperty;
    public static readonly DependencyProperty NameProperty;
    public static readonly DependencyProperty TagProperty;
    public static readonly DependencyProperty DataContextProperty;
    public static readonly RoutedEvent RequestBringIntoViewEvent;
    public static readonly RoutedEvent SizeChangedEvent;
    public static readonly DependencyProperty ActualWidthProperty;
    public static readonly DependencyProperty ActualHeightProperty;
    public static readonly DependencyProperty LayoutTransformProperty;
    public static readonly DependencyProperty InputScopeProperty;
    public static readonly DependencyProperty WidthProperty;
    protected internal static readonly DependencyProperty DefaultStyleKeyProperty;
 
    public FrameworkElement();
 
    public Transform LayoutTransform { get; set; }
    public double Width { get; set; }
    public double MinWidth { get; set; }
    public double MaxHeight { get; set; }
    public double Height { get; set; }
    public double MinHeight { get; set; }
    public double ActualHeight { get; }
    public double MaxWidth { get; set; }
    public double ActualWidth { get; }
    public TriggerCollection Triggers { get; }
    public object Tag { get; set; }
    public string Name { get; set; }
    public XmlLanguage Language { get; set; }
    public BindingGroup BindingGroup { get; set; }
    public object DataContext { get; set; }
    public ResourceDictionary Resources { get; set; }
    public DependencyObject TemplatedParent { get; }
    public bool UseLayoutRounding { get; set; }
    public FlowDirection FlowDirection { get; set; }
    public InputScope InputScope { get; set; }
    public Thickness Margin { get; set; }
    public Style Style { get; set; }
    public VerticalAlignment VerticalAlignment { get; set; }
    public bool OverridesDefaultStyle { get; set; }
    public HorizontalAlignment HorizontalAlignment { get; set; }
    public ContextMenu ContextMenu { get; set; }
    public object ToolTip { get; set; }
    public DependencyObject Parent { get; }
    public bool IsInitialized { get; }
    public bool ForceCursor { get; set; }
    public Cursor Cursor { get; set; }
    public Style FocusVisualStyle { get; set; }
    public bool IsLoaded { get; }
    protected override int VisualChildrenCount { get; }
    protected internal InheritanceBehavior InheritanceBehavior { get; set; }
    protected internal virtual IEnumerator LogicalChildren { get; }
    protected internal object DefaultStyleKey { get; set; }
 
    public event ToolTipEventHandler ToolTipClosing;
    public event ToolTipEventHandler ToolTipOpening;
    public event RoutedEventHandler Unloaded;
    public event DependencyPropertyChangedEventHandler DataContextChanged;
    public event SizeChangedEventHandler SizeChanged;
    public event RequestBringIntoViewEventHandler RequestBringIntoView;
    public event EventHandler<DataTransferEventArgs> SourceUpdated;
    public event EventHandler<DataTransferEventArgs> TargetUpdated;
    public event RoutedEventHandler Loaded;
    public event EventHandler Initialized;
    public event ContextMenuEventHandler ContextMenuClosing;
    public event ContextMenuEventHandler ContextMenuOpening;
 
    public static FlowDirection GetFlowDirection(DependencyObject element);
    public static void SetFlowDirection(DependencyObject element, FlowDirection value);
    public bool ApplyTemplate();
    public virtual void BeginInit();
    public void BeginStoryboard(Storyboard storyboard, HandoffBehavior handoffBehavior, bool isControllable);
    public void BeginStoryboard(Storyboard storyboard);
    public void BeginStoryboard(Storyboard storyboard, HandoffBehavior handoffBehavior);
    public void BringIntoView();
    public void BringIntoView(Rect targetRectangle);
    public virtual void EndInit();
    public object FindName(string name);
    public object FindResource(object resourceKey);
    public BindingExpression GetBindingExpression(DependencyProperty dp);
    public sealed override bool MoveFocus(TraversalRequest request);
    public virtual void OnApplyTemplate();
    public sealed override DependencyObject PredictFocus(FocusNavigationDirection direction);
    public void RegisterName(string name, object scopedElement);
    public BindingExpressionBase SetBinding(DependencyProperty dp, BindingBase binding);
    public BindingExpression SetBinding(DependencyProperty dp, string path);
    public void SetResourceReference(DependencyProperty dp, object name);
    public bool ShouldSerializeResources();
    public bool ShouldSerializeStyle();
    public bool ShouldSerializeTriggers();
    public object TryFindResource(object resourceKey);
    public void UnregisterName(string name);
    public void UpdateDefaultStyle();
    protected sealed override void ArrangeCore(Rect finalRect);
    protected virtual Size ArrangeOverride(Size finalSize);
    protected override Geometry GetLayoutClip(Size layoutSlotSize);
    protected override Visual GetVisualChild(int index);
    protected sealed override Size MeasureCore(Size availableSize);
    protected virtual Size MeasureOverride(Size availableSize);
    protected virtual void OnContextMenuClosing(ContextMenuEventArgs e);
    protected virtual void OnContextMenuOpening(ContextMenuEventArgs e);
    protected override void OnGotFocus(RoutedEventArgs e);
    protected virtual void OnInitialized(EventArgs e);
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e);
    protected virtual void OnToolTipClosing(ToolTipEventArgs e);
    protected virtual void OnToolTipOpening(ToolTipEventArgs e);
    protected internal void AddLogicalChild(object child);
    protected internal DependencyObject GetTemplateChild(string childName);
    protected internal override DependencyObject GetUIParentCore();
    protected internal override void OnRenderSizeChanged(SizeChangedInfo sizeInfo);
    protected internal virtual void OnStyleChanged(Style oldStyle, Style newStyle);
    protected internal override void OnVisualParentChanged(DependencyObject oldParent);
    protected internal virtual void ParentLayoutInvalidated(UIElement child);
    protected internal void RemoveLogicalChild(object child);

}
```

**属性分析**

1.LayoutTransform 属性：获取或设置在执行布局时应应用于此元素的图形转换。这个属性与UIElement类中的RenderTransform属性有相似之处，所以我们在此将两者进行对比说明一下。两个属性都是Transform类型，而Transform是一个抽象类，这个类可以实现控件在平面中的各种转换，包括

- 旋转 (System.Windows.Media.RotateTransform)
- 缩放 (System.Windows.Media.ScaleTransform)、
- 倾斜 (System.Windows.Media.SkewTransform) 、
- 平移 (System.Windows.Media.TranslateTransform)。

虽然两个属性都可以达到控件的变换效果，但是两者还是有区别的。LayoutTransform属性是在控件布局之前对控件进行变换，而RenderTransform属性是在布局结束后执行控件的变换，LayoutTransform开销比RenderTransform要大，所以，尽量使用RenderTransform属性去实现控件的变换。（我们会在后面专门探讨控件的变换）

2.Width属性：这是表示控件的宽度。与之相关的还有以下几个属性。

- ActualWidth：获取此元素的呈现宽度。只读属性。
- MaxWidth：获取或设置一个控件的最大宽度。
- MinWidth：获取或设置一个控件的最小宽度。

3.Height属性：这是表示控件的高度，与之相关的还有以下几个属性。

- ActualHeight：获取此元素的呈现高度。只读属性。
- MaxHeight：获取或设置一个控件的最大高度。
- MinHeight：获取或设置一个控件的最小高度。

4.Tag属性：这个属性非常重要，它是object类型，意味着可以保存任意类型的对象值。它就像FrameworkElement类身上的一个小口袋，但确能容纳万物。我们通常会将一些与控件相关的数据临时存放在Tag属性中，当把控件作为参数传递时，小口袋里面的对象也随之传递过去了。

5.Name属性：获取或设置控件的标识名称。在同一个窗体、页、用户控件中，Name标识是唯一的。设置了控件的名称后，我们就可以在后端代码直接以这个标识去引用控件。

6.Margin属性：获取或设置控件的外边距。如下所示，我们定义了一个button的margin，距离左边、上边、右边和下边的像素分别是20、40、60、80。

```html
<Grid>
     <Button Content="WPF中文网" Margin="20 40 60 80" />
</Grid>
```

![[assets/WPF从小白到大佬：控件父类：：FrameworkElement类/27a383d360e13525e184b9c735087e67_MD5.jpg]]

> Padding属性说明
> 
> 与Margin相对应的是Padding，表示设置控件的内边距。但是这个属性并不在FrameworkElement中，而在Control类中，从本节第一张图所示，说明只有内容控件才具有Padding，而Shape和Panel是没有Padding属性的。

7.HorizontalAlignment属性：设置控件的水平对齐方式。这个对齐方式是相对于父元素而言的，比如我们有一个Button控件，在外面还包裹了一层Grid控件，那么，设置Button控件的HorizontalAlignment属性，可以将Button控件分别显示在Grid控件的左边、中间、右边三个位置。

8.VerticalAlignment属性：设置控件的垂直对齐方式。与HorizontalAlignment属性类似，只是对方的方向不同，可以设置控件在垂直方向上是居于顶部、中间、还是底部三个位置。

总结：上述两个属性的值都是枚举型，它们都有一个共同的值，那就是stretch，表示是拉伸的方式填充父元素的布局。

9.ToolTip属性：获取或设置用户界面 (UI) 中为此元素显示的工具提示对象。指鼠标移到控件上方时显示的提示内容，它是一个object类型，意味着可以显示任意布局外观。

10.Parent属性：获取此元素的逻辑父元素。它是一个只读属性。

接下来，我们将**介绍几个比较重要的属性**，这些属性是WPF框架中非常核心的知识概念，需要单独形成章节来学习，在这里，我们只是通过这些属性来引出其概念。

**WPF样式**（Style）

什么是样式？简单来说，是指控件呈现时的样子。比如我们上班时会穿上工作服，休假时会穿上更个性化的衣服，我还是那个我，但是身上的衣服却不同，不管是颜色、款式，甚至包括我们的头饰、妆容，都会有所不同。

对于控件而言，同样都是button按钮，有的按钮是方的，有的是圆的，有的是蓝色，有的是红色，有的有文字，有的有图标，如果做到这些不同的样式呢？答案是Style属性。

11.Style属性：获取或设置此元素呈现时所使用的样式。（关于Style样式我们会专门拿一章节来探讨）

与Style相关的还有一个属性，叫FocusVisualStyle，顾名思义，控件在获得焦点时的样式。

**WPF资源**（ResourceDictionary）

什么是资源？资源，也就是资源字典，也就是ResourceDictionary类，它提供一个哈希表/字典实现，其中包含组件所使用的 WPF 资源以及 WPF 应用程序的其他元素。我们可以把WPF的控件、窗体、Application应用所用到的一切资源都放在其中，将多个ResourceDictionary元素合并起来形成一个ResourceDictionary元素（ResourceDictionary也是一个隐式集合）。所以FrameworkElement类设计一个资源属性。

12.Resources 属性：获取或设置本地定义的资源字典。(关于Resources资源我们会专门拿一章节来探讨）

**WPF的数据上下文**（DataContext）

我们曾经在前面的《DependencyObject类》一文中提到过数据驱动模式，控件的值绑定某个“变量”，当“变量”的值发生改变，控件的值也跟着改变，反过来说，当控件的值发生改变，“变量”的值也跟着改变。那么这个特指的“变量”是什么？它和我们今天要介绍的数据上下文有什么关系？

答案是，这个“变量”其实也是一个属性，且必须是一个属性（重点），它是谁的属性？WPF说，它是某个ViewModel类的属性。

假定我们有一个View窗体，窗体有一个TextBox控件；又假如我们还有一个ViewModel实体，这个实体中有一个叫Name的属性。如果我们要将TextBox控件的Text属性和ViewModel实体的Name属性成功的建立绑定关系，必备的条件是什么？

首先，由于View窗体继承于FrameworkElement类，所以每个窗体（或控件）都有一个叫DataContext的数据上下文属性。所以必备的条件是：**ViewModel实体必须先赋值给View窗体的DataContext，ViewModel的Name属性才能绑定到TextBox控件的Text属性。**换言之，领导之间要先搭好桥，下属和下属才好配合工作。这就是DataContext的概念和用途。（关于DataContext数据上下文我们会专门拿一章节来探讨）

13.DataContext属性：获取或设置元素参与数据绑定时的数据上下文。

14.ContextMenu属性：设置与获取控件的上下文菜单 ，就是鼠标在控件上右键时弹出来的菜单。

15.Cursor属性：获取或设置在鼠标指针位于此元素上时显示的光标。

> 友情提示
> 
> ==上述所介绍的属性，是WPF中所有控件都有的属性哦，所以，学一个FrameworkElement类，就把所有控件都学了30%呢。——重庆教主==

**事件分析**

FrameworkElement类提供了12个事件，一般比较常用的是：Initialized、Loaded、Unloaded、SizeChanged等事件。

**方法成员**

FrameworkElement类还提供了一些方法成员。

1.FindName(String)：表示查找某个元素。比如我们在窗体中要查找某个控件。

2.FindResource(Object)：查找某个资源。如果在调用对象上找不到该资源，则接下来搜索逻辑树中的父元素，然后搜索应用程序、主题，最后搜索系统资源。实在找不到就抛出异常。

3.TryFindResource(Object)：尝试去找某个资源。建议使用这个方法。

4.RegisterName (string , object );注册控件的名称到父控件上。

```cs
button2 = new Button();
button2.Name = "Button2";
            
// 注册button2的名称到myMainPanel控件上
myMainPanel.RegisterName(button2.Name, button2);
button2.Content = "Button 2";
button2.Click += new RoutedEventHandler(button2Clicked);
myMainPanel.Children.Add(button2);
```

5.`SetBinding(DependencyProperty, BindingBase)`和`SetBinding(DependencyProperty, String)`，这两个成员都和绑定相关，我们将在后面做专题介绍。

> 最后，我们来看哪些类会继承这个FrameworkElement基类，以便于了解我们接下来要学哪些内容。
> 
> [Microsoft.Windows.Themes.BulletChrome](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.windows.themes.bulletchrome?view=windowsdesktop-7.0)  
> [Microsoft.Windows.Themes.ScrollChrome](https://learn.microsoft.com/zh-cn/dotnet/api/microsoft.windows.themes.scrollchrome?view=windowsdesktop-7.0)  
> [System.Windows.Controls.AccessText](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.accesstext?view=windowsdesktop-7.0)  
> [System.Windows.Controls.AdornedElementPlaceholder](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.adornedelementplaceholder?view=windowsdesktop-7.0)  
> [System.Windows.Controls.ContentPresenter](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.contentpresenter?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Control](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.control?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Decorator](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.decorator?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Image](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.image?view=windowsdesktop-7.0)  
> [System.Windows.Controls.InkCanvas](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.inkcanvas?view=windowsdesktop-7.0)  
> [System.Windows.Controls.ItemsPresenter](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.itemspresenter?view=windowsdesktop-7.0)  
> [System.Windows.Controls.MediaElement](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.mediaelement?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Page](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.page?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Panel](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.panel?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Primitives.DocumentPageView](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.primitives.documentpageview?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Primitives.GridViewRowPresenterBase](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.primitives.gridviewrowpresenterbase?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Primitives.Popup](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.primitives.popup?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Primitives.TickBar](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.primitives.tickbar?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Primitives.Track](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.primitives.track?view=windowsdesktop-7.0)  
> [System.Windows.Controls.TextBlock](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.textblock?view=windowsdesktop-7.0)  
> [System.Windows.Controls.ToolBarTray](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.toolbartray?view=windowsdesktop-7.0)  
> [System.Windows.Controls.Viewport3D](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.controls.viewport3d?view=windowsdesktop-7.0)  
> [System.Windows.Documents.Adorner](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.documents.adorner?view=windowsdesktop-7.0)  
> [System.Windows.Documents.AdornerLayer](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.documents.adornerlayer?view=windowsdesktop-7.0)  
> [System.Windows.Documents.DocumentReference](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.documents.documentreference?view=windowsdesktop-7.0)  
> [System.Windows.Documents.FixedPage](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.documents.fixedpage?view=windowsdesktop-7.0)  
> [System.Windows.Documents.Glyphs](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.documents.glyphs?view=windowsdesktop-7.0)  
> [System.Windows.Documents.PageContent](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.documents.pagecontent?view=windowsdesktop-7.0)  
> [System.Windows.Interop.HwndHost](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.interop.hwndhost?view=windowsdesktop-7.0)  
> [System.Windows.Shapes.Shape](https://learn.microsoft.com/zh-cn/dotnet/api/system.windows.shapes.shape?view=windowsdesktop-7.0)

好，关于FrameworkElement类的成员，我们就先讲这么多，在介绍完WPF的父类之后，下一章，我们正式开始学习WPF控件。

——重庆教主 2023年8月16日