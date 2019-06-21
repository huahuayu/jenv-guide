# jenv-guide
jenv guide & troubleshoot

## jenv简介  
linux/mac OS下的命令行工具，功能是用来管理java的多版本  

## 官网
官网：https://www.jenv.be/  
github：https://github.com/jenv/jenv  

## 安装
mac安装方法  
``` bash
$ brew install jenv
```

## 初始化
将下面这行添加到bash_profile或者zprofile末尾，然后source一下    
```
eval "$(jenv init -)"
```

或者执行  
```
$ echo 'eval "$(jenv init -)"' >> ~/.bash_profile
```

## 查看jdk版本
``` bash
shiming@pro ➜  ~ jenv versions
* system (set by /Users/shiming/.jenv/version) # 没添加jdk版本前，应该只有这一行
  1.8
  1.8.0.212
  oracle64-1.8.0.212
```

## 添加jdk版本
首先要自己先安装jdk，比如mac有了java11后，想安装java8/9/10，通过brew  
``` bash
brew tap adoptopenjdk/openjdk

brew cask install adoptopenjdk8
brew cask install adoptopenjdk9
brew cask install adoptopenjdk10
```

然后使用`jenv add`添加java版本  
``` bash
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home
```

如果报错
``` bash
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_121.jdk/Contents/Home  
ln: /Users/reapor.yurnero/.jenv/versions/oracle64-1.8.0.121: No such file or directory 
```

那就是没做初始化，需要将`eval "$(jenv init -)"`加到bashrc或zshrc里面  

初始化后如果还是不行，检查一下，`~/.jenv/versions`文件夹是否存在，如果不存在自己新建一下`mkdir -p ~/.jenv/versions`  

添加完了再查看一下版本，看下有没有加上去

## 添加maven和export插件

```
shiming@pro ➜  ~ jenv enable-plugin maven
maven plugin activated
shiming@pro ➜  ~ jenv enable-plugin export
You may restart your session to activate jenv export plugin echo export plugin activated
```
参考：https://developer.bring.com/blog/configuring-jenv-the-right-way/

如果报错
```
jenv: no such command `enable-plugin'
```

执行一下`jenv doctor`命令，会检测问题，很可能是没有做初始化，需要将以下语句放入到zshrc中  
```
eval "$(jenv init -)"
```


## 版本设定
### 设定全局jdk版本  
``` bash
shiming@pro ➜  ~ jenv global 1.8
shiming@pro ➜  ~ javac -version
javac 1.8.0_212
```

### 设定本地jdk版本
先`cd`到目录，然后执行`jenv local {version}`则在此目录中使用指定的java版本    
``` bash
shiming@pro ➜  ~ cd ~/x
shiming@pro ➜  x jenv local 11
shiming@pro ➜  x javac -version
javac 11.0.1
shiming@pro ➜  x ls -al
total 8
drwxr-xr-x    3 shiming  staff    96 Jun 21 11:45 .
drwxr-xr-x+ 137 shiming  staff  4384 Jun 21 11:53 ..
-rw-r--r--    1 shiming  staff     3 Jun 21 11:53 .java-version  # local version就是由这个隐藏文件控制的
shiming@pro ➜  x cd ..
shiming@pro ➜  ~ javac -version
javac 1.8.0_212  # 全局还是java8
```
