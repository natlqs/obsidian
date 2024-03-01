一、概述

几乎所有的文本、数字、符号的输入都是用TextBox文本框来完成的。TextBox用来获取用户的键盘输入的信息，这也是一个常用的控件。它继承于TextBoxBase，而TextBoxBase又继承于Control，我们在前面已经介绍过Control基类，我们先看看它及TextBoxBase基类的结构定义：

二、TextBoxBase基类

```cs
public abstract class TextBoxBase : Control
{
    public static readonly DependencyProperty IsReadOnlyProperty;
    public static readonly RoutedEvent SelectionChangedEvent;
    public static readonly RoutedEvent TextChangedEvent;
    public static readonly DependencyProperty IsSelectionActiveProperty;
    public static readonly DependencyProperty CaretBrushProperty;
    public static readonly DependencyProperty SelectionOpacityProperty;
    public static readonly DependencyProperty SelectionBrushProperty;
    public static readonly DependencyProperty AutoWordSelectionProperty;
    public static readonly DependencyProperty IsInactiveSelectionHighlightEnabledProperty;
    public static readonly DependencyProperty IsUndoEnabledProperty;
    public static readonly DependencyProperty VerticalScrollBarVisibilityProperty;
    public static readonly DependencyProperty HorizontalScrollBarVisibilityProperty;
    public static readonly DependencyProperty AcceptsTabProperty;
    public static readonly DependencyProperty AcceptsReturnProperty;
    public static readonly DependencyProperty IsReadOnlyCaretVisibleProperty;
    public static readonly DependencyProperty UndoLimitProperty;
 
    public double ViewportWidth { get; }
    public double ExtentHeight { get; }
    public double ExtentWidth { get; }
    public ScrollBarVisibility VerticalScrollBarVisibility { get; set; }
    public bool AcceptsReturn { get; set; }
    public SpellCheck SpellCheck { get; }
    public bool AcceptsTab { get; set; }
    public bool IsReadOnlyCaretVisible { get; set; }
    public double ViewportHeight { get; }
    public ScrollBarVisibility HorizontalScrollBarVisibility { get; set; }
    public double HorizontalOffset { get; }
    public double SelectionOpacity { get; set; }
    public bool CanUndo { get; }
    public bool CanRedo { get; }
    public bool IsUndoEnabled { get; set; }
    public int UndoLimit { get; set; }
    public bool AutoWordSelection { get; set; }
    public Brush SelectionBrush { get; set; }
    public bool IsReadOnly { get; set; }
    public Brush CaretBrush { get; set; }
    public bool IsSelectionActive { get; }
    public bool IsInactiveSelectionHighlightEnabled { get; set; }
    public double VerticalOffset { get; }
 
    public event TextChangedEventHandler TextChanged;
    public event RoutedEventHandler SelectionChanged;
 
    public void AppendText(string textData);
    public void BeginChange();
    public void Copy();
    public void Cut();
    public IDisposable DeclareChangeBlock();
    public void EndChange();
    public void LineDown();
    public void LineLeft();
    public void LineRight();
    public void LineUp();
    public void LockCurrentUndoUnit();
    public override void OnApplyTemplate();
    public void PageDown();
    public void PageLeft();
    public void PageRight();
    public void PageUp();
    public void Paste();
    public bool Redo();
    public void ScrollToEnd();
    public void ScrollToHome();
    public void ScrollToHorizontalOffset(double offset);
    public void ScrollToVerticalOffset(double offset);
    public void SelectAll();
    public bool Undo();
    protected override void OnContextMenuOpening(ContextMenuEventArgs e);
    protected override void OnDragEnter(DragEventArgs e);
    protected override void OnDragLeave(DragEventArgs e);
    protected override void OnDragOver(DragEventArgs e);
    protected override void OnDrop(DragEventArgs e);
    protected override void OnGiveFeedback(GiveFeedbackEventArgs e);
    protected override void OnGotKeyboardFocus(KeyboardFocusChangedEventArgs e);
    protected override void OnKeyDown(KeyEventArgs e);
    protected override void OnKeyUp(KeyEventArgs e);
    protected override void OnLostFocus(RoutedEventArgs e);
    protected override void OnLostKeyboardFocus(KeyboardFocusChangedEventArgs e);
    protected override void OnMouseDown(MouseButtonEventArgs e);
    protected override void OnMouseMove(MouseEventArgs e);
    protected override void OnMouseUp(MouseButtonEventArgs e);
    protected override void OnMouseWheel(MouseWheelEventArgs e);
    protected override void OnPreviewKeyDown(KeyEventArgs e);
    protected override void OnQueryContinueDrag(QueryContinueDragEventArgs e);
    protected override void OnQueryCursor(QueryCursorEventArgs e);
    protected virtual void OnSelectionChanged(RoutedEventArgs e);
    protected override void OnTemplateChanged(ControlTemplate oldTemplate, ControlTemplate newTemplate);
    protected virtual void OnTextChanged(TextChangedEventArgs e);
    protected override void OnTextInput(TextCompositionEventArgs e);
 
}
```

