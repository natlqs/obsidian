MediaElement，一个可以播放音频或视频的控件，继承于FrameworkElement基类。MediaElement包含了常见的音频或视频格式，如果您需要更强大的功能，可以考虑使用VLC库。

> 官方说明
> 
> MediaElement 可以在两种不同的模式下使用，具体取决于驱动控件的内容：独立模式或时钟模式。 在独立模式下使用 时， MediaElement 类似于图像， Source 可以直接指定 URI。 在时钟模式下， MediaElement 可以将 视为动画的目标，因此它将在计时树中具有相应的 Timeline 和 Clock 条目。

一、MediaElement的定义

```cs
	public class MediaElement : FrameworkElement, IUriContext
{
    public static readonly DependencyProperty SourceProperty;
    public static readonly RoutedEvent ScriptCommandEvent;
    public static readonly RoutedEvent BufferingEndedEvent;
    public static readonly RoutedEvent BufferingStartedEvent;
    public static readonly RoutedEvent MediaOpenedEvent;
    public static readonly RoutedEvent MediaFailedEvent;
    public static readonly DependencyProperty StretchDirectionProperty;
    public static readonly RoutedEvent MediaEndedEvent;
    public static readonly DependencyProperty LoadedBehaviorProperty;
    public static readonly DependencyProperty UnloadedBehaviorProperty;
    public static readonly DependencyProperty ScrubbingEnabledProperty;
    public static readonly DependencyProperty IsMutedProperty;
    public static readonly DependencyProperty BalanceProperty;
    public static readonly DependencyProperty VolumeProperty;
    public static readonly DependencyProperty StretchProperty;
 
    public MediaElement();
 
    public MediaState LoadedBehavior { get; set; }
    public bool CanPause { get; }
    public bool IsBuffering { get; }
    public double DownloadProgress { get; }
    public double BufferingProgress { get; }
    public int NaturalVideoHeight { get; }
    public Duration NaturalDuration { get; }
    public bool HasAudio { get; }
    public bool HasVideo { get; }
    public TimeSpan Position { get; set; }
    public double SpeedRatio { get; set; }
    public MediaState UnloadedBehavior { get; set; }
    public int NaturalVideoWidth { get; }
    public bool ScrubbingEnabled { get; set; }
    public MediaClock Clock { get; set; }
    public double Balance { get; set; }
    public double Volume { get; set; }
    public StretchDirection StretchDirection { get; set; }
    public Stretch Stretch { get; set; }
    public Uri Source { get; set; }
    public bool IsMuted { get; set; }
 
    public event RoutedEventHandler BufferingEnded;
    public event RoutedEventHandler BufferingStarted;
    public event RoutedEventHandler MediaOpened;
    public event EventHandler<ExceptionRoutedEventArgs> MediaFailed;
    public event RoutedEventHandler MediaEnded;
    public event EventHandler<MediaScriptCommandRoutedEventArgs> ScriptCommand;
 
    public void Close();
    public void Pause();
    public void Play();
    public void Stop();
    protected override Size ArrangeOverride(Size finalSize);
    protected override Size MeasureOverride(Size availableSize);
    protected override AutomationPeer OnCreateAutomationPeer();
    protected override void OnRender(DrawingContext drawingContext);
 
}
```

二、属性成员

|   |   |
|---|---|
|属性名称|说明|
|LoadedBehavior|获取或设置加载媒体的行为，如果加载希望手动控制播放，请设置为Manual。|
|CanPause|获取一个值，该值指示是否可以暂停媒体。|
|IsBuffering|获取一个值，该值指示是否缓冲媒体。|
|DownloadProgress|获取一个百分比值，该值为位于远程服务器上的内容完成的下载量。|
|BufferingProgress|获取一个值，该值指示缓冲进度的百分比。0-1之间|
|NaturalVideoHeight|获取与媒体关联的视频的高度。|
|NaturalDuration|获取介质的自然持续时间。也就是视频播放总时长。|
|HasAudio|获取一个值，该值指示媒体是否具有音频。|
|HasVideo|获取一个值，该值指示媒体是否具有视频。|
|Position|通过媒体的播放时间获取或设置进度的当前位置。|
|SpeedRatio|获取或设置媒体的速率。也就是按几倍播放视频。|
|UnloadedBehavior|获取或设置卸载媒体的行为。|
|NaturalVideoWidth|获取与媒体关联的视频的宽度。|
|ScrubbingEnabled|获取或设置一个值，该值指示MediaElement 是否将更新帧的查找操作在暂停状态。|
|Clock|获取或设置MediaElement 媒体播放相关联的时钟。|
|Balance|获取或设置扬声器的音量比。|
|Volume|获取或设置媒体的音量。0-1之间，默认0.5|
|StretchDirection|获取或设置一个值，确定扩展的限制应用于映像。|
|Stretch|获取或设置MediaElement媒体的拉伸方式。|
|Source|获取或设置MediaElement媒体源[重点]|
|IsMuted|是否静音|

三、事件成员

|   |   |
|---|---|
|事件名称|说明|
|BufferingEnded|媒体缓冲结束时发生。|
|BufferingStarted|媒体缓冲开始时发生。|
|MediaOpened|媒体加载已完成时发生。|
|MediaFailed|遇到错误时发生。|
|MediaEnded|媒体结束时发生。|
|ScriptCommand|在媒体中遇到的脚本命令时发生。|

四、MediaElement示例

我们以MediaElement的独立模式为例，开发一个基础版本的视频播放器，该项目将会用到MediaElement、Gird、Border、TextBlock、Button、Slider、ProgressBar等控件，也算是对之前学过的控件章节一次总结和回顾。

