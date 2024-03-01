Selector继承于ItemsControl，但它是一个抽象类，所以不能被实例化。从命名上看，它是一个选择器。

一、Selector类定义

```cs
public abstract class Selector : ItemsControl
{
    public static readonly RoutedEvent SelectionChangedEvent;
    public static readonly RoutedEvent SelectedEvent;
    public static readonly RoutedEvent UnselectedEvent;
    public static readonly DependencyProperty IsSelectionActiveProperty;
    public static readonly DependencyProperty IsSelectedProperty;
    public static readonly DependencyProperty IsSynchronizedWithCurrentItemProperty;
    public static readonly DependencyProperty SelectedIndexProperty;
    public static readonly DependencyProperty SelectedItemProperty;
    public static readonly DependencyProperty SelectedValueProperty;
    public static readonly DependencyProperty SelectedValuePathProperty;
 
    protected Selector();
 
    public object SelectedValue { get; set; }
    public object SelectedItem { get; set; }
    public int SelectedIndex { get; set; }
    public bool? IsSynchronizedWithCurrentItem { get; set; }
    public string SelectedValuePath { get; set; }
 
    public event SelectionChangedEventHandler SelectionChanged;
 
    public static void AddSelectedHandler(DependencyObject element, RoutedEventHandler handler);
    public static void AddUnselectedHandler(DependencyObject element, RoutedEventHandler handler);
    public static bool GetIsSelected(DependencyObject element);
    public static bool GetIsSelectionActive(DependencyObject element);
    public static void RemoveSelectedHandler(DependencyObject element, RoutedEventHandler handler);
    public static void RemoveUnselectedHandler(DependencyObject element, RoutedEventHandler handler);
    public static void SetIsSelected(DependencyObject element, bool isSelected);
    protected override void ClearContainerForItemOverride(DependencyObject element, object item);
    protected override void OnInitialized(EventArgs e);
    protected override void OnIsKeyboardFocusWithinChanged(DependencyPropertyChangedEventArgs e);
    protected override void OnItemsChanged(NotifyCollectionChangedEventArgs e);
    protected override void OnItemsSourceChanged(IEnumerable oldValue, IEnumerable newValue);
    protected virtual void OnSelectionChanged(SelectionChangedEventArgs e);
    protected override void PrepareContainerForItemOverride(DependencyObject element, object item);
 
}
```

接下来，我们来看看它提供了哪些可用的属性。

二、Selector类的属性

|                               |                                                       |
| ----------------------------- | ----------------------------------------------------- |
| 属性名称                          | 说明                                                    |
| SelectedValue                 | 获取或设置SelectedValuePath属性指定的元素的属性值                     |
| SelectedItem                  | 获取或设置当前所选内容中的第一项或如果所选内容为空则返回 null                     |
| SelectedIndex                 | 获取或设置当前所选内容或返回的第一项的索引为负一 (-1) 如果所选内容为空。               |
| SelectedValuePath             | 获取或设置SelectedItem当前元素的某个属性名，这个元素属性名将决定SelectedValue的值 |
| IsSynchronizedWithCurrentItem | 是否同步当前项。                                              |

SelectedItem和SelectedValue有点类似，都是object类型。但是，他们俩不一定指同一个内容。比如，我们将有这样一个数据实体类。

```cs
    public class Person
    {
        public string Name { get; set; }
        public string Address { get; set; }
        public int Age { get; set; }
    }
```

然后我们实例化多个Person组成一个集合绑定到Items属性中，这个时候选中某一个元素，SelectedItem便等于这个Person元素，但是SelectedValue是什么，要看SelectedValuePath的值，如果SelectedValuePath的值指向的是Person.Name，那么SelectedValue就是一个字符串，如果SelectedValuePath指向的是Person的Age ，那么SelectedValue就是一个int整数，**只有**不设置**SelectedValuePath时，SelectedValue和SelectedItem两者才相等，即Person实例**。

具体关于这一个重点，我们在下一节讨论ListBox控件时进行演示和讲解。

> 重庆教主说：
> 
> SelectedItem、SelectedValue、SelectedValuePath非常重要，一定要理解透彻哦！另外，还有一个属性叫DisplayMemberPath，它在ItemsControl基类中，意思是设置要显示的属性名，而SelectedValuePath是设置要选择的属性名。

——重庆教主 2023年9月1日