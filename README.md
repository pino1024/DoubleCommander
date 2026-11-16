# Double Commander（界面与操作优化版）

本仓库是 [Double Commander](https://github.com/doublecmd/doublecmd) 的个人修改版，主要针对 Windows 下中文使用场景，做了几处界面和操作上的小优化。

## 来源

- 上游项目：<https://github.com/doublecmd/doublecmd>（作者 Alexander Koblov 等）
- 修改基线：上游提交 `b33d3b3`（2026-11-10，`FIX: compilation error on Windows`），程序版本号 `1.3.0~1`
- 许可证：沿用上游的 GNU GPL v2，见 [LICENSE.md](LICENSE.md)。本仓库的修改同样以 GPL v2 发布。
- 本仓库保留了上游的完整提交历史，我的改动是在其上的独立提交，可用 `git log` 或 `git diff b33d3b3` 查看全部差异。
- 本仓库与上游项目无隶属关系，问题请不要提交到上游。

## 优化改动点

共修改 6 个源码文件，161 行新增、16 行删除（相对于基线 `b33d3b3`，不含本 README）。

| 序号 | 改动 | 文件 |
|---|---|---|
| 1 | 单击路径栏直接进入路径编辑状态，可以粘贴路径后回车跳转。原来单击会弹出历史菜单。点击路径中的某一级子目录，仍然是跳到那一级 | `src/fileviews/ufileviewheader.pas`（`PathLabelClick`） |
| 2 | 右键点击路径栏弹出历史目录菜单。原来右键是进入编辑。中键仍然是目录热点列表 | `src/fileviews/ufileviewheader.pas`（`PathLabelMouseUp`） |
| 3 | 目录名默认不再加方括号，例如 `[掉落]` 显示为 `掉落`。选项里的"目录名加方括号"开关仍然可用 | `src/uglobs.pas`（`gDirBrackets` 默认值改为 `False`） |
| 4 | 放大文件页签：字体 12，标签高度 32，更容易点中 | `src/ufileviewnotebook.pas`（构造函数） |
| 5 | 放大盘符栏：字体 12，按钮高度改为图标大小 + 14 | `src/fmain.pas` |
| 6 | 选中的页签更醒目：浅蓝底色加顶部 3 像素深蓝强调条，文字不加粗。仅在 Windows 亮色模式、页签在顶部时生效，深色模式不受影响 | `src/ufileviewnotebook.pas`（`PaintWindow`） |
| 7 | 路径栏字体默认从 10 号放大到 12 号，与页签、盘符栏一致 | `src/uglobs.pas`（`dcfPathEdit` 默认字号） |
| 8 | 盘符栏后追加"桌面、下载、文档"三个按钮，带系统文件夹图标。路径取自 Windows 已知文件夹，移动过位置也能正确跳转。界面语言为中文时显示中文名，否则显示 `Desktop`、`Downloads`、`Documents`。左盘符栏作用于左面板，右盘符栏作用于右面板。仅 Windows 生效 | `src/fmain.pas`（`AddCommonFolderButtons`），新增类型 `TKASFolderItem` 在 `src/ukastoolitemsextended.pas` |
| 9 | 路径行不再随活动面板变成选中的深蓝色，固定为白底黑字。点击进入编辑时，输入框变为浅蓝底，与选中页签同色 | `src/fileviews/ufileviewheader.pas`（`SetActive`、`PathLabelSetColor`、`ShowPathEdit`） |
| 10 | 路径行上下各画一条灰色分隔线，使页签行、路径行、文件列表之间界线清楚 | `src/upathlabel.pas`（`Paint`） |

第 9 项的副作用：路径行未激活时的颜色固定为系统的"窗口白"和"窗口文字色"，"选项 → 颜色 → 路径"里对应的未激活颜色设置不再生效。

### 只改配置、未改源码的项

"盘符与剩余空间那一行"的显示，是用户配置里的选项，没有改源码。要去掉这一行，在 `%APPDATA%\doublecmd\doublecmd.xml` 里设置：

```xml
<DrivesListButton Enabled="False">
...
<DriveFreeSpace>False</DriveFreeSpace>
```

也可以在"配置 → 选项 → 布局"里取消对应勾选。顶部的盘符按钮条不受影响。

### 验证情况

- 以上改动都在 Windows 上用 Lazarus 4.0 / FPC 3.2.2 编译通过，程序能正常启动、响应正常。
- 第 6、8、9、10 项在 Windows 上截取程序窗口确认过显示效果。第 10 项的分隔线只有 1 像素，主要靠第 9 项的白色底色来区分页签行。
- 选中页签的颜色可以改 `PaintWindow` 里的 `RGBToColor` 取值。
- 第 8 项在非 Windows 平台是空实现，不会出现这三个按钮。
- Linux、macOS 没有编译和测试。

## 构建（Windows）

1. 安装 [Lazarus](https://www.lazarus-ide.org/) 4.0（带 FPC 3.2.2），例如 `choco install lazarus`。
2. 设置环境变量并构建：

```bat
set LAZARUS_HOME=C:\lazarus
set PATH=C:\lazarus;C:\lazarus\fpc\3.2.2\bin\x86_64-win64;%PATH%
build.bat components
build.bat doublecmd
```

3. 生成的 `doublecmd.exe` 在仓库根目录。如需压缩包、FTP 等插件，再执行 `build.bat plugins`。

构建日志里会有 `File "dmhigh.json" not found`、`File "doublecmd.svgz" not found` 两条提示，是资源文件缺失，不影响编译和运行。

界面语言：程序自带简体中文和繁体中文翻译（`language/` 目录），系统区域为中文时会自动选用，也可以在"配置 → 选项 → 语言"里手动切换。

---

以下是上游项目的原始 README。

**Double Commander** is a [free](https://www.gnu.org/philosophy/free-sw.html) cross-platform open source file manager with two panels side by side (or one above the other). It is inspired by Total Commander and features some innovative new ideas. 

Double Commander can be run on several platforms and operating systems.
It supports 32-bit and 64-bit processors. See [Supported platforms](https://github.com/doublecmd/doublecmd/wiki/Supported-platforms)
for a complete list.

See Double Commander in action in the [Screenshot Gallery](https://doublecmd.sourceforge.io/gallery).

## Where to start

### Download

Go to the [Double Commander download page](https://sourceforge.net/p/doublecmd/wiki/Download) to download the latest release.

You can check the latest version on the [Versions](https://github.com/doublecmd/doublecmd/wiki/Versions) page.

See if Double Commander is supported for your platform on the [Supported
platforms](https://github.com/doublecmd/doublecmd/wiki/Supported-platforms) page.

### Develop

For more information on the development of Double Commander,
see the [Development](https://github.com/doublecmd/doublecmd/wiki/Development) page.

### Discuss
  
Go to our [forum](https://doublecmd.h1n.ru) for discussions. There are English and Russian sections.

If you want to stay up-to-date with the project, you can check out the available [news feeds](https://github.com/doublecmd/doublecmd/wiki/News-feeds).
