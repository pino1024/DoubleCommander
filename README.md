# Double Commander（界面与操作优化版）

本仓库是 [Double Commander](https://github.com/doublecmd/doublecmd) 的个人修改版，主要针对 Windows 下中文使用场景，做了几处界面和操作上的小优化。

## 来源

- 上游项目：<https://github.com/doublecmd/doublecmd>（作者 Alexander Koblov 等）
- 修改基线：上游提交 `b33d3b3`（2026-11-10，`FIX: compilation error on Windows`），程序版本号 `1.3.0~1`
- 许可证：沿用上游的 GNU GPL v2，见 [LICENSE.md](LICENSE.md)。本仓库的修改同样以 GPL v2 发布。
- 本仓库保留了上游的完整提交历史，我的改动是在其上的独立提交，可用 `git log` 或 `git diff b33d3b3` 查看全部差异。
- 本仓库与上游项目无隶属关系，问题请不要提交到上游。

## 优化改动点

共改动 6 个源码文件（相对基线 `b33d3b3`：161 行新增、16 行删除），按主题归纳如下：

1. **快速跳转目录**
   - 单击路径栏直接进入编辑，可粘贴路径后回车；右键弹出历史目录菜单。
   - 盘符栏后新增"桌面、下载、文档"三个带图标的按钮（仅 Windows）。
2. **界面放大，更易点击**：页签、盘符栏、路径栏字体统一放大到 12 号，页签和盘符按钮也相应加高。
3. **当前页签更醒目**：选中的页签用浅蓝底色加顶部蓝条标出（仅 Windows 亮色模式）。
4. **路径行更清晰**：路径行不再随活动面板变成深蓝色，改为白底，上下加分隔线，与页签行、文件列表分开；点击编辑时才显示浅蓝底。
5. **目录名不再带方括号**：`[掉落]` 显示为 `掉落`，选项里的开关仍可重新打开。

涉及文件：`fmain.pas`、`ufileviewheader.pas`、`ufileviewnotebook.pas`、`upathlabel.pas`、`uglobs.pas`、`ukastoolitemsextended.pas`（均在 `src/` 下）。

注意：路径行未激活颜色改为固定值后，"选项 → 颜色 → 路径"中对应的未激活颜色设置不再生效。

### 可选配置

要去掉盘符旁的"剩余空间"那一行，可在"配置 → 选项 → 布局"取消对应勾选，或在 `%APPDATA%\doublecmd\doublecmd.xml` 中设置 `DrivesListButton Enabled="False"` 和 `DriveFreeSpace` 为 `False`。顶部盘符按钮条不受影响。

### 验证情况

仅在 Windows 上用 Lazarus 4.0 / FPC 3.2.2 编译，界面效果截图确认过。Linux、macOS 未编译、未测试。


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
