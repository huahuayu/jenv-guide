# jenv-guide
jenv guide, problem diagnosis

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

## 版本设定
### 全局jdk版本  
``` bash
shiming@pro ➜  ~ jenv global 1.8
shiming@pro ➜  ~ javac -version
javac 1.8.0_212
```

### 本地jdk版本
先`cd`到目录，然后执行`jenv local {version}`则在此目录使用指定的版本    
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

## troubleshoot  
### jenv doctor
如果使用jenv的过程中遇到报错，先使用`jenv doctor`检查一下，有问题会指出并给出解决建议    
```
shiming@pro ➜  ~ jenv doctor
[OK]	No JAVA_HOME set
[OK]	Java binaries in path are jenv shims
[OK]	Jenv is correctly loaded
```

### jenv: no such command `enable-plugin'


```
shiming@pro ➜  ~ jenv enable-plugin maven
jenv: no such command `enable-plugin'
shiming@pro ➜  ~ jenv doctor
[OK]	No JAVA_HOME set
[ERROR]	Java binary in path is not in the jenv shims.
[ERROR]	Please check your path, or try using /path/to/java/home is not a valid path to java installation.
	PATH : /usr/local/Cellar/jenv/0.5.2/libexec/libexec:/Users/shiming/Downloads/sonarqube-6.7.6/bin/macosx-universal-64:/Users/shiming/Downloads/sonar-scanner-3.3.0.1492-macosx/bin:/Users/shiming/fabric-samples/bin:/usr/local/Cellar/grafana/5.4.3/bin:/usr/local/Cellar/python/3.7/bin:/usr/local/Cellar/python/3.7.1/bin:/usr/local/mysql/bin:/Users/shiming/Nutstore/5-goland/bin:/bin/:/Users/shiming/Downloads/sonarqube-6.7.6/bin/macosx-universal-64:/Users/shiming/Downloads/sonar-scanner-3.3.0.1492-macosx/bin:/Users/shiming/fabric-samples/bin:/Users/shiming/.nvm/versions/node/v8.15.1/bin:/usr/local/Cellar/grafana/5.4.3/bin:/usr/local/Cellar/python/3.7/bin:/usr/local/Cellar/python/3.7.1/bin:/usr/local/mysql/bin:/Users/shiming/Nutstore/5-goland/bin:/bin/:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin
[ERROR]	Jenv is not loaded in your zsh
[ERROR]	To fix : 	cat eval "$(jenv init -)" >> /Users/shiming/.zshrc
```
