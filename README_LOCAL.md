# Scoop安装本地应用程序说明

## 一、原因

由于有些程序不易下载，无法从官网上得到最新版本等原因，无法使用 `scoop`进行在线安装。因此建立本地安装脚本，直接安装在本地磁盘准备好的应用程序安装包。

## 二、安装程序位置

本地应用程序的安装脚本统一以 `lc_`为前缀，后面是软件名称。软件名称在本文中称为`app_name`。

各个应用程序的安装文件已经在安装脚本中指定为 `\Scoop\local_installers`。该路径默认执行 `scoop`时的位置与安装文件在相同磁盘分区。

如果安装文件不在该路径，有两个解决方案：

1. 修改各个安装脚本中的`url`，使其指向正确的安装文件。这样做比较麻烦，不建议采纳。
2. 使用`mklink`命令建立链接，指向正确的位置。**建议采纳**。

除路径外，安装文件的文件名，也要与脚本中一致。

## 三、安装及卸载

### 3.1 安装

如果在安装脚本中为安装文件指定了带盘符的路径，则可在任意位置执行 `scoop`命令。但为了每次安装不必锁定特定的分区，所以在安装脚本中没有指定盘符。

因此，执行安装或更新命令前，需进入到安装程序所在磁盘分区，再执行 `scoop`命令。

执行方式：

```bash
scoop install {path_to_bucket\}{app_name}.json
```

建议执行 `scoop`之前先进入安装脚本所在路径，假设`scoop`安装于`F:\Scoop`，执行如下命令进入脚本所在路径：

```bash
F:
cd \Scoop\buckets\ajqk
```

`ajqk`是添加当前`bucket`时起的名字。

此时就不必指定 `path_to_bucket，`执行方式可简化为：

```bash
scoop install {app_name}.json
```

**注意**：脚本文件扩展名 `.json`是必须的。

### 3.2 卸载

使用应用程序安装脚本文件名执行以下命令：

```bash
scoop uninstall {app_name}
```

**注意**：不可以添加脚本文件扩展名 `.json`。

### 3.3 举例

对于 `app_name`为 `systemexplorer`的程序，其安装及卸载的命令如下，假设当前命令行位置位于脚本所在路径：

```bash
# 安装
scoop install lc_systemexplorer.json

# 卸载
scoop uninstall lc_systemexplorer

# 更新
scoop update lc_systemexplorer.json
```

## 四、本地应用软件列表

| 文件名                          | 说明                         | app_name          |
| :------------------------------ | :--------------------------- | :---------------- |
| System_Explorer_v7.1.0.5359.exe | 官网最新版为7.0.0，已不再更新  | lc_systemexplorer |
| WXWorkLocal_2.5.40002.154.exe   | 企业微信首发定制版，专网使用 | lc_bchdwechatwork |
| XYLinkClient-102.29.0.32835.exe | 小鱼易连首发定制版，专网使用 | lc_bchdxylink     |
