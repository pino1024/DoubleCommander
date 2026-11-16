# Double Commander（界面与操作优化版）

本仓库是 [Double Commander](https://github.com/doublecmd/doublecmd) 的个人修改版，主要针对 Windows 下中文使用场景，做了几处界面和操作上的小优化。

## 来源

- 上游项目：<https://github.com/doublecmd/doublecmd>（作者 Alexander Koblov 等）
- 修改基线：上游提交 `b33d3b3`（2026-11-10，`FIX: compilation error on Windows`），程序版本号 `1.3.0~1`
- 许可证：沿用上游的 GNU GPL v2，见 [LICENSE.md](LICENSE.md)。本仓库的修改同样以 GPL v2 发布。
- 本仓库保留了上游的完整提交历史，我的改动是在其上的独立提交，可用 `git log` 或 `git diff b33d3b3` 查看全部差异。
- 本仓库与上游项目无隶属关系，问题请不要提交到上游。

## 优化改动点

只修改了 4 个源码文件，共 36 行新增、10 行删除。

| 序号 | 改动 | 文件 |
|---|---|---|
| 1 | 单击路径栏直接进入路径编辑状态，可以粘贴路径后回车跳转。原来单击会弹出历史菜单。点击路径中的某一级子目录，仍然是跳到那一级 | `src/fileviews/ufileviewheader.pas`（`PathLabelClick`） |
| 2 | 右键点击路径栏弹出历史目录菜单。原来右键是进入编辑。中键仍然是目录热点列表 | `src/fileviews/ufileviewheader.pas`（`PathLabelMouseUp`） |
| 3 | 目录名默认不再加方括号，例如 `[掉落]` 显示为 `掉落`。选项里的"目录名加方括号"开关仍然可用 | `src/uglobs.pas`（`gDirBrackets` 默认值改为 `False`） |
| 4 | 放大文件页签：字体 12，标签高度 32，更容易点中 | `src/ufileviewnotebook.pas`（构造函数） |
| 5 | 放大盘符栏：字体 12，按钮高度改为图标大小 + 14 | `src/fmain.pas` |
| 6 | 选中的页签更醒目：浅蓝底色、顶部 3 像素深蓝强调条、粗体文字。仅在 Windows 亮色模式、页签在顶部时生效，深色模式不受影响 | `src/ufileviewnotebook.pas`（`PaintWindow`） |

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
- 第 6 项（选中页签配色）作者没有拿到界面截图确认，颜色和文字位置如果不合适，可以改 `PaintWindow` 里的 `RGBToColor` 取值。
- 其他平台（Linux、macOS）没有测试。

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
