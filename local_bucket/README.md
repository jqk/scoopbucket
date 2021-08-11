# Scoop安装本地程序说明

## 一、原因

有些程序由于不易下载等原因，无法使用`scoop`的在线安装功能，因此建立本地安装脚本。

## 二、安装程序位置

各个程序的安装文件已经在安装脚本文件中指定为`\Scoop\local_installers`。

该路径默认执行`scoop`时的位置与安装文件在相同盘。

如果安装文件不在该路径，有两个解决方案：
1. 修改各个安装脚本中的`url`，使其指向正确的安装文件。这样做比较麻烦，不建议采纳。
1. 使用`mklink`命令建立链接，指向正确的位置。建议采纳。

## 三、安装及卸载

### 3.1 安装

如果在安装脚本中指定了带盘符的路径，则可在任意位置执行`scoop`命令。

否则，需首先进入到安装程序所在盘符，再执行`scoop`命令。

执行方式：

```bash
scoop install {path_to_bucket\}{app_name}.json
```

建议执行`scoop`之前先进入安装脚本所在路径，这样执行方式可简化为：

```bash
scoop install {app_name}.json
```

**注意**：脚本文件扩展名`.json`是必须的。

### 3.2 卸载

使用应用程序安装脚本文件名执行以下命令：

```bash
scoop uninstall {app_name}
```

**注意**：不可以添加脚本文件扩展名`.json`。

### 3.3 举例

对于`app_name`为`systemexplorer`的程序，其安装及卸载的命令如下，假设当前命令行位置位于脚本所在路径：

```bash
scoop install systemexplorer.json
scoop uninstall systemexplorer
```

## 四、软件列表

| 文件名 | 说明 | app_name |
| :--- | :---| :--- |
| System_Explorer_v7.1.0.5359.exe | 无法从官网下载的最新版SystemExplorer | systemexplorer |
| WXWorkLocal_2.5.40002.154.exe | 企业微信首发定制版，专网使用 |  bchdwechatwork |
| XYLinkClient-102.29.0.32835.exe | 小鱼易连首发定制版，专网使用 | bchdxylink |
