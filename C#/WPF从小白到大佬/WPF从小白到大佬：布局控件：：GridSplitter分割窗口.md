GridSplitter控件用来分割窗体的布局，必须放在Grid栅格控件中配合使用，通过鼠标按住GridSplitter进行左右或上下拖动，即可调整行列尺寸。

注意事项：

1、如果您希望GridSplitter控件可以水平调整左右的Grid列宽时，那么HorizontalAlignment属性必须设置为Stretch或者Center。

2、如果您希望GridSplitter控件可以垂直调整行高，那么VerticalAlignment属性必须设置为Stretch或者Center。

3、ShowsPreview属性表示拖动时是否及时绘制调整尺寸。

接下来，我们通过一个例子来说明它的用法

前端代码

```html
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="350" Width="500">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition/>
            <ColumnDefinition Width="auto"/>
            <ColumnDefinition/>
        </Grid.ColumnDefinitions>
        <Border Grid.Column="0" Background="LightBlue">
            <TextBlock TextWrapping="Wrap" Padding="10" LineHeight="20">
                您好，我是站长重庆教主，欢迎来到 WPF中文网，我们的网址是：http://www.wpfsoft.com。您还可以在百度、B站、51CTO、csdn搜索我的名字，以便找到我其它的技术文章或视频课程。本站上线于2023年8月3日，在一个稀松平常的午后，我突然想搭建一个关于学习和分享WPF框架的博客网站，于是开始注册域名、购买空间、安装网站、设置栏目，不到3个小时，WPF中文网就诞生了。
            </TextBlock>
        </Border>
        <GridSplitter Grid.Column="1" Width="5" HorizontalAlignment="Center"   ShowsPreview="False"/>
        <Border Grid.Column="2" Background="LightCoral">
            <TextBlock TextWrapping="Wrap" Padding="10" LineHeight="20">
接下来的日子里，我将从WPF的起源、概述、学习路径等，一路写下去，一直把WPF最后一滴知识详尽才会封笔，我明白这是一场耗费个人巨大精力的战争。但是，那些我曾踩过的坑与走过的弯路，都无时无刻不提醒着我，尽量像讲故事一样，把这一切都写下来吧，总结自己，照亮来者。
            </TextBlock>
        </Border>
 
    </Grid>
</Window>
```

![[assets/WPF从小白到大佬：布局控件：：GridSplitter分割窗口/1cbdb275022d7361a7328778b589ed81_MD5.jpg]]

接下来我们看看F5运行后，可以用鼠标左右拖动的窗体效果。最好是为GridSplitter单独分配一行或者一列，同时，GridSplitter需要跨越整行或整列，这样的效果会更好。如上面的代码所示，我们在Grid中分割了3个单元格（3列），将GridSplitter居在放置，简单设置一下GridSplitter的属性，就可以达到我们的目的了。

![[assets/WPF从小白到大佬：布局控件：：GridSplitter分割窗口/176c13839609a570e502d6977049dc01_MD5.jpg]]

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：033-《GridSplitter分割控件》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月30日