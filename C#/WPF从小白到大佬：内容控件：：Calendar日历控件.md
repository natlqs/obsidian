Calendar提供一个日历界面，供用户选择日期，它继承于Control基类。

一、Calendar类的定义

```cs
public class Calendar : Control
{
    public static readonly RoutedEvent SelectedDatesChangedEvent;
    public static readonly DependencyProperty SelectionModeProperty;
    public static readonly DependencyProperty SelectedDateProperty;
    public static readonly DependencyProperty FirstDayOfWeekProperty;
    public static readonly DependencyProperty DisplayModeProperty;
    public static readonly DependencyProperty DisplayDateStartProperty;
    public static readonly DependencyProperty IsTodayHighlightedProperty;
    public static readonly DependencyProperty DisplayDateProperty;
    public static readonly DependencyProperty CalendarItemStyleProperty;
    public static readonly DependencyProperty CalendarDayButtonStyleProperty;
    public static readonly DependencyProperty CalendarButtonStyleProperty;
    public static readonly DependencyProperty DisplayDateEndProperty;
 
    public Calendar();
 
    public DateTime? DisplayDateStart { get; set; }
    public Style CalendarItemStyle { get; set; }
    public Style CalendarDayButtonStyle { get; set; }
    public Style CalendarButtonStyle { get; set; }
    public CalendarBlackoutDatesCollection BlackoutDates { get; }
    public CalendarMode DisplayMode { get; set; }
    public DateTime? DisplayDateEnd { get; set; }
    public bool IsTodayHighlighted { get; set; }
    public DateTime? SelectedDate { get; set; }
    public SelectedDatesCollection SelectedDates { get; }
    public CalendarSelectionMode SelectionMode { get; set; }
    public DateTime DisplayDate { get; set; }
    public DayOfWeek FirstDayOfWeek { get; set; }
 
    public event EventHandler<SelectionChangedEventArgs> SelectedDatesChanged;
    public event EventHandler<CalendarDateChangedEventArgs> DisplayDateChanged;
    public event EventHandler<EventArgs> SelectionModeChanged;
    public event EventHandler<CalendarModeChangedEventArgs> DisplayModeChanged;
 
    public override void OnApplyTemplate();
    public override string ToString();
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnDisplayDateChanged(CalendarDateChangedEventArgs e);
    protected virtual void OnDisplayModeChanged(CalendarModeChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnKeyUp(KeyEventArgs e);
    protected virtual void OnSelectedDatesChanged(SelectionChangedEventArgs e);
    protected virtual void OnSelectionModeChanged(EventArgs e);
 
}
```

二、属性成员

|                        |                                                             |
| ---------------------- | ----------------------------------------------------------- |
| 属性名称                   | 说明                                                          |
| DisplayDateStart       | 获取或设置可在日历中的第一个日期。                                           |
| CalendarItemStyle      | 获取或设置CalendarItem的样式                                        |
| CalendarDayButtonStyle | 获取或设置CalendarDayButton的样式                                   |
| CalendarButtonStyle    | 获取或设置CalendarButton的样式                                      |
| BlackoutDates          | 获取标记为不可选择的日期的集合。                                            |
| DisplayMode            | 获取或设置一个值，该值指示是否日历显示月、 年或十年。                                 |
| DisplayDateEnd         | 获取或设置可在日历中的日期范围内的最后日期。                                      |
| IsTodayHighlighted     | 获取或设置一个值，该值指示是否突出显示当前日期。默认true。                             |
| SelectedDate           | 获取或设置当前选定的日期。[重要]                                           |
| SelectedDates          | 获取选定日期的集合。                                                  |
| SelectionMode          | 获取或设置一个值，指示允许包含什么样的选择。如果是多选的号，就可以从SelectedDates属性获取所有已选的日期。 |
| DisplayDate            | 获取或设置要显示的日期。                                                |
| FirstDayOfWeek         | 获取或设置在一天中被视为周的开始。                                           |

三、事件成员

|   |   |
|---|---|
|事件名称|说明|
|SelectedDatesChanged|开启多选后，当所选集合的元素数量发生变化时引发|
|DisplayDateChanged|DisplayDate属性被修改后引发|
|SelectionModeChanged|SelectionMode属性（选择模式）发生改变后引发|
|DisplayModeChanged|DisplayMode属性（显示模式）发生改变后引发|

四、Calendar示例

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel HorizontalAlignment="Center" Margin="30">
        <Calendar x:Name="_Calendar" 
                  HorizontalAlignment="Left" 
                  VerticalAlignment="Top"
                  Margin="0,15" 
                  DisplayDateStart="2020/1/1 00:00:00"
                  DisplayDateEnd="2030/1/1 00:00:00"
                  SelectionMode="MultipleRange"/>
        <Button Content="选择" Click="Button_Click"/>
    </StackPanel>
</Window>
```

后端代码

```cs
public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
 
        private void Button_Click(object sender, RoutedEventArgs e)
        {
            var list = _Calendar.SelectedDates;
            var current = _Calendar.SelectedDate;
            MessageBox.Show($"当前日期数量:{list.Count},当前日期：{current}");
        }
    }
```

我们设置了Calendar的DisplayDateStart和DisplayDateEnd属性，这样将日期的可选范围限制在2020年-2030年之间，同时开启了Calendar多项选择，最后在后端去获取所选的日期集合与当前日期。

当前日期是多选时的第一个日期。

![[assets/WPF从小白到大佬：内容控件：：Calendar日历控件/fa3119053f84f7a5195c2667677ab5c8_MD5.jpg]]

![[assets/WPF从小白到大佬：内容控件：：Calendar日历控件/31990aad98b8c3de9e7bf05f69732c37_MD5.jpg]]

在WPF中还有一个选择日期控件——DatePicker，它对当前这个Calendar控件进行了封装，通常DatePicker会更常用一些。下一节，我们来看看DatePicker控件的用法。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：034-《Calendar日历控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月31日