前端代码

```cs
<Window x:Class="HelloWorld.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:HelloWorld"
        mc:Ignorable="d" FontSize="14"
        Title="WPF中文网之控件课程 - www.wpfsoft.com" Height="400" Width="550">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="auto"/>
            <RowDefinition Height="auto"/>
        </Grid.RowDefinitions>
        <Grid>
            <MediaElement x:Name="_MediaElement" LoadedBehavior="Manual" />
            <Border x:Name="_Border" Background="Black">
                <TextBlock x:Name="_TextBlock" Text="MediaElement | 媒体播放器" Foreground="LightCoral" FontSize="20"
                           HorizontalAlignment="Center" VerticalAlignment="Center"/>
            </Border>
        </Grid>
        
        <StackPanel Orientation="Horizontal" Grid.Row="1">
            <Button Content="打开" Width="60" Height="25" Margin="5" Click="OpenMedia"/>
            <Button Content="播放" Width="60" Height="25" Margin="5" Click="PlayMedia"/>
            <Button Content="停止" Width="60" Height="25" Margin="5" Click="StopMedia"/>
            <Button Content="后退" Width="60" Height="25" Margin="5" Click="BackMedia"/>
            <Button Content="前进" Width="60" Height="25" Margin="5" Click="ForwardMedia"/>
            <TextBlock Text="音量" VerticalAlignment="Center" Margin="5"/>
            <Slider x:Name="_Slider" Width="120" VerticalAlignment="Center" Maximum="100" Value="50" ValueChanged="_Slider_ValueChanged"/>
        </StackPanel>
 
        <Grid Grid.Row="2">
            <ProgressBar x:Name="_ProgressBar" Height="10" Margin="5"/>
        </Grid>
    </Grid>
</Window>
```

后端代码

```cs
using System;
using System.Windows;
using System.Windows.Threading;
 
namespace HelloWorld
{
    /// <summary>
    /// MainWindow.xaml 的交互逻辑
    /// </summary>
    public partial class MainWindow : Window
    {
        private string file = string.Empty;
        public MainWindow()
        {
            InitializeComponent();
 
            DispatcherTimer timer = new DispatcherTimer();
            timer.Interval = TimeSpan.FromMilliseconds(1000);
            timer.Tick += (s, e) =>
            {
                var ts = _MediaElement.Position;
                _ProgressBar.Value = ts.TotalMilliseconds;//更新当前播放进度
 
            };
            timer.Start();
        }
 
        private void OpenMedia(object sender, RoutedEventArgs e)
        {
            var openFileDialog = new Microsoft.Win32.OpenFileDialog()
            {
                Filter = "视频文件 (.mp4)|*.mp4",
                Multiselect = true
            };
            var result = openFileDialog.ShowDialog();
            if (result == true)
            {
                file = openFileDialog.FileName;
                _MediaElement.MediaOpened += _MediaElement_MediaOpened;
                _MediaElement.Source = new System.Uri(file);
                this.Title = file;
                _TextBlock.Text = file;
            }
        }
 
        private void _MediaElement_MediaOpened(object sender, RoutedEventArgs e)
        {
            if (_MediaElement.NaturalDuration.HasTimeSpan)
            {
                var ts = _MediaElement.NaturalDuration.TimeSpan;
                _ProgressBar.Maximum = ts.TotalMilliseconds;//设置播放进度条总长
            }
        }
 
        private void PlayMedia(object sender, RoutedEventArgs e)
        {
            _MediaElement.Play();//播放
            _Border.Visibility = Visibility.Collapsed;            
           
        }
 
        private void StopMedia(object sender, RoutedEventArgs e)
        {
            _MediaElement.Pause();//暂停
        }
 
        private void BackMedia(object sender, RoutedEventArgs e)
        {
            _MediaElement.Position = _MediaElement.Position - TimeSpan.FromSeconds(10);//快退10秒
        }
 
        private void ForwardMedia(object sender, RoutedEventArgs e)
        {
            _MediaElement.Position = _MediaElement.Position + TimeSpan.FromSeconds(10);//快进10秒
 
        }
 
        private void _Slider_ValueChanged(object sender, RoutedPropertyChangedEventArgs<double> e)
        {
            _MediaElement.Volume = _Slider.Value;//设置音量
        }
    }
}
```

![[assets/WPF从小白到大佬：内容控件：：MediaElement媒体播放器/d7eefb4afd9070beb2121d6ed7225e03_MD5.jpg]]

如图所示，我们将前面所学的控件知识综合利用起来，设计出一款播放界面。MediaElement控件没有Background属性，所以我们放了一个Border控件（黑色背景）在MediaElement控件的上方，待加载视频并播放时，将Border隐藏起来。

在后端C#语言中，我们在构造函数里通过DispatcherTimer 开启了一个子线程，用以更新当前播放进度。DispatcherTimer是运行在UI线程上的定时器,可以直接更新UI元素，不会引发跨线程调用的异常。

在打开视频业务中，我们采用Microsoft.Win32.OpenFileDialog去获取视频文件地址，然后创建一个Uri实例，最后把这个实例赋值给MediaElement的Source属性。

![[assets/WPF从小白到大佬：内容控件：：MediaElement媒体播放器/89ae1bbe8b54dafe1763a28bad90df10_MD5.jpg]]

最后，我们只需要调用MediaElement提供的一系列方法成员，比如Play()、Pause()、或设置播放位置的Position属性。好，关于MediaElement控件就先介绍到这里啦。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：037-《MediaElement媒体播放器》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年8月31日