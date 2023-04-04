# VSCode搭建go开发环境

## 1、到官网下载并安装Go
 - （推荐）官网地址：https://golang.google.cn/
 - go中文网地址：https://studygolang.com/dl


## 2、打开go modules功能
打开终端并执行
 > `$ go env -w GO111MODULE=on`


## 3、添加代理，提升插件下载/更新速度
### 【goproxy.io】
- Bash (Linux or macOS)
  配置 GOPROXY 环境变量
  > `export GOPROXY=https://proxy.golang.com.cn,direct`
  还可以设置不走 proxy 的私有仓库或组，多个用逗号相隔（可选）
  > `export GOPRIVATE=git.mycompany.com,github.com/my/private`

- PowerShell (Windows)
  配置 GOPROXY 环境变量
  > `$env:GOPROXY = "https://proxy.golang.com.cn,direct"`
  还可以设置不走 proxy 的私有仓库或组，多个用逗号相隔（可选）
  > `$env:GOPRIVATE = "git.mycompany.com,github.com/my/private"`

### 【七牛云】
- 在Go 1.13 及以上（推荐），打开终端并执行：
 > `go env -w GOPROXY=https://goproxy.cn,direct`

- 在macOS或Linux打开终端并执行：
 > `export GO111MODULE=on`  
 > `export GOPROXY=https://goproxy.cn`

- 在Windows打开 PowerShell 并执行：
 > `$env:GO111MODULE = "on"`  
 > `$env:GOPROXY = "https://goproxy.cn"`

- 取消代理
 > `$ go env -u GOPROXY`

- 查看GO的配置
 > `$ go env`  
  //以JSON格式输出  
 > `$ go env -json`


## 4、到官网下载VSCode安装包，安装
vscode国内镜像域名  
https://vscode.cdn.azure.cn/


## 5、在VSCode安装go插件
 - 5.1 打开VSCode左侧的【扩展】按钮，安装go插件
 - 5.2 打开一个go程序编辑页，按command+shift+p组合键，输入选择：Go: Install/Update Tools，全选确认安装
 - 5.3 如果上一步安装失败，在【扩展】安装Go Test Explorer插件再继续


## 6、安装go开发工具包
 - Windows平台按下快捷键：【 Ctrl+Shift+P 】，Mac平台按【 Command+Shift+P 】
 - 输入框中输入Go:Install/Update Tools并选中，然后全选弹出的列表，确认即可安装
 
## 7、升级go
Linux系统
 - 7.1先移除旧版本再安装
   > $ rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.2.linux-amd64.tar.gz
 - 7.2添加/usr/local/go/bin到环境变量(如.bash_profile)
   > export PATH=$PATH:/usr/local/go/bin

Mac系统
 - 下载新版本安装包，安装时会自动检测旧版本，覆盖安装即可
 
