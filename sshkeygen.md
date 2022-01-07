# ssh-keygen秘钥生成
## 默认秘钥生成
`ssh-keygen -t rsa -C "your_email@youremail.com"`
## 多个秘钥生成
- 模式1,使用不同文件名
`ssh-keygen -t rsa -C "YOUR_EMAIL@YOUREMAIL.COM" -f ~/.ssh/abc`  
- 模式2，使用不同文件夹默认文件名
` ssh-keygen -t rsa -C ‘m’ -f ~/.ssh/abc/id-rsa`
### 秘钥测试
`ssh -T git@github.com /会有成功提示`
### 检查ssh-key代理
`ssh-add -l`
### 清除代理
`ssh-add -D`
### 添加代理
```git
ssh-add ~/.ssh/id_rsa  
ssh-add ~/.ssh/abc
```
如果使用 `ssh-add ~/.ssh/id_rsa`的时候报如下错误:  
`Could not open a connection to your authentication agent.`  
则需要先运行一下 `ssh-agent bash` 命令后再执行 `ssh-add ...`等命令
## 配置~/.ssh/config 文件
该文件用于配置私钥对应的服务器  
```git
# 默认GitHub 使用(first@mail.com)  
Host github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa
 
# abc站点使用 (abc_email@mail.com)
Host github-abc
HostName github.com
User git
IdentityFile ~/.ssh/abc
```
使用方法  
`git remote add test git@github-abc:sadhus/test.git `
使用githun-abc来代替github，使用配置文件里面参数，主机名依旧是github，秘钥文件则使用单独的，从而实现两个秘钥分别对应  
或者直接使用abc 而不使用github-abc，都可以



