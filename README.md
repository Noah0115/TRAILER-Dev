# TRAILER游戏盒反编译脱壳学习(dnSpy+de4dot)

#### 2023年7月21日 18:12:21

操作过程：

先使用de4dot进行脱壳

![image-20230728121559709](http://sapic.lyh27.top/static/upload/admin/image-20230728121559709.png)

然后打开Dnspy，打开脱壳后的exe文件，右键资源中的dll，选择保存**.dll，将资源中的dll保存到磁盘上，然后dnspy打开这些dll文件

![image-20230728130040121](http://sapic.lyh27.top/static/upload/admin/image-20230728130040121.png)

打开DLL后程序集资源管理器：

<img src="http://sapic.lyh27.top/static/upload/admin/image-20230728130237477.png"/>

注意：每个游戏的程序包包名不同，要对fghjaz进行替换

MetroDemo.YeMian1a.YeMian1.jianceAqidong():

![image-20230728125832310](http://sapic.lyh27.top/static/upload/admin/image-20230728125832310.png)

```c#
// MetroDemo.YeMian1a.YeMian1
// Token: 0x0600000B RID: 11
public void jianceAqidong(object sender, global::System.EventArgs e)
{
	string defau = "steam_api64.dll";
	string mutl = "steam_api65.dll";
	string single = "steam_api66.dll";
	string weizhiz = global::CommonServiceLocator.ServiceLocator.Current.GetInstance<global::fghjaz.ViewModel.MainViewModel>().GetProjectRootPath() + "\\..\\..\\..\\Engine\\Binaries\\ThirdParty\\Steamworks\\Steamv146\\Win64\\";
	if (!this.IfIsOccupied(weizhiz + defau))
	{
		foreach (global::System.Diagnostics.Process process in global::System.Diagnostics.Process.GetProcesses())
		{
			if (process.ProcessName == "Steam")
			{
				process.Kill();
			}
		}
		global::System.Threading.Thread.Sleep(500);
		if (global::System.Diagnostics.Process.GetProcessesByName("Steam").Length != 0)
		{
			foreach (global::System.Diagnostics.Process process2 in global::System.Diagnostics.Process.GetProcesses())
			{
				if (process2.ProcessName == "Steam")
				{
					process2.Kill();
				}
			}
		}
	}
	global::System.Threading.Thread.Sleep(1000);
	string panduandjorlj = "";
	if (!this.IfIsOccupied(weizhiz + defau))
	{
		global::System.Windows.MessageBox.Show("steam进程或未完全关闭,请重试,或重启电脑在试");
		this.KaiShiYouXi.Content = "启动方式";
		this.KaiShiYouXi.IsEnabled = true;
	}
	else
	{
		if (global::System.Diagnostics.Process.GetProcessesByName("Steam").Length != 0)
		{
			global::System.Threading.Tasks.Task.Run(delegate()
			{
				global::System.IO.File.Copy(weizhiz + mutl, weizhiz + defau, true);
				panduandjorlj = "l";
			});
		}
		else
		{
			global::System.Threading.Tasks.Task.Run(delegate()
			{
				global::System.IO.File.Copy(weizhiz + single, weizhiz + defau, true);
				panduandjorlj = "d";
			});
		}
		global::System.Threading.Thread.Sleep(500);
		if (panduandjorlj == "d")
		{
			global::System.Windows.MessageBox.Show("当前单机模式..\r\n详情查看教程(后台开启Steam则为联机模式)\r\n点“确定”继续");
		}
		else
		{
			global::System.Windows.MessageBox.Show("当前联机模式..\r\n详情查看教程(后台关闭Steam则为单机模式)\r\n点“确定”继续");
		}
		string exe = global::MetroDemo.YeMian1a.YeMian1.c;
		string local = global::CommonServiceLocator.ServiceLocator.Current.GetInstance<global::fghjaz.ViewModel.MainViewModel>().GetProjectRootPath();
		if (global::System.IO.File.Exists(local + "\\" + exe + ".exe"))
		{
			try
			{
				string pathLink = local + "\\" + exe + ".lnk";
				global::IWshRuntimeLibrary.IWshShortcut wshShortcut = (global::IWshRuntimeLibrary.IWshShortcut)new global::IWshRuntimeLibrary.WshShellClass().CreateShortcut(pathLink);
				wshShortcut.TargetPath = local + "\\" + exe + ".exe";
				wshShortcut.WorkingDirectory = local;
				wshShortcut.WindowStyle = 1;
				wshShortcut.Description = "设置";
				wshShortcut.Arguments = null;
				wshShortcut.Save();
				if (global::System.IO.File.Exists(local + "\\" + exe + ".lnk"))
				{
					global::System.Threading.Tasks.Task.Run(delegate()
					{
						global::System.Diagnostics.Process.Start(local + "\\" + exe + ".lnk");
					});
					this.KaiShiYouXi.Content = "启动方式";
					this.KaiShiYouXi.IsEnabled = true;
				}
				else
				{
					global::System.Windows.MessageBox.Show("快捷目标丢失1");
					this.KaiShiYouXi.Content = "启动方式";
					this.KaiShiYouXi.IsEnabled = true;
				}
				goto IL_31A;
			}
			catch (global::System.Exception ex)
			{
				global::System.Windows.MessageBox.Show(ex.Message);
				goto IL_31A;
			}
		}
		global::System.Windows.MessageBox.Show("快捷目标丢失0");
		this.KaiShiYouXi.Content = "启动方式";
		this.KaiShiYouXi.IsEnabled = true;
	}
	IL_31A:
	global::MetroDemo.YeMian1a.YeMian1.jianceA.Stop();
}

```

![image-20230728125919649](http://sapic.lyh27.top/static/upload/admin/image-20230728125919649.png)

JiHuoA.JiHuo.LiJiJiHuo_Click()：

```C#
// JiHuoA.JiHuo
// Token: 0x06000108 RID: 264
private void LiJiJiHuo_Click(object sender, global::System.Windows.RoutedEventArgs e)
{
	this.JieYaGuoCheng();
	this.win.HideMetroDialogAsync(this, null);
}

```

注意：最新版的启动器使用IntelliLock加壳，反编译的时候出现命名编译错误，只需要删除反混淆生成的代码，同时修改LiJiJiHuo_Click()函数，不需要修改jianceAqidong()函数。

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

不要忘记保存！！！