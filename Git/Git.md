# Git

## 集中式 vs 分布式

> 集中式与分布式最大的区别在于是否有完整的本地版本库。 
---

1. 集中式以“中央服务器”为中心，从中央服务器拉取项目工作，并在完成工作后重新上传到中央服务器。 本地只保留部分版本内容。 当中央服务器出现状况，便无法继续工作。 当然可以通过定期备份服务器来作保障。 


2. 分布式服务器拥有完成的本地版本库，中央服务器只作为交流协作使用，因此哪怕中央服务器出现状况，仍然可以通过本地版本库恢复。


## 安装
```
sudo apt install git

git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

## 创建版本库
```
1. 选择一个作为版本库的文件夹
2. 输入 git init 命令

此时文件夹中会出现一个名为 .git 的隐藏文件夹，这就是版本库
```


```plantuml
@startmindmap
* linux
** fd
@endmindmap
```
