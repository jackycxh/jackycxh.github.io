# Emacs Tramp Mode


## Emacs Tramp Mode

日常用Emacs的同学可能很多都是开机打开Emacs就不关的，或者干脆就是用Emacs的Daemon模式，每次要用就开一个`emacsclient`。这时经常会碰到的一个问题就是提权编辑。编辑配置文件时经常要用root权限。一般我们会用sudo或su提权，比始`sudo emacs /etc/...`，这会需要另外打开一个Emacs进程。

Emacs 里面默认带有一个包TRAMP (Transparent Remote Access, Multiple Protocols)允许我们在emacs里面进行提权，甚至可以打开其它机器里面的文件，不需要再另开Emacs。

下面是具体的用法。

* 用root用户打开本地文件(以`/etc/host`/为例)。

  ```bash
  # 使用sudo
  C-x C-f /sudo::/etc/host
  # 或使用su
  C-x C-f /su::/etc/host
  ```

  {{< admonition note >}}

  必需要有两个冒号

  {{< /admonition >}}

* 用其它用户`otheruser`打开本地文件`/home/otheruser/otherfile`。

  ```bash
  # 使用su
  C-x C-f /su:otheruser@localhost:/home/otheruser/otherfile
  # 或使用sudo
  C-x C-f /sudo:otheruser@localhost:/home/otheruser/otherfile
  ```

* 通过SSH打开其它机器`remotehost`上的文件`~/.profile`。

  ```bash
  C-x C-f /ssh:remoteuser@remotehost:~/.profile
  # 此处可用绝对路径或相对路径（相对于ssh登录后的默认目录，一般为用户主目录）
  ```

### 偷懒用法

可能有同学就会问了，一般情况我只需要用`sudo`，或者相反，经常要用`ssh`到某一台特定的服务器上，有没有什么可以偷懒的办法？有的！Tramp支持设置默认方法`tramp-default-method`，默认用户名`tramp-default-user`，默认机器名`tramp-default-host`。初始设置中默认方法为`scp`, 默认用户名为空，默认机器名为本机名。下面为设置默认值的办法。把下面这段加到`.emacs`或`~/.emacs.d/init.el`中。支持的method参看[Inline methods](https://www.gnu.org/software/emacs/manual/html_node/tramp/Inline-methods.html#Inline-methods) 和[External methods](https://www.gnu.org/software/emacs/manual/html_node/tramp/External-methods.html#External-methods) 。

```lisp
;; 设置默认方法为ssh. 
(setq tramp-default-method "ssh")
;; 设置默认用户名为user1.
(setq tramp-default-user "user1")
;;设置默认主机名为remotehost.
(setq tramp-default-host "remotehost")
```

设置完后就可以用缩写了。
* 省略方法
  ```bash
  C-x C-f /remoteuser@remotehost:remotefile
  ```
* 省略用户名
  ```bash
  C-x C-f /ssh:remotehost:remotefile
  ```
* 省略方法和用户名
  ```bash
  C-x C-f /remotehost:remotefile
  ```
* 省略用户名和主机名（其实上文中sudo的就是这个）

  ```bash
  C-x C-f /ssh::remotefile
  ```
* 全部省略
  ```bash
  C-x C-f /-::remotefile
  ```

### 高级用法

在Emacs 24.3以及之后的版本中，Tramp支持多重连接，每个连接之间通过`|`相连。

* 通过跳板机连接到另一台服务器
  ```bash
  C-x C-f /ssh:user1@jumphost|ssh:username@remotehost:remotefile
  ```
* 在另一台机器中用sudo提权编辑
  ```bash
  C-x C-f /ssh:remoteuser@remotehost|sudo:remotehost:remotefile
  ```
  {{< admonition note >}}

  此处sudo后面一定要有远端主机名

  {{< /admonition >}}