我们看一看TextBoxBase基类都提供了哪些成员

属性成员

|   |   |
|---|---|
|属性名称|说明|
|VerticalScrollBarVisibility|垂直滚动条是否显示|
|HorizontalScrollBarVisibility|水平滚动条是否显示|
|AcceptsReturn|表示用户按下回车键时是否插入新行。|
|AcceptsTab|用来设置用户按下tab键的响应，为true表示插入一个制表符，否则将焦点移动到标记为制表位的下一个控件且不插入制表符。|
|IsReadOnlyCaretVisible|表示只读文本框是否显示插入符号，用得较少。|
|SelectionOpacity|用来设置用户选中的文本的透明度。|
|IsUndoEnabled|表示文本编辑控件是否启用撤消支持。|
|UndoLimit|获取或设置存储在撤消队列中的操作数目。|
|AutoWordSelection|表示自动选择字词，默认为false。|
|SelectionBrush|表示用户选择的文本段落的画笔，比较常用。|
|IsReadOnly|表示文本框是否只读，这个属性经常使用。|
|CaretBrush|表示获取或设置用于绘制的文本框中插入符号的画笔。|
|IsInactiveSelectionHighlightEnabled|表示获取或设置一个值，该值指示当文本框没有焦点时，文本框中是否显示选定的文本。|

事件成员

TextBoxBase基类提供了两个事件，分别是TextChanged和SelectionChanged。

TextChanged事件：只要文本框中的内容被修改，将会触发引事件，这通常用来做一些判断业务。比如某个文本框只能输入数字，那就可以去订阅TextChanged事件。

SelectionChanged事件：选中的文本框内容发生改变时引发的事件。

三、TextBox控件

在了解TextBoxBase基类之后，我们来看看这个控件本身提供了哪些属性、方法和事件。

```cs
public class TextBox : TextBoxBase, IAddChild, ITextBoxViewHost
{
    public static readonly DependencyProperty TextWrappingProperty;
    public static readonly DependencyProperty MinLinesProperty;
    public static readonly DependencyProperty MaxLinesProperty;
    public static readonly DependencyProperty TextProperty;
    public static readonly DependencyProperty CharacterCasingProperty;
    public static readonly DependencyProperty MaxLengthProperty;
    public static readonly DependencyProperty TextAlignmentProperty;
    public static readonly DependencyProperty TextDecorationsProperty;
 
    public TextBox();
 
    public int MinLines { get; set; }
    public int MaxLines { get; set; }
    public string Text { get; set; }
    public CharacterCasing CharacterCasing { get; set; }
    public int MaxLength { get; set; }
    public TextAlignment TextAlignment { get; set; }
    public int CaretIndex { get; set; }
    public int SelectionLength { get; set; }
    public int SelectionStart { get; set; }
    public Typography Typography { get; }
    public int LineCount { get; }
    public TextDecorationCollection TextDecorations { get; set; }
    public string SelectedText { get; set; }
    public TextWrapping TextWrapping { get; set; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public void Clear();
    public int GetCharacterIndexFromLineIndex(int lineIndex);
    public int GetCharacterIndexFromPoint(Point point, bool snapToText);
    public int GetFirstVisibleLineIndex();
    public int GetLastVisibleLineIndex();
    public int GetLineIndexFromCharacterIndex(int charIndex);
    public int GetLineLength(int lineIndex);
    public string GetLineText(int lineIndex);
    public int GetNextSpellingErrorCharacterIndex(int charIndex, LogicalDirection direction);
    public Rect GetRectFromCharacterIndex(int charIndex, bool trailingEdge);
    public Rect GetRectFromCharacterIndex(int charIndex);
    public SpellingError GetSpellingError(int charIndex);
    public int GetSpellingErrorLength(int charIndex);
    public int GetSpellingErrorStart(int charIndex);
    public void ScrollToLine(int lineIndex);
    public void Select(int start, int length);
    public bool ShouldSerializeText(XamlDesignerSerializationManager manager);
    protected override Size MeasureOverride(Size constraint);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnPropertyChanged(DependencyPropertyChangedEventArgs e);
 
}
```

