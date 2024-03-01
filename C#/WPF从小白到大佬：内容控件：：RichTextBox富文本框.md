RichTextBox继承于TextBoxBase基类，所以很大程度上与TextBox控件类似，两者在某些情况下可以互相替换。但是，如果要为用户提供更强大的文档编辑功能，非RichTextBox莫属。在学习这个控件之前，请参阅FlowDocument（流文档）一节。

首先，我们来看看RichTextBox的结构定义：

```cs
public class RichTextBox : TextBoxBase, IAddChild
{
    public static readonly DependencyProperty IsDocumentEnabledProperty;
 
    public RichTextBox();
    public RichTextBox(FlowDocument document);
 
    public FlowDocument Document { get; set; }
    public bool IsDocumentEnabled { get; set; }
    public TextSelection Selection { get; }
    public TextPointer CaretPosition { get; set; }
    protected internal override IEnumerator LogicalChildren { get; }
 
    public TextPointer GetNextSpellingErrorPosition(TextPointer position, LogicalDirection direction);
    public TextPointer GetPositionFromPoint(Point point, bool snapToText);
    public SpellingError GetSpellingError(TextPointer position);
    public TextRange GetSpellingErrorRange(TextPointer position);
    public bool ShouldSerializeDocument();
    protected override Size MeasureOverride(Size constraint);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnDpiChanged(DpiScale oldDpiScaleInfo, DpiScale newDpiScaleInfo);
 
}
```

RichTextBox控件有一个带参数的构造函数，参数的类型是FlowDocument类，另外，它还有一个Document属性，类型也是FlowDocument类，说明RichTextBox控件的元素必须且只能是FlowDocument类，如果您试图将RichTextBox.Document=null，会发现它会报错。

我们假定您对FlowDocument类有一定的了解，所以，我们直接看一下示例。

前端代码

```cs
	<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="16"
        Title="HelloWorld - www.wpfsoft.com" Height="360" Width="500">
    <StackPanel>
        <RichTextBox x:Name="_richTextBox" Margin="10 5" Height="270">
            <FlowDocument>
                <Paragraph>RichTextBox富文本框控件到底有什么强大的功能?
                    <Bold Foreground="DarkRed">请看下面.</Bold>
                </Paragraph>
                <Paragraph Foreground="Blue">RichTextBox唯一的子元素是FlowDocument</Paragraph>
                <Paragraph Foreground="DarkGreen">
                    FlowDocument是指流文档,一个流文档由一个或多个Block构成，
                    所以它有一个Blocks属性。Block只是一个抽象基类，
                    所以流文档的子元素其实是继承了Block的子类，例如：
                </Paragraph>
                <List MarkerOffset="25" MarkerStyle="Decimal"  StartIndex="1">
                    <ListItem>
                        <Paragraph>BlockUIContainer（UI元素容器）</Paragraph>
                    </ListItem>
                    <ListItem>
                        <Paragraph>List（有序列表）</Paragraph>
                    </ListItem>
                    <ListItem>
                        <Paragraph>Paragraph（段落）</Paragraph>
                    </ListItem>
                    <ListItem>
                        <Paragraph>Section（分组）</Paragraph>
                    </ListItem>
                    <ListItem>
                        <Paragraph>Table（网格）</Paragraph>
                    </ListItem>
                </List>
            </FlowDocument>
        </RichTextBox>
        <Button x:Name="_button" Content="确定" Margin="10,5" Click="_button_Click"/>
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
            TextRange textRange = new TextRange(_richTextBox.Document.ContentStart, _richTextBox.Document.ContentEnd);
            MessageBox.Show(textRange.Text);
 
            Paragraph paragraph = new Paragraph();
            Run run = new Run($"当前时间：{DateTime.Now}"); //手动加换行
            run.Foreground = Brushes.Black;
            paragraph.Inlines.Add(run);
            _richTextBox.Document.Blocks.Add(paragraph);
        }
    }
}
```

![[assets/WPF从小白到大佬：内容控件：：RichTextBox富文本框/a1b5ac667185f10ff25896296005081f_MD5.jpg]]

如上所示，我们在窗体中实例化了一个RichTextBox控件，并实例化了一个FlowDocument对象。RichTextBox唯一的子元素是FlowDocument，

FlowDocument是指流文档,一个流文档由一个或多个Block构成，所以它有一个Blocks属性。Block只是一个抽象基类，FlowDocument流文档的子元素都继承了Block抽象基类，例如：

- BlockUIContainer（UI元素容器）
- List（有序列表）
- Paragraph（段落）
- Section（分组）
- Table（网格）

BlockUIContainer是一个非常强大的段落元素，因为它可以直接包含WPF的控件。这样一来，我们就可以将设计的UI写入到流文档中显示或打印。

上面这五个元素继承了TextElement、FrameworkContentElement和ContentElement三个父素，所以实际上这五个子元素就拥有了许多字体属性的设置、资源、样式、数据绑定、以及各种事件的应用。

如果要获取RichTextBox的文本信息，可以使用TextRange类。FlowDocument类有两个属性，分别ContentStart和ContentEnd，表示文字内容的开始和结束。

![[assets/WPF从小白到大佬：内容控件：：RichTextBox富文本框/cf3590f15184a22c51d2bf859b0b5a69_MD5.jpg]]

所以通过TextRange类的Text，我们就能访问到RichTextBox控件的内容。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：024-《RichTextBox文本框》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月28日