每个控件都有自己的视觉外观，比如，我们一眼就能分清楚Button和CheckBox两个按钮。为什么？那是因为这两种按钮呈现出来的外观完全不一样。WPF为每一种控件都提供了一个默认的视觉外观，同时支持开发者去重写这个视觉外观，只需要将重写的视觉外观赋值到Template属性即可——这就是Template模板的由来。

模块定义了控件的视觉外观。

在前面的章节中，我们了解到WPF的控件大致也是可分为好几种，比如以Panel为基类的那些布局控件，以ContentControl为基类的那些内容控件，以ItemsControl为基类的那些集合控件，这些不同种类的控件都有各自的视觉外观，同时也说明它们都有不同的模板。

一谈到“不同的模板”，那肯定有相同的地方，以OOP的思想，这些不同的模板肯定会继承同一个基类。是的没错，WPF的模板基类叫**FrameworkTemplate**，它是一个抽象类，它有三个子类，分别是ControlTemplate(控件模板)、ItemsPanelTemplate（元素面板模板）和DataTemplate（数据模板）。

**ControlTemplate控件模板**用于定义控件的外观，也就是Control基类的Template属性，而绝大多数控件都继承于Control基类，意味着我们都可以去重新定义它们的视觉外观。

**DataTemplate数据模板**即数据的外衣。用于从一个对象中提取数据，并在内容控件或列表控件的各个项中显示数据。比如ContentControl基类中的ContentTemplate属性，或者集合控件ItemsControl基类ItemTemplate属性，它们都是DataTemplate数据模板，用来定义数据的外观（数据的呈现形式）。

**ItemsPanelTemplate元素面板模板**也是用于ItemsControl控件或ItemsControl的子类控件中，因为在集合控件中要考虑每个元素之间的布局方式，所以可以采用ItemsPanelTemplate去定义。ItemsControl基类有一个ItemsPanel属性，它就是一个ItemsPanelTemplate模板。

一、**FrameworkTemplate**基类

```cs
public abstract class FrameworkTemplate : DispatcherObject, INameScope, ISealable, IHaveResources, IQueryAmbient
{
    protected FrameworkTemplate();
 
    public bool IsSealed { get; }
    public FrameworkElementFactory VisualTree { get; set; }
    public TemplateContent Template { get; set; }
    public ResourceDictionary Resources { get; set; }
    public bool HasContent { get; }
 
    public object FindName(string name, FrameworkElement templatedParent);
    public DependencyObject LoadContent();
    public void RegisterName(string name, object scopedElement);
    public void Seal();
    public bool ShouldSerializeResources(XamlDesignerSerializationManager manager);
    public bool ShouldSerializeVisualTree();
    public void UnregisterName(string name);
    protected virtual void ValidateTemplatedParent(FrameworkElement templatedParent);
 
}
```

**FrameworkTemplate**基类有一个VisualTree属性，这是我们首次看到“视觉树”这个关键词，实际上，WPF拥有两棵树，即逻辑树（LogicalTree）和视觉树（VisualTree），并提供了两个帮助类，LogicalTreeHelper 和 VisualTreeHelper。前者提供用于查询逻辑树中的对象的静态帮助器方法。后者提供一些实用工具方法，用于执行涉及可视化树中的节点的常规任务。

接下来，我们将分享这两棵树的内容。

——重庆教主 2023年9月20日