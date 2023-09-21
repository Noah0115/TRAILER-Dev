# TRAILER游戏盒反编译及脱壳学习(dnSpy+de4dot)

# 操作过程：

## ①使用de4dot脱壳

打开de4dot-All-Version-2021文件夹中的De4Dot ToolKit V 2.02222.exe

![image-20230728121559709](http://sapic.lyh27.top/static/upload/admin/image-20230728121559709.png)

根据系统位数选择x64或x32，然后在下拉菜单中选择de4dot_Latest，最后将.exe拖入框中，弹出cmd窗口并如下图显示即说明脱壳完成。

![image-20230921150521907](http://sapic.lyh27.top/static/upload/admin/image-20230921150521907.png)

脱壳后的exe文件在同一个目录下。

## ②使用Dnspy进行反编译

打开Dnspy，在Dnspy中打开脱壳后的exe文件，右键资源中的dll，选择保存**.dll，将资源中的dll保存到磁盘上

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230728130040121.png" alt="image-20230728130040121" style="zoom: 80%;" />

再将保存后的dll文件全部用Dnspy打开，如下图：

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230728130237477.png" style="zoom: 80%;" />

## ③反编译

#### 方法一：

注意：每个游戏的程序包包名不同

找到**程序名.JiHuoA.LiJiJiHuo_Click()**函数

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230921151315792.png" alt="image-20230921151315792" style="zoom:80%;" />

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230921151438820.png" alt="image-20230921151438820" style="zoom:80%;" />

其中关键代码为：

```c#
# 隐藏UI函数
this.win.HideMetroDialogAsync(this, null);
# 激活函数
this.JiHuoZou();
# 解压函数
this.JieYaGuoCheng();
```

在代码中右键，选择编辑方法，在弹出的编辑框中进行代码修改，通常情况下，只需保留关键代码即可：

```c#
using System;
using System.CodeDom.Compiler;
using System.ComponentModel;
using System.Diagnostics;
using System.IO;
using System.Text.RegularExpressions;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Documents;
using System.Windows.Markup;
using System.Windows.Threading;
using CommonServiceLocator;
using ControlzEExx.Controls;
using ControlzEExx.Controls.Dialogs;
using DengLuZhangHaoLei.Core.Common;
using DuoXianCheng.Core.Common;
using fghjaz.ViewModel;
using Ionic.Zip;
using MetroDemo.YeMian1a;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace JiHuoA
{
	public partial class JiHuo : CustomDialog, IComponentConnector
	{
		private void LiJiJiHuo_Click(object sender, RoutedEventArgs e)
		{
			this.JiHuoZou();
			this.JieYaGuoCheng();
			this.win.HideMetroDialogAsync(this, null);
		}
	}
}
```

修改好代码，点击编译，点击左上角的文件，选择**保存模块**，再按F5调试程序，程序打开后，提示输入激活码，不用管，直接点击确定，再点击“启动方式”按钮，选择创建快捷方式就可以了。

![image-20230921152336043](http://sapic.lyh27.top/static/upload/admin/image-20230921152336043.png)

![image-20230921152137342](http://sapic.lyh27.top/static/upload/admin/image-20230921152137342.png)

![image-20230921152210445](http://sapic.lyh27.top/static/upload/admin/image-20230921152210445.png)

注意：最新版的启动器使用IntelliLock加壳，反编译的时候出现方法名混乱的情况，其实只需要删除反混淆生成的代码，同时修改LiJiJiHuo_Click()函数如下图所示：

```c#
public partial class JiHuo : CustomDialog, IComponentConnector
	{
		private void LiJiJiHuo_Click(object sender, RoutedEventArgs e)
		{
			this.metroWindow_0.HideMetroDialogAsync(this, null);
			this.method_0();
			this.method_5();
		}
	}
```

#### 方法二：

对于更新时间为2021年及以前的游戏，安装包加密方式有所区别，如果在解压的时候提示激活码

![image-20230921161148325](http://sapic.lyh27.top/static/upload/admin/image-20230921161148325.png)

那么需要另一个工具InnoExtractor进行提取游戏文件：

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230921161212698.png" alt="image-20230921161212698" style="zoom: 67%;" />

将exe文件拖入进去，加载的时间很长，需要耐心等待，加载完成后如下图：

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230921161448275.png" alt="image-20230921161448275" style="zoom:80%;" />

点击上方提取并设置提取路径，最后点击确定

![image-20230921161547245](http://sapic.lyh27.top/static/upload/admin/image-20230921161547245.png)

![image-20230921161627722](http://sapic.lyh27.top/static/upload/admin/image-20230921161627722.png)



其中{app}目录即是游戏目录，至此方法二结束。