# WpfWindowChromeExample

原文地址：[在 WPF 中使用 WindowChrome 自定义标题栏](https://www.developerastrid.com/wpf/wpf-windowchrome-title-bar/)

本文将介绍在 WPF 中如何使用 WindowChrome 自定义标题栏。

## 一、使用 WindowChrome

在 `<Window></Window>` 节点里写如下代码便能够完成客户区（Client Area）到非客户区（Non-client Area）的覆盖：

```xml
<WindowChrome.WindowChrome>
    <WindowChrome />
</WindowChrome.WindowChrome>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271350196.png" alt="样式已经被遮挡" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>样式已经被遮挡</p></figcaption></figure>

现在，为了能够观察到 `WindowChrome` 各种属性设置的效果，我们为 `Window` 定义一个新的 `Template`，里面就是空的，这样就没有什么内容能够遮挡我们设置的样式了。

```xml
<Window.Template>
    <ControlTemplate TargetType="Window">
        <Border />
    </ControlTemplate>
</Window.Template>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271352396.png" alt="没有遮挡的窗口" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>没有遮挡的窗口</p></figcaption></figure>

然而即便如此，我们也只解决了系统主题色边框的问题，没有解决调整窗口的拖拽热区问题。而且边框还如此之丑。

### 1.1 `GlassFrameThickness` 和 `NonClientFrameEdges`

```xml
<WindowChrome.WindowChrome>
    <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right"/>
</WindowChrome.WindowChrome>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271356743.png" alt="设置 GlassFrameThickness 和 NonClientFrameEdges" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>设置 GlassFrameThickness 和 NonClientFrameEdges</p></figcaption></figure>

到这里，我们算是模拟得比较像了。

## 二、定制 Window 的控件模板

`WindowChrome` 提供客户区内容覆盖到非客户区的能力，所以我们通过定制 `Window` 的 `ControlTemplate` 能够在保证原生窗口体验的同时，尽可能定制我们的窗口样式。

```xml
<Window.Template>
    <ControlTemplate TargetType="Window">
        <Border Name="RootBorder">
            <Grid Background="Aqua">
                <AdornerDecorator>
                    <ContentPresenter/>
                </AdornerDecorator>
            </Grid>
        </Border>
    </ControlTemplate>
</Window.Template>

<WindowChrome.WindowChrome>
    <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right"/>
</WindowChrome.WindowChrome>

<Grid>
    <TextBlock Text="12345"></TextBlock>
</Grid>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271421984.png" alt="客户区内容覆盖到非客户区" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>客户区内容覆盖到非客户区</p></figcaption></figure>

可以看到 `<Grid>`（客户区）的内容覆盖到了非客户区，并且默认的“最小化”、“最大化”、“关闭”功能还是在的，也是可以点击的，只是看不见了而已（被客户区覆盖）。

我们调整一下边距，让非客户区域露出来。

```xml
<Border Name="RootBorder"  Padding="0 30 0 0" BorderBrush="Transparent" BorderThickness="4 0 4 4">
    <Grid Background="Aqua">
        <AdornerDecorator>
            <ContentPresenter/>
        </AdornerDecorator>
    </Grid>
</Border>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271426466.png" alt="调整客户区域的边距" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>调整客户区域的边距</p></figcaption></figure>

### 2.1 定制布局

把 `Grid` 分为上下两行，上面一行作为标题栏，下面一行作为内容主体。

```xml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"></RowDefinition>
        <RowDefinition Height="*"></RowDefinition>
    </Grid.RowDefinitions>
    <!--标题栏 开始-->
    <Border Grid.Row="0" Height="30" Margin="0 -29 0 0">
        <DockPanel>
            <StackPanel Orientation="Horizontal" DockPanel.Dock="Right">
                <Button Content="最小化"></Button>
                <Button Content="最大化"></Button>
                <Button Content="向下还原"></Button>
                <Button Content="关闭"></Button>
            </StackPanel>
            <StackPanel Orientation="Horizontal">
                <TextBlock
                    Text="自定义的标题栏"
                    VerticalAlignment="Center"></TextBlock>
            </StackPanel>
        </DockPanel>
    </Border>
    <!--标题栏 结束-->
    <Grid Grid.Row="1">
        <TextBlock Text="主体内容区域"></TextBlock>
    </Grid>
</Grid>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271434450.png" alt="定制布局" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>定制布局</p></figcaption></figure>

现在，点击右上角，还是会触发默认的“最小化”、“最大化”、“关闭”行为。所以，我们需要禁用它的默认行为。

```xml
<WindowChrome.WindowChrome>
    <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right" UseAeroCaptionButtons="False"/>
</WindowChrome.WindowChrome>
```

### 2.2 自定义“最小化”、“最大化”、“关闭”事件

定义命令

```xml
<Window.CommandBindings>
    <CommandBinding Command="{x:Static SystemCommands.CloseWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_CloseWindow"/>
    <CommandBinding Command="{x:Static SystemCommands.MaximizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MaximizeWindow"/>
    <CommandBinding Command="{x:Static SystemCommands.MinimizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MinimizeWindow"/>
    <CommandBinding Command="{x:Static SystemCommands.RestoreWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_RestoreWindow"/>
</Window.CommandBindings>
```

按钮上绑定命令

```xml
<Button Name="ButtonMinimizeWindow"  Content="最小化"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
<Button Name="ButtonMaximizeWindow" Content="最大化"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
<Button Name="ButtonRestoreWindow" Content="向下还原"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
<Button Name="ButtonCloseWindow" Content="关闭"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
```

### 2.3 美化按钮

定义字体资源，参考 [https://blog.csdn.net/mybelief321/article/details/102461597](https://blog.csdn.net/mybelief321/article/details/102461597)

```xml
<Window.Resources>
    <ResourceDictionary>
        <FontFamily x:Key="IconFont">pack://application:,,,/WindowChromeExample;component/#iconfont,</FontFamily>
    </ResourceDictionary>
</Window.Resources>
```

使用字体资源

```xml
<Button Name="ButtonMinimizeWindow" FontFamily="{StaticResource IconFont}"  Content="&#xe629;"  Width="24" Height="24"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
<Button Name="ButtonMaximizeWindow" FontFamily="{StaticResource IconFont}" Content="&#xe653;"  Width="24" Height="24"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
<Button Name="ButtonRestoreWindow" FontFamily="{StaticResource IconFont}" Content="&#xe677;"  Width="24" Height="24"
        Visibility="Collapsed"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
<Button Name="ButtonCloseWindow" FontFamily="{StaticResource IconFont}" Content="&#xeaf2;"  Width="24" Height="24"
        WindowChrome.IsHitTestVisibleInChrome="True"
        Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271504795.png" alt="美化按钮" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>美化按钮</p></figcaption></figure>

默认的按钮还是会显示，个他加给背景，就看不到了。

```xml
<Border Grid.Row="0" Height="30" Margin="0 -29 0 0">
    <DockPanel Background="Red">
        <StackPanel Orientation="Horizontal" DockPanel.Dock="Right">
            <Button Name="ButtonMinimizeWindow" FontFamily="{StaticResource IconFont}"  Content="&#xe629;"  Width="24" Height="24"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
            <Button Name="ButtonMaximizeWindow" FontFamily="{StaticResource IconFont}" Content="&#xe653;"  Width="24" Height="24"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
            <Button Name="ButtonRestoreWindow" FontFamily="{StaticResource IconFont}" Content="&#xe677;"  Width="24" Height="24"
                    Visibility="Collapsed"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
            <Button Name="ButtonCloseWindow" FontFamily="{StaticResource IconFont}" Content="&#xeaf2;"  Width="24" Height="24"
                    WindowChrome.IsHitTestVisibleInChrome="True"
                    Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
        </StackPanel>
        <StackPanel Orientation="Horizontal">
            <TextBlock
                Text="自定义的标题栏"
                VerticalAlignment="Center"></TextBlock>
        </StackPanel>
    </DockPanel>
</Border>
```

<figure><img src="https://cdn.developerastrid.com/img/202202271507818.png" alt="红色背景" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>红色背景</p></figcaption></figure>

## 三、完整代码

### 3.1 XAML 代码

```xml
<Window x:Class="WindowChromeExample.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="450" Width="800">

    <!--字体资源-->
    <Window.Resources>
        <ResourceDictionary>
            <FontFamily x:Key="IconFont">pack://application:,,,/WindowChromeExample;component/#iconfont,</FontFamily>
        </ResourceDictionary>
    </Window.Resources>

    <!--自定义标题栏 开始-->
    <Window.Template>
        <ControlTemplate TargetType="Window">
            <Border Name="RootBorder"  Padding="0 30 0 0" BorderBrush="Transparent" BorderThickness="4 0 4 4">
                <Grid Background="{TemplateBinding Background}">
                    <AdornerDecorator>
                        <ContentPresenter/>
                    </AdornerDecorator>
                </Grid>
            </Border>
            <ControlTemplate.Triggers>
                <Trigger Property="WindowState" Value="Maximized">
                    <Setter TargetName="RootBorder" Property="BorderThickness" Value="8" />
                </Trigger>
            </ControlTemplate.Triggers>
        </ControlTemplate>
    </Window.Template>
    <!--自定义标题栏 结束-->

    <WindowChrome.WindowChrome>
        <WindowChrome GlassFrameThickness="0 30 0 0" NonClientFrameEdges="Left,Bottom,Right" UseAeroCaptionButtons="False"/>
    </WindowChrome.WindowChrome>

    <!--标题栏按钮事件 开始-->
    <Window.CommandBindings>
        <CommandBinding Command="{x:Static SystemCommands.CloseWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_CloseWindow"/>
        <CommandBinding Command="{x:Static SystemCommands.MaximizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MaximizeWindow"/>
        <CommandBinding Command="{x:Static SystemCommands.MinimizeWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_MinimizeWindow"/>
        <CommandBinding Command="{x:Static SystemCommands.RestoreWindowCommand}" CanExecute="CommandBinding_OnCanExecute" Executed="CommandBinding_OnExecuted_RestoreWindow"/>
    </Window.CommandBindings>
    <!--标题栏按钮事件 结束-->


    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"></RowDefinition>
            <RowDefinition Height="*"></RowDefinition>
        </Grid.RowDefinitions>
        <!--标题栏 开始-->
        <Border Grid.Row="0" Height="30" Margin="0 -29 0 0">
            <DockPanel Background="White">
                <StackPanel Orientation="Horizontal" DockPanel.Dock="Right">
                    <Button
                        Name="ButtonMinimizeWindow"
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xe629;"
                        ToolTip="最小化"
                        Command="{x:Static SystemCommands.MinimizeWindowCommand}"></Button>
                    <Button
                        Name="ButtonMaximizeWindow"
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xe653;"
                        ToolTip="最大化"
                        Command="{x:Static SystemCommands.MaximizeWindowCommand}"></Button>
                    <Button
                        Name="ButtonRestoreWindow"
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xe677;"
                        ToolTip="向下还原"
                        Visibility="Collapsed"
                        Command="{x:Static SystemCommands.RestoreWindowCommand}"></Button>
                    <Button
                        Name="ButtonCloseWindow"
                        WindowChrome.IsHitTestVisibleInChrome="True"
                        FontFamily="{StaticResource IconFont}"
                        Width="32"
                        Content="&#xeaf2;"
                        ToolTip="关闭"
                        Command="{x:Static SystemCommands.CloseWindowCommand}"></Button>
                </StackPanel>
                <StackPanel Orientation="Horizontal">
                    <TextBlock
                        Text="自定义标题栏"
                        VerticalAlignment="Center"></TextBlock>
                </StackPanel>
            </DockPanel>
        </Border>
        <!--标题栏 结束-->
        <Grid Grid.Row="1">
            <TextBlock Text="主体内容"></TextBlock>
        </Grid>
    </Grid>
</Window>
```

### 3.2 C\#

```csharp
using System;
using System.Windows;
using System.Windows.Input;

namespace WindowChromeExample
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            StateChanged += MainWindow_StateChanged;
        }

        private void MainWindow_StateChanged(object? sender, EventArgs e)
        {
            if (WindowState == WindowState.Maximized)
            {
                ButtonRestoreWindow.Visibility = Visibility.Visible;
                ButtonMaximizeWindow.Visibility = Visibility.Collapsed;
            }
            else
            {
                ButtonRestoreWindow.Visibility = Visibility.Collapsed;
                ButtonMaximizeWindow.Visibility = Visibility.Visible;
            }
        }

        private void CommandBinding_OnCanExecute(object sender, CanExecuteRoutedEventArgs e)
        {
            e.CanExecute = true;
        }

        private void CommandBinding_OnExecuted_MinimizeWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.MinimizeWindow(this);
        }

        private void CommandBinding_OnExecuted_MaximizeWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.MaximizeWindow(this);
        }

        private void CommandBinding_OnExecuted_RestoreWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.RestoreWindow(this);
        }

        private void CommandBinding_OnExecuted_CloseWindow(object sender, ExecutedRoutedEventArgs e)
        {
            SystemCommands.CloseWindow(this);
        }
    }
}
```

### 3.3 最终效果

<figure><img src="https://cdn.developerastrid.com/img/202202271523810.png" alt="最终效果" class="img-fluid mx-auto d-block figure-img"><figcaption class="figure-caption text-center"><p>最终效果</p></figcaption></figure>

## 源码地址

到这里，已经完整的介绍了在 WPF 中如何使用 WindowChrome 自定义标题栏。

源码地址：[https://github.com/astrid9527/WpfWindowChromeExample](https://github.com/astrid9527/WpfWindowChromeExample)

## 参考

- [https://docs.microsoft.com/zh-cn/dotnet/api/system.windows.shell.windowchrome?view=net-6.0](https://docs.microsoft.com/zh-cn/dotnet/api/system.windows.shell.windowchrome?view=net-6.0)

- [https://blog.walterlv.com/post/wpf-simulate-native-window-style-using-window-chrome.html](https://blog.walterlv.com/post/wpf-simulate-native-window-style-using-window-chrome.html)

- [https://www.cnblogs.com/dino623/p/customwindowstyle.html](https://www.cnblogs.com/dino623/p/customwindowstyle.html)

- [https://www.codeproject.com/Articles/5255192/Use-WindowChrome-to-Customize-the-Title-Bar-in-WPF](https://www.codeproject.com/Articles/5255192/Use-WindowChrome-to-Customize-the-Title-Bar-in-WPF)
