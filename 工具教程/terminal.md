## 个人终端配置流程

https://think.leftshadow.com/

***一、zsh+oh my zsh配置***

https://sysin.org/blog/linux-zsh/

**1、Zsh安装**

* ubuntu:

```shell
sudo apt install zsh
```

***2、 设置zsh为默认终端***

- 为 root 设置默认 shell

  ```shell
  sudo chsh -s /bin/zsh
  ```

- 为特定用户设置默认 shell

  ```shell
  sudo chsh -s /bin/zsh <username>    # <username> 替换为实际用户名
  ```

***3、 安装oh my zsh***

```shell
sh -c "$(wget -O- https://install.ohmyz.sh)"
```

***4、 修改主题***

```shell
vi ~/.zshrc

ZSH_THEME='target themes'
```

***5、添加插件***

```
plugins=([plugins...] zsh-syntax-highlighting)
```



***二、vim配置及使用***

***1、配置文件：***

```shell
.vimrc   ##默认配置文件
.vim/	 ##默认文件
```



***2、常用命令：***

![img](https://people.csail.mit.edu/vgod/vim/vim-cheat-sheet-en.png)

***三、tmux配置及使用***：

https://www.cnblogs.com/zuoruining/p/11074367.html

***1、配置文件***：

```shell
.tmux.config
.tmux/
```

```bash
# c+b 之后，输入 :
:source ~/.tmux.conf

# 成功之后左下角能看到
tmux reload ok
```

***2、常用命令：***

![](https://what30.qoding.us/wp-content/uploads/2020/12/adj85l9p49i91.png)

***3、插件配置***：

插件文件夹：

```shell
./tmux/plugins/
```

