WPF 提供一组预定义命令。命令库包括以下类：ApplicationCommands、NavigationCommands、MediaCommands、EditingCommands 和 ComponentCommands。 这些类提供诸如 Cut、BrowseBack、BrowseForward、Play、Stop 和 Pause 的命令。这些类仅由 RoutedCommand 对象构成，而不包含命令的实现逻辑。 实现逻辑由在其上执行命令的对象负责。

我们来看看这几个类的定义。

一、ApplicationCommands的定义

```cs
public static class ApplicationCommands
{
    public static RoutedUICommand Cut { get; }
    public static RoutedUICommand Stop { get; }
    public static RoutedUICommand ContextMenu { get; }
    public static RoutedUICommand Properties { get; }
    public static RoutedUICommand PrintPreview { get; }
    public static RoutedUICommand CancelPrint { get; }
    public static RoutedUICommand Print { get; }
    public static RoutedUICommand SaveAs { get; }
    public static RoutedUICommand Save { get; }
    public static RoutedUICommand Close { get; }
    public static RoutedUICommand CorrectionList { get; }
    public static RoutedUICommand Open { get; }
    public static RoutedUICommand Help { get; }
    public static RoutedUICommand SelectAll { get; }
    public static RoutedUICommand Replace { get; }
    public static RoutedUICommand Find { get; }
    public static RoutedUICommand Redo { get; }
    public static RoutedUICommand Undo { get; }
    public static RoutedUICommand Delete { get; }
    public static RoutedUICommand Paste { get; }
    public static RoutedUICommand Copy { get; }
    public static RoutedUICommand New { get; }
    public static RoutedUICommand NotACommand { get; }
 
}
```

二、NavigationCommands 的定义

```cs
public static class NavigationCommands
{
    public static RoutedUICommand BrowseBack { get; }
    public static RoutedUICommand BrowseForward { get; }
    public static RoutedUICommand BrowseHome { get; }
    public static RoutedUICommand BrowseStop { get; }
    public static RoutedUICommand Refresh { get; }
    public static RoutedUICommand Favorites { get; }
    public static RoutedUICommand Search { get; }
    public static RoutedUICommand IncreaseZoom { get; }
    public static RoutedUICommand DecreaseZoom { get; }
    public static RoutedUICommand Zoom { get; }
    public static RoutedUICommand NextPage { get; }
    public static RoutedUICommand PreviousPage { get; }
    public static RoutedUICommand FirstPage { get; }
    public static RoutedUICommand LastPage { get; }
    public static RoutedUICommand GoToPage { get; }
    public static RoutedUICommand NavigateJournal { get; }
 
}
```

三、MediaCommands的定义

```cs
public static class MediaCommands
{
    public static RoutedUICommand Play { get; }
    public static RoutedUICommand DecreaseMicrophoneVolume { get; }
    public static RoutedUICommand IncreaseMicrophoneVolume { get; }
    public static RoutedUICommand BoostBass { get; }
    public static RoutedUICommand DecreaseBass { get; }
    public static RoutedUICommand IncreaseBass { get; }
    public static RoutedUICommand DecreaseTreble { get; }
    public static RoutedUICommand IncreaseTreble { get; }
    public static RoutedUICommand MuteVolume { get; }
    public static RoutedUICommand DecreaseVolume { get; }
    public static RoutedUICommand IncreaseVolume { get; }
    public static RoutedUICommand Select { get; }
    public static RoutedUICommand TogglePlayPause { get; }
    public static RoutedUICommand ChannelDown { get; }
    public static RoutedUICommand ChannelUp { get; }
    public static RoutedUICommand Rewind { get; }
    public static RoutedUICommand FastForward { get; }
    public static RoutedUICommand PreviousTrack { get; }
    public static RoutedUICommand NextTrack { get; }
    public static RoutedUICommand Record { get; }
    public static RoutedUICommand Stop { get; }
    public static RoutedUICommand Pause { get; }
    public static RoutedUICommand MuteMicrophoneVolume { get; }
    public static RoutedUICommand ToggleMicrophoneOnOff { get; }
 
}
```

四、EditingCommands的定义

