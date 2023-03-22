# Mac用VSCode搭建go开发环境

## 1、到官网下载go安装包，安装
（推荐）官网地址：https://golang.google.cn/
go中文网地址：https://studygolang.com/dl

## 2、打开终端并执行
打开go modules功能
`$ go env -w GO111MODULE=on`
打开代理
`$ go env -w GOPROXY=https://goproxy.cn,direct` # 七牛云

## 3、到官网下载VSCode安装包，安装
vscode国内镜像域名
https://vscode.cdn.azure.cn/

## 4、在VSCode安装go插件
4.1 打开VSCode左侧的【扩展】按钮，安装go插件
4.2 打开一个go程序编辑页，按command+shift+p组合键，输入选择：Go: Install/Update Tools，全选确认安装
4.3 如果上一步安装失败，在【扩展】安装Go Test Explorer插件再继续
