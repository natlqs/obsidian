ItemsPanelTemplate用于指定集合控件中元素与元素之间的布局的方式，所以，ItemsPanelTemplate其实就是一个布局面板，而我们在前面的章节中已经学习了WPF的面板控件，它们都继承于Panel基类，分别是Grid、UniformGrid、StackPanel、WrapPanel、DockPanel、Canvas等。而在使用ItemsPanelTemplate模板去设置某一个集合控件的元素布局面板时，默认使用StackPanel布局，或者WrapPanel。

例如在上一章节的ItemsControl控件中，我们有4个元素，它们都是垂直排列的。我们可以修改ItemsPanel属性，用以设置元素以瀑布流的方式排列显示。

```xml
<ItemsControl.ItemsPanel>
    <ItemsPanelTemplate>
        <WrapPanel/>
    </ItemsPanelTemplate>
</ItemsControl.ItemsPanel>
```

![[assets/WPF从小白到大佬：模板：：ItemsPanelTempate元素模板/6e3b2b96e0404c1327143203bd91921c_MD5.jpg]]

如上图所示，此时ItemsControl中的元素便随着Window窗体大小的改变，而自适应水平排列其中的元素。

> **当前课程源码下载：（注明：本站所有源代码请按标题搜索）**
> 
> 文件名：067-《ItemsPanelTemplate元素面板模板》-源代码  
> 链接：https://pan.baidu.com/s/1yu-q4tUtl0poLVgmcMfgBA  
> 提取码：wpff

——重庆教主 2023年10月7日