```cs
public static class EditingCommands
{
public static RoutedUICommand ToggleInsert { get; }
public static RoutedUICommand SelectDownByParagraph { get; }
public static RoutedUICommand SelectUpByParagraph { get; }
public static RoutedUICommand SelectDownByPage { get; }
public static RoutedUICommand SelectUpByPage { get; }
public static RoutedUICommand SelectToLineStart { get; }
public static RoutedUICommand SelectToLineEnd { get; }
public static RoutedUICommand SelectToDocumentStart { get; }
public static RoutedUICommand SelectToDocumentEnd { get; }
public static RoutedUICommand ToggleBold { get; }
public static RoutedUICommand ToggleItalic { get; }
public static RoutedUICommand ToggleUnderline { get; }
public static RoutedUICommand ToggleSubscript { get; }
public static RoutedUICommand ToggleSuperscript { get; }
public static RoutedUICommand IncreaseFontSize { get; }
public static RoutedUICommand DecreaseFontSize { get; }
public static RoutedUICommand AlignLeft { get; }
public static RoutedUICommand AlignCenter { get; }
public static RoutedUICommand AlignRight { get; }
public static RoutedUICommand AlignJustify { get; }
public static RoutedUICommand ToggleBullets { get; }
public static RoutedUICommand ToggleNumbering { get; }
public static RoutedUICommand IncreaseIndentation { get; }
public static RoutedUICommand DecreaseIndentation { get; }
public static RoutedUICommand SelectUpByLine { get; }
public static RoutedUICommand SelectDownByLine { get; }
public static RoutedUICommand SelectLeftByWord { get; }
public static RoutedUICommand SelectRightByWord { get; }
public static RoutedUICommand Delete { get; }
public static RoutedUICommand Backspace { get; }
public static RoutedUICommand DeleteNextWord { get; }
public static RoutedUICommand DeletePreviousWord { get; }
public static RoutedUICommand EnterParagraphBreak { get; }
public static RoutedUICommand EnterLineBreak { get; }
public static RoutedUICommand TabForward { get; }
public static RoutedUICommand TabBackward { get; }
public static RoutedUICommand MoveRightByCharacter { get; }
public static RoutedUICommand MoveLeftByCharacter { get; }
public static RoutedUICommand MoveRightByWord { get; }
public static RoutedUICommand CorrectSpellingError { get; }
public static RoutedUICommand MoveLeftByWord { get; }
public static RoutedUICommand MoveUpByLine { get; }
public static RoutedUICommand MoveDownByParagraph { get; }
public static RoutedUICommand MoveUpByParagraph { get; }
public static RoutedUICommand MoveDownByPage { get; }
public static RoutedUICommand MoveUpByPage { get; }
public static RoutedUICommand MoveToLineStart { get; }
public static RoutedUICommand MoveToLineEnd { get; }
public static RoutedUICommand MoveToDocumentStart { get; }
public static RoutedUICommand MoveToDocumentEnd { get; }
public static RoutedUICommand SelectRightByCharacter { get; }
public static RoutedUICommand SelectLeftByCharacter { get; }
public static RoutedUICommand MoveDownByLine { get; }
public static RoutedUICommand IgnoreSpellingError { get; }
 
}
```

五、ComponentCommands的定义

```cs
public static class ComponentCommands
{
public static RoutedUICommand ScrollPageUp { get; }
public static RoutedUICommand MoveFocusBack { get; }
public static RoutedUICommand MoveFocusForward { get; }
public static RoutedUICommand MoveFocusDown { get; }
public static RoutedUICommand MoveFocusUp { get; }
public static RoutedUICommand SelectToPageDown { get; }
public static RoutedUICommand SelectToPageUp { get; }
public static RoutedUICommand SelectToEnd { get; }
public static RoutedUICommand SelectToHome { get; }
public static RoutedUICommand ExtendSelectionRight { get; }
public static RoutedUICommand ExtendSelectionLeft { get; }
public static RoutedUICommand ExtendSelectionDown { get; }
public static RoutedUICommand MoveFocusPageUp { get; }
public static RoutedUICommand ExtendSelectionUp { get; }
public static RoutedUICommand MoveToPageUp { get; }
public static RoutedUICommand MoveToEnd { get; }
public static RoutedUICommand MoveToHome { get; }
public static RoutedUICommand MoveDown { get; }
public static RoutedUICommand MoveUp { get; }
public static RoutedUICommand MoveRight { get; }
public static RoutedUICommand MoveLeft { get; }
public static RoutedUICommand ScrollByLine { get; }
public static RoutedUICommand ScrollPageRight { get; }
public static RoutedUICommand ScrollPageLeft { get; }
public static RoutedUICommand ScrollPageDown { get; }
public static RoutedUICommand MoveToPageDown { get; }
public static RoutedUICommand MoveFocusPageDown { get; }
 
}
```

这些命令都是RoutedUICommand类型，RoutedUICommand的父类是RoutedCommand，而RoutedCommand继承了ICommand接口。所以，本质上，RoutedUICommand也是一个ICommand对象。我们需要了解一下它的父类的定义

```cs
public class RoutedCommand : ICommand
{
      public RoutedCommand();
      public RoutedCommand(string name, Type ownerType);
      public RoutedCommand(string name, Type ownerType, InputGestureCollection inputGestures);
 
      public string Name { get; }
      public Type OwnerType { get; }
      public InputGestureCollection InputGestures { get; }
 
      public event EventHandler CanExecuteChanged;
 
      public bool CanExecute(object parameter, IInputElement target);
      public void Execute(object parameter, IInputElement target);
 
}
```

这些预先定义好的命令并没有实现业务逻辑，如果要实现业务逻辑，需要用到CommandBinding对象。下一节，我们将介绍CommandBinding对象在命令与逻辑代码中间发挥了什么关键性的作用。

——重庆教主 2023年10月11日