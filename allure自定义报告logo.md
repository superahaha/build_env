# allure自定义报告logo
## 找到allure安装目录和样式配置文件
/usr/local/Cellar/allure/2.20.1/libexec/plugins/custom-logo-plugin/static/styles.css
增加下面代码：
## logo图
.side-nav__brand {
  background: url('allure_logo.jpg') no-repeat left center !important;
  margin-left: 10px;
  height: 50px;
  background-size: contain !important;
}
## logo名
.side-nav__brand span{
  display: none;
}
.side-nav__brand:after{
  content: "IGC";
  margin-left: 20px;
}

## 启用自定义logo
/usr/local/Cellar/allure/2.20.1/libexec/config/allure.yml
增加一行：- custom-logo-plugin
