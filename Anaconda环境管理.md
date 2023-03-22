# Anaconda环境管理

## 创建虚拟环境
创建名为dev的虚拟环境：
`conda create --name dev`  
创建名为test的环境并安装python3.9：
`conda create --name test python=3.9`  
等价于`conda create -n test python=3.9`  

通过克隆创建环境：  
`conda create --name test39 --clone test_python39`  

## 删除虚拟环境
`conda remove -n test_python39 --all`  

## 查看虚拟环境清单
`conda env list`
`conda info --envs`  

## 进入、切换虚拟环境
Windows, macOS, Linux: `conda activate dev`(只适用conda 4.6以后版本)  
Windows: `activate dev`(兼容conda 4.6以前版本)  
macOS, Linux: `source activate dev`(兼容conda 4.6以前版本)  

## 从当前环境切到默认环境（base）
Windows: `activate`  
macOS, Linux: `source activate`  
退出当前环境：`deactivate`  

## 安装包（在虚拟环境）
指定环境安装  
`conda install -n dev numpy -i https://pypi.tuna.tsinghua.edu.cn/simple/`  
不指定环境，默认安装在活跃的环境  
查找软件包：`conda search python=3.9`  
安装软件：`conda install python=3.9.10`  
`pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple/`  
`pip install pandas -i https://pypi.tuna.tsinghua.edu.cn/simple/`  
`pip install scikit-learn -i https://pypi.tuna.tsinghua.edu.cn/simple/`  
`pip install TensorFlow -i https://pypi.tuna.tsinghua.edu.cn/simple/`  
`pip install opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple/`  

## 打开Jupyter
`jupyter notebook`  
