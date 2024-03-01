DatePicker与Calender在某些属性上很相似，只是为了方便显示和操作，DatePicker将Calender进行了封装。

一、DatePicker定义

```cs
	public class DatePicker : Control
{
    public static readonly RoutedEvent SelectedDateChangedEvent;
    public static readonly DependencyProperty TextProperty;
    public static readonly DependencyProperty SelectedDateFormatProperty;
    public static readonly DependencyProperty IsTodayHighlightedProperty;
    public static readonly DependencyProperty IsDropDownOpenProperty;
    public static readonly DependencyProperty SelectedDateProperty;
    public static readonly DependencyProperty DisplayDateStartProperty;
    public static readonly DependencyProperty DisplayDateEndProperty;
    public static readonly DependencyProperty DisplayDateProperty;
    public static readonly DependencyProperty CalendarStyleProperty;
    public static readonly DependencyProperty FirstDayOfWeekProperty;
 
    public DatePicker();
 
    public CalendarBlackoutDatesCollection BlackoutDates { get; }
    public DateTime? DisplayDateStart { get; set; }
    public DateTime? DisplayDateEnd { get; set; }
    public DateTime DisplayDate { get; set; }
    public Style CalendarStyle { get; set; }
    public bool IsTodayHighlighted { get; set; }
    public bool IsDropDownOpen { get; set; }
    public DatePickerFormat SelectedDateFormat { get; set; }
    public string Text { get; set; }
    public DayOfWeek FirstDayOfWeek { get; set; }
    public DateTime? SelectedDate { get; set; }
    protected internal override bool HasEffectiveKeyboardFocus { get; }
 
    public event RoutedEventHandler CalendarClosed;
    public event RoutedEventHandler CalendarOpened;
    public event EventHandler<SelectionChangedEventArgs> SelectedDateChanged;
    public event EventHandler<DatePickerDateValidationErrorEventArgs> DateValidationError;
 
    public override void OnApplyTemplate();
    public override string ToString();
    protected virtual void OnCalendarClosed(RoutedEventArgs e);
    protected virtual void OnCalendarOpened(RoutedEventArgs e);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected virtual void OnDateValidationError(DatePickerDateValidationErrorEventArgs e);
    protected virtual void OnSelectedDateChanged(SelectionChangedEventArgs e);
 
}
```

二、属性成员

|   |   |
|---|---|
|属性成员|说明|
|BlackoutDates|获取或设置为不可选择的日期的标记集合。不常用。|
|DisplayDateStart|获取或设置要显示的第一个日期。|
|DisplayDateEnd|获取或设置要显示的最后日期。|
|DisplayDate|获取或设置要显示的日期。|
|CalendarStyle|获取或设置呈现日历时所使用的样式。|
|IsTodayHighlighted|获取或设置一个值，该值指示是否将突出显示当前日期。|
|IsDropDownOpen|获取或设置一个值，该值指示Calendar 下拉列表是打开还是关闭。|
|SelectedDateFormat|获取或设置用于显示所选的日期的格式。|
|Text|获取DatePicker显示文本，或设置选定的日期|
|FirstDayOfWeek|获取或设置在一天中被视为周的开始。|
|SelectedDate|获取或设置当前选定的日期。|
|HasEffectiveKeyboardFocus|获取一个值，该值指示DatePicker 是否 具有焦点。|

三、事件成员

|   |   |
|---|---|
|事件名称|说明|
|CalendarClosed|DatePicker下拉列表关闭时引发此事件|
|CalendarOpened|DatePicker下拉列表打开时引发此事件|
|SelectedDateChanged|SelectedDate属性发生改变时引发此事件|
|DateValidationError|当Text属性不是日期形式的字符串时引发此事件|

接下来，我们以一个例子来说明它的用法

四、DatePicker示例

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
    <StackPanel HorizontalAlignment="Center" Margin="30" VerticalAlignment="Center">
        <StackPanel Orientation="Horizontal" Margin="10">
            <TextBlock Text="开始日期" VerticalAlignment="Center" Margin="10,0"/>
            <DatePicker x:Name="_DatePickerStart" VerticalAlignment="Center" Width="120"/>
        </StackPanel>
        <StackPanel Orientation="Horizontal" Margin="10">
            <TextBlock Text="结束日期" VerticalAlignment="Center" Margin="10,0"/>
            <DatePicker x:Name="_DatePickerEnd" VerticalAlignment="Center" Width="120"/>
        </StackPanel>
        <Button Content="查询" Click="Button_Click" Margin="10,0"/>
       
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
            var selectedDate = $"查询日期:{_DatePickerStart.SelectedDate} {_DatePickerEnd.SelectedDate}\r\n";
            var text = $"文本值:{_DatePickerStart.Text} {_DatePickerEnd.Text}";
            MessageBox.Show($"{selectedDate} {text}");
        }
    }
```

![[assets/WPF从小白到大佬：内容控件：：DatePicker日期控件/e6a1610c36e3aa8f01f4df69a7c48ad3_MD5.jpg]]

需要注意一点的是，DatePicker的SelectedDate属性是一个DateTime?日期型，而Text属性是一个string类型，所以两者的内容是不相等的。看一下运行结果：

![[assets/WPF从小白到大佬：内容控件：：DatePicker日期控件/6800a1c51cf14dd7a00a5c6d1a97f107_MD5.jpg]]

SelectedDate属性后面会带上时分秒信息。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：035-《DatePicker日期控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月31日