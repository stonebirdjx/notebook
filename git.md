[toc]

# github-gitee

添加ssl-key

```shell
ssh-keygen -t rsa -C "1245863260@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/tianqixin/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):    # 直接回车
Enter same passphrase again:                   # 直接回车
Your identification has been saved in /Users/tianqixin/.ssh/id_rsa.
Your public key has been saved in /Users/tianqixin/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:MDKVidPTDXIQoJwoqUmI4LBAsg5XByBlrOEzkxrwARI 429240967@qq.com
The key's randomart image is:
+---[RSA 3072]----+
|E*+.+=**oo       |
|%Oo+oo=o. .      |
|%**.o.o.         |
|OO.  o o         |
|+o+     S        |
|.                |
|                 |
|                 |
|                 |
+----[SHA256]-----+
```

然后在github gitee **SSH and GPG keys** 绑定

```shell
cd workpath
git init
git add .
git commit -m "git.md"
git remote add github git@github.com:StoneBridPathWeave/notebook.git
git remote add gitee git@gitee.com:hujixu/notebook.git
git remote -v  #查看仓库信息
git push [-f 强制] github master
git push [-f 强制] gitee master
```