属性成员

|   |   |
|---|---|
|属性名称|说明|
|MinLines|获取或设置最小可见的行数。|
|MaxLines|获取或设置可见行的最大数目。|
|Text|获取或设置文本框的文本内容。|
|CharacterCasing|获取或设置文本框字符的大小写形式，默认不转换。 它是一个枚举，Normal表示不转换大小写，Lower表示全部转换成小写，Upper表示全部转换成大写|
|MaxLength|获取或设置最大可以在文本框中手动输入的字符数。|
|TextAlignment|获取或设置文本框的内容的水平对齐方式。例如左对齐，右对齐，居在对齐和两端对齐。|
|CaretIndex|获取或设置插入点移动的插入位置索引。|
|SelectionLength|获取或设置一个值，该值在文本框中当前所选内容中的字符数。|
|SelectionStart|获取或设置当前所选内容的起始位置的字符索引。|
|Typography|获取文本框中的文本内容的当前有效的版式变体。|
|LineCount|获取文本框中的总行数。|
|TextDecorations|获取要应用于文本框中的文本修饰。|
|SelectedText|获取或设置文本框中当前选择的内容。|
|TextWrapping|获取或设置文本框中文本的换行方式。这个属性比较常用，在较长的文字段落显示时可以设置为Wrap，这样自动换行，界面呈现的效果比较令人满意。|

TextBox文本框本身没有任务事件，都是继承父类的事件。我们先看一下它的简单使用示例。
  
前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="HelloWorld - www.wpfsoft.com" Height="350" Width="500">
    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <TextBlock Text="用户名" Margin="5"/>
        <TextBox x:Name="_textbox" Width="100" Height="25" MaxLength="10" CharacterCasing="Upper"/>
        <Button x:Name="_button" Content="确定" Height="25" Margin="5 0" Click="_button_Click"/>
    </StackPanel>
</Window>
```

后端代码

```cs
namespace HelloWorld
{
        /// <summary>
        /// MainWindow.xaml 的交互逻辑
        /// </summary>
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }
 
        private void _button_Click(object sender, RoutedEventArgs e)
        {
            MessageBox.Show($"您的用户名：{_textbox.Text}");
        }
    }
}
```

![[assets/WPF从小白到大佬：内容控件：：TextBox文本框/a2e2d0a303873da6e0979f3409862531_MD5.jpg]]

我们使用了CharacterCasing="Upper"这个设置，可以看到图片中的显示效果，虽然我在输入时是小写的china字符，但是，TextBox会转换成大写的CHINA，另外，总长度不能超过10个字符。

最后要获取TextBox文本框的内容，使用Text属性即可。当我们在学习了样式之后，我们还会回过头来，对TextBox控件进行深入学习。另外，TextBox还有一个大哥，也是继承于TextBoxBase基类，它叫RichTextBox 类。这个控件的功能更加强大，能够对FlowDocument流文档进行操作。如果您想开发类似Word的桌面软件，RichTextBox 和FlowDocument搭配组合是非常好的选择。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：023-《TextBox文本框》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月28日