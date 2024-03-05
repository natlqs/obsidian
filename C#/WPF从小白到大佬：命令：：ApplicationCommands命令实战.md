这一节我们来演示一下ApplicationCommands预定义命令的使用，通过一个建议的记事本开发流程，掌握ApplicationCommands预定义命令的用法。

使用预定义命令，最重要的三个地方，第一点，如何通过CommandBinding对象去关联一个Command，第二点，如何编写命令的业务代码，第三点，命令如何触发，或者说，命令如何绑定到命令源对象。

第一点，通过CommandBinding对象去关联一个Command

```xml
<Window.CommandBindings>
    <CommandBinding Command="ApplicationCommands.Open"  
                    CanExecute="OpenCommandCanExecute" 
                    Executed="OpenCommandExecuted"  />
    
    <CommandBinding Command="ApplicationCommands.Cut" 
                    CanExecute="CutCommandCanExecute" 
                    Executed="CutCommandExecuted" />
    
    <CommandBinding Command="ApplicationCommands.Paste" 
                    CanExecute="PasteCommandCanExecute" 
                    Executed="PasteCommandExecuted" />
    
    <CommandBinding Command="ApplicationCommands.Save"  
                    CanExecute="SaveCommandCanExecute" 
                    Executed="SaveCommandExecuted" />
</Window.CommandBindings>```

在Window窗体的CommandBindings集合中，我们实例化了4个CommandBinding对象，分别关联了Open、Cut、Paste和Save，对应打开、剪切、粘贴和保存。

第二点，如何编写命令的业务代码

```cs
private void OpenCommandCanExecute(object sender, CanExecuteRoutedEventArgs e)
{
    e.CanExecute = true;
}
 
private void OpenCommandExecuted(object sender, ExecutedRoutedEventArgs e)
{
    var openFileDialog = new Microsoft.Win32.OpenFileDialog()
    {
        Filter = "文本文档 (.txt)|*.txt",
        Multiselect = true
    };
    var result = openFileDialog.ShowDialog();
    if (result.Value)
    {
        textbox.Text = File.ReadAllText(openFileDialog.FileName);
    }
}
 
private void CutCommandCanExecute(object sender, CanExecuteRoutedEventArgs e)
{
    e.CanExecute = textbox != null && textbox.SelectionLength > 0;
}
 
private void CutCommandExecuted(object sender, ExecutedRoutedEventArgs e)
{
    textbox.Cut();
}
 
private void PasteCommandCanExecute(object sender, CanExecuteRoutedEventArgs e)
{
    e.CanExecute = Clipboard.ContainsText();
}
 
private void PasteCommandExecuted(object sender, ExecutedRoutedEventArgs e)
{
    textbox.Paste();
}
 
private void SaveCommandCanExecute(object sender, CanExecuteRoutedEventArgs e)
{
    e.CanExecute = textbox != null && textbox.Text.Length > 0;
}
 
private void SaveCommandExecuted(object sender, ExecutedRoutedEventArgs e)
{
    var saveFileDialog = new Microsoft.Win32.SaveFileDialog
    {
        Filter = "文本文档 (.txt)|*.txt"
    };
 
    if (saveFileDialog.ShowDialog() == true)
    {
        File.WriteAllText(saveFileDialog.FileName , textbox.Text);
    }
}
```

我们需要给Command的CanExecute和Executed成员分别创建各自的回调函数，并在函数中编写自己的业务逻辑。

第三点，命令如何绑定到命令源对象

绑定命令的方式通常是通过控件的事件、或者响应键盘或鼠标的操作去实现的，在这里，我们演示这两种方法，第一种是通过快捷键去绑定上面的命令。

```xml
<Window.InputBindings>
    <KeyBinding Key="O" Modifiers="Control" Command="ApplicationCommands.Open" />
    <KeyBinding Key="X" Modifiers="Control" Command="ApplicationCommands.Cut" />
    <KeyBinding Key="V" Modifiers="Control" Command="ApplicationCommands.Paste" />
    <KeyBinding Key="S" Modifiers="Control" Command="ApplicationCommands.Save" />
</Window.InputBindings>
```

这些KeyBinding可以定义快捷键，并指向某个命令。第二种方式就是创建一个前端控件，比如实例化一个按钮，利用按钮的Command绑定命令。

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="auto"/>
        <RowDefinition/>
    </Grid.RowDefinitions>
    <StackPanel Orientation="Horizontal">
        <Button Content="打开" Margin="5" Command="ApplicationCommands.Open"/>
        <Button Content="剪切" Margin="5" Command="ApplicationCommands.Cut"/>
        <Button Content="粘贴" Margin="5" Command="ApplicationCommands.Paste"/>
        <Button Content="保存" Margin="5" Command="ApplicationCommands.Save"/>
    </StackPanel>
    <TextBox x:Name="textbox" Grid.Row="1" Margin="5" TextWrapping="Wrap">
        
    </TextBox>
</Grid>
```

![[assets/WPF从小白到大佬：命令：：ApplicationCommands命令实战/2394eb8bd9a10edf04bb41defa5a0b43_MD5.jpg]]

坦白说，WPF的预定义命令并不常用，但是作为开发者，还是很有必要全面了解一下相关知识。我们更多时候还是使用ICommand的自定义命令，下一讲，我们将介绍控件的其它事件如何去引用一个命令。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：071-《ApplicationCommands与记事本开发实战》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月12日