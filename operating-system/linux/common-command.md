# common linux command

```bash
grep -i -l -r -e 'arthinking.github.io' /Users/arthinking/Dev/informal-essay-github/* | xargs sed -i "" "s/arthinking.github.io/informal-essay/g"
```

[sed在mac下使用差异](http://www.th7.cn/system.mac/201411/77742.shtml
http://blog.csdn.net/ybygjy/article/details/42305313)


# scp

Linux的scp命令可以在Linux之间复制文件和目录：

> scp [可选参数] file_source file_target

可选参数：

* `-v`和大多数 linux 命令中的 -v 意思一样 , 用来显示进度 . 可以用来查看连接 , 认证 , 或是配置错误；* `-C`使能压缩选项；* `-P`选择端口 . 注意 -p 已经被 rcp 使用；* `-4`强行使用`IPV4`地址；* `-6`强行使用`IPV6`地址。

## 从本地复制到远程

### 复制文件：
```bash
# 指定上传文件夹
scp local_file remote_username@remote_ip:remote_folder # 指定上传后的文件名scp local_file remote_username@remote_ip:remote_file 
# 以下两个不指定远程服务器的登录名scp local_file remote_ip:remote_folder scp local_file remote_ip:remote_file 
```

### 复制目录

```bash
scp -r local_folder remote_username@remote_ip:remote_folder scp -r local_folder remote_ip:remote_folder 
```

会在`remote_folder`下面创建`local_folder`文件夹

## 从远程复制到本地

只要将从本地复制到远程的命令的后2个参数调换顺序即可。

## 例子
从远程下载文件到本地

```bash
scp root@182.92.6.82:/home/guitargg.gz ~/Downloads/
```

把本金文件传递到另一台主机上面：

```bash
scp ~/Downloads/guitargg.gz root@182.92.6.82:/home/
```

# tar
解压：
tar -zxvf /usr/local/test.tar.gz
tar -zxvf 压缩文件名.tar.gz -C /usr/local/maven

01-.tar格式
解包：[＊＊＊＊＊＊＊]$ tar xvf FileName.tar
打包：[＊＊＊＊＊＊＊]$ tar cvf FileName.tar DirName（注：tar是打包，不是压缩！）

02-.gz格式
解压1：[＊＊＊＊＊＊＊]$ gunzip FileName.gz
解压2：[＊＊＊＊＊＊＊]$ gzip -d FileName.gz
压 缩：[＊＊＊＊＊＊＊]$ gzip FileName

03-.tar.gz格式
解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tar.gz
压缩：[＊＊＊＊＊＊＊]$ tar zcvf FileName.tar.gz DirName

04-.bz2格式
解压1：[＊＊＊＊＊＊＊]$ bzip2 -d FileName.bz2
解压2：[＊＊＊＊＊＊＊]$ bunzip2 FileName.bz2
压 缩： [＊＊＊＊＊＊＊]$ bzip2 -z FileName

05-.tar.bz2格式
解压：[＊＊＊＊＊＊＊]$ tar jxvf FileName.tar.bz2
压缩：[＊＊＊＊＊＊＊]$ tar jcvf FileName.tar.bz2 DirName

06-.bz格式
解压1：[＊＊＊＊＊＊＊]$ bzip2 -d FileName.bz
解压2：[＊＊＊＊＊＊＊]$ bunzip2 FileName.bz

07-.tar.bz格式
解压：[＊＊＊＊＊＊＊]$ tar jxvf FileName.tar.bz

08-.Z格式
解压：[＊＊＊＊＊＊＊]$ uncompress FileName.Z
压缩：[＊＊＊＊＊＊＊]$ compress FileName

09-.tar.Z格式
解压：[＊＊＊＊＊＊＊]$ tar Zxvf FileName.tar.Z
压缩：[＊＊＊＊＊＊＊]$ tar Zcvf FileName.tar.Z DirName

10-.tgz格式
解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tgz

11-.tar.tgz格式
解压：[＊＊＊＊＊＊＊]$ tar zxvf FileName.tar.tgz
压缩：[＊＊＊＊＊＊＊]$ tar zcvf FileName.tar.tgz FileName

12-.zip格式
解压：[＊＊＊＊＊＊＊]$ unzip FileName.zip
压缩：[＊＊＊＊＊＊＊]$ zip FileName.zip DirName

13-.lha格式
解压：[＊＊＊＊＊＊＊]$ lha -e FileName.lha
压缩：[＊＊＊＊＊＊＊]$ lha -a FileName.lha FileName

14-.rar格式
解压：[＊＊＊＊＊＊＊]$ rar a FileName.rar
压缩：[＊＊＊＊＊＊＊]$ rar e FileName.rar     
rar请到：http://www.rarsoft.com/download.htm 下载！
解压后请将rar_static拷贝到/usr/bin目录（其他由$PATH环境变量
指定的目录也行）：[＊＊＊＊＊＊＊]$ cp rar_static /usr/bin/rar

# cp

复制并且覆盖目标文件的文件
cp -r -f /webapp/* /dist/

# lsof

查看端口占用

lsof -i tcp:port







