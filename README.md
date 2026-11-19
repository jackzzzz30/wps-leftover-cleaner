# WPS Leftover Cleaner / WPS 残留清理

One double-click to undo what WPS Office did to your Windows file associations and right-click **New** menu, so Microsoft Office (Microsoft 365) works normally again.

双击一次，撤销 WPS Office 对 Windows 文件关联和右键“新建”菜单的修改，让 Microsoft Office（Microsoft 365）恢复正常。

---

## The problem / 问题

After installing (and even after uninstalling) WPS Office, you may see:

- The right-click **New** menu still shows WPS entries ("WPS 文字 文档", "WPS 表格 工作表", "WPS PDF Document" …).
- **New → Microsoft Word / Excel / PowerPoint** entries are missing, even after repairing Office.
- The Word / Excel / PowerPoint icons in the **New** menu are blank.

Office repair alone does not fix this, because WPS writes per-user overrides (`HKEY_CURRENT_USER\Software\Classes`) that take priority over Office's own system-wide settings, and Office repair does not touch them.

安装（甚至卸载）WPS Office 之后，可能出现：

- 右键“新建”里仍然有 WPS 的选项（WPS 文字 文档、WPS 表格 工作表、WPS PDF 等）。
- 修复 Office 之后，“新建”里还是没有 Word / Excel / PowerPoint。
- “新建”菜单里 Word / Excel / PowerPoint 的图标是空白的。

只修复 Office 不管用：WPS 在用户级注册表（`HKEY_CURRENT_USER\Software\Classes`）写了覆盖设置，优先级高于 Office 的系统级设置，而 Office 修复不会去动这些。

## What the script does / 脚本做了什么

[`clean-wps-leftovers.reg`](clean-wps-leftovers.reg):

1. Points `.doc` `.docx` `.xls` `.xlsx` `.ppt` `.pptx` back to the Office file types (`Word.Document.12`, `Excel.Sheet.12`, `PowerPoint.Show.12` …).
   把 `.doc` `.docx` `.xls` `.xlsx` `.ppt` `.pptx` 的默认类型改回 Office。
2. Deletes the WPS sub-keys left under those extensions (`WPS.Docx.6`, `ET.Xlsx.6`, `WPP.PPTX.6` …).
   删除 WPS 挂在这些扩展名下的子项。
3. Deletes WPS's own file types and its PDF **New** entry (`.wps` `.et` `.dps` `.pdfwpsshellnew`).
   删除 WPS 自己的文件类型和 WPS PDF 的“新建”项。
4. Removes per-user overrides of the Office file types (the usual cause of blank icons), so Office's system-wide settings apply.
   删除用户级对 Office 文件类型的覆盖（图标空白的常见原因），让 Office 的系统级设置生效。
5. Clears Explorer's cached **New** menu so it is rebuilt.
   清除资源管理器的“新建”菜单缓存。

It only removes WPS entries and per-user overrides. Office's own system-wide registration is not deleted.

脚本只删除 WPS 的条目和用户级覆盖，不会删除 Office 自己的系统级注册信息。

## Usage / 使用方法

1. Download [`clean-wps-leftovers.reg`](clean-wps-leftovers.reg) and double-click it → **Yes** → **OK** (administrator rights required).
   下载 `clean-wps-leftovers.reg`，双击 →“是”→“确定”（需要管理员权限）。
2. Run a **Quick Repair** of Microsoft 365 / Office:
   **Settings → Apps → Installed apps → Microsoft 365 → ⋯ → Modify → Quick Repair**.
   对 Microsoft 365 / Office 做一次 **快速修复**：
   **设置 → 应用 → 已安装的应用 → Microsoft 365 → ⋯ → 修改 → 快速修复**。
3. Restart the computer.
   重启电脑。

If something still looks wrong, run **Online Repair** instead of Quick Repair in step 2.

如果还有问题，第 2 步改用 **联机修复**。

## Requirements / 适用环境

- Windows 10 / 11
- Microsoft Office or Microsoft 365 installed / 已安装 Microsoft Office 或 Microsoft 365
- WPS Office already uninstalled, or you no longer want it as the default / 已卸载 WPS，或不再希望 WPS 作为默认程序

## Disclaimer / 免责声明

This edits the Windows registry. It was written for and tested on one machine; registry layouts can differ. Back up the registry first if you are unsure (**regedit → File → Export**). Use at your own risk.

本脚本会修改注册表，只在一台电脑上实际测试过，不同电脑的注册表情况可能不同。不放心的话先备份注册表（**注册表编辑器 → 文件 → 导出**）。使用风险自负。

## License / 许可证

[MIT](LICENSE)
