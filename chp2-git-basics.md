# Git 起步

如果你想通过阅读一章就去使用 Git ，本章就是。本章涵盖了每一个你在日常工作中大量
使用的基础命令。到本章末尾，你将能够：配置并初始化一个仓库，添加、删除追踪文
件，以及 stage 和 commmit 变化。同时本章也会介绍如何取忽略文件以及按模式忽略文
件，如和简单快速地修复错误，如何查看项目的历史，以及如何查看不同 commit 之间的
变化，以及如何从远端仓库 pull 和 push 代码。

## 获取一个仓库

获取一个 Git 项目主要有两种方法。 一是将已有的项目或者目录导入到 Git 。二是从其
他服务器上 clone 一个 现存的 Git 仓库。

### 在目录中初始化一个仓库

如果你想将一个现存的项目交由 Git 管理，你需要跳转到项目的目录下，输入：

```
$ git init

```

该命令创建一个名为 .git 的子文件夹。 此文件夹包含一个 Git 骨架，所有仓库相关信
息都会存储在这里。到目前为止，项目中的所有文件都还没被追踪起来。（参见11章详细
介绍你刚刚创建的 .git 文件夹到底都办好了什么文件）。

如果你想将现有的文件版本控制起来（空文件夹除外）， 你需要将文件都追踪起来，然后
做一个初始化的提交。你可以用 `git add` 命令指定你需要追踪的文件， 然后通过
`git commit` 去提交。

```
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

我们几分钟后再来详细说明这些命令的含义。到目前为止，你已经有了一个 Git 仓库，追
踪了一些文件，做了一个初始化的提交。

### clone 一个现存的仓库

如果你想获取一个现存的 Git 仓库 —— 举例来说，一个你想贡献的项目 —— 你需要的命令
是 `git clone` 。如果你熟悉其他 VCS 系统，如 Subversion, 你会注意到命令是
`clone` 而不是 `checkout` 。这是一个重要的不同之处 —— 不仅仅是只复制工作相关的
内容， Git 基本上是对服务器的全量拷贝。项目的每一个版本，每一个文件的历史信息
默认都会被 `git clone` 命令拷贝回来。事实上，如果你的服务器磁盘损坏了，你可以使
用任何一个客户端的任何一个拷贝来服务器到 clone 时的状态（你可能会丢失一些服务端
的钩子等，但是所有版本控制的代码都会保存下来 —— 第四章有更多细节描述）。

clone 一个项目用 `git clone [url]` 。举例来说，如果你想 clone 一个 可通过链接访
问的 Git 仓库 libgit2, 你可以按如下操作：

```
$ git clone https://gihub.com/libgit2/libgit2
```

此步骤创建了一个 libgit2 的文件夹，并初始化了一个 .git 子文件夹，同时拉下了仓库
的所有数据， 并 check out 了一个最近版本的工作拷贝（working copy）。如果你查看
新创建的 libgit2 文件夹，你会发现有所的项目文件都在哪里，等待这你的使用或编辑。
如果不想用 libgit2这个名字，可以通过制定另外一个命令行选项：

```
git clone https://github.com/libgit2/libgit2 mylibgit
```

上面的命令和前一个命令行为一致，除了目标路径名叫做 mylibgit 。

Git 有多种不同的传输协议。前面的例子是 https 协议， 但是你也会看到 `git://` 或
者 `user@server:path/to/repo.git`, 它们使用的 SSH 传输协议。第4章会介绍服务器配
置 Git 访问的所有的可项，同时会介绍他们的优点和缺点。

### 在仓库中记录变化

你已经获取了一个真实的 Git 仓库，并且 check out 了一个工作项目拷贝。当项目到达
某个阶段的时候, 你需要做一些修改，并将修改按快照提交到仓库里。

牢记，你的工作目录的文件只有两种状态：追踪(checked)或未追踪(untracked）。追踪
的文件是最近的一次快照。他们可以被修改，取消修改或者stage起来。未追踪的文件是
处此之外的所有文件--工作路径下任何不在最近一次快照,或暂存区的所有文件。当你新
clone 一个项目下来，所有的文件处于追踪未修改状态，因为你仅仅签出了工作目录，但
还未做任何修改。

一旦你修改一些文件，Git 会认为它们是被修改的状态，因为相对于上一次提交，文件已
发生了变化。你暂存修改，然后提交这些暂存的文件，依此循环。


图片2-1： 文件的生命周期

### 查看文件的状态

查看什么文件在什么状体最好的工具是 `git status` 命令。如果你在 clone 后马上运行
这个命令，你可能会看到如下的提示：

```
$ git status
On branch master
noting to commit, working directory clean
```

这意味着你拥有一个干净的工作目录——换句话说，没有追踪、修改的文件。Git 同时也没
有未追踪的文件，否则会显示在提示里。最后， 该命令提示你在哪个 branch 上，并且提
示你本地与服务器段并没有分离。目前所有的分支都是默认的 master, 不必担心更多细
节。分支和引用（references）在第三章会详细讨论。

我们假设你要添加一个新的文件到项目，比如一个简单的 README 文件。 如果文件此文件
之前不存在，运行 git status ，你会看到未追踪的文件显示如下：

```
$ echo 'My Project' > README
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
    README

nothing added to commit but untracked files present (use "git add" to track)
```

你可以看到你新建的 README 文件处于未追踪状态，在 “Untracked files” 下可以看到
输出结果。未追踪通常意味着 Git 认为这个文件在上一个快照或 commit 中不存在；Git
不会主动包含文件到快照内，这需要你显示的添加进来。这样做的目的是避免不小心将非
必要的文件添加进来，如生成的二进制文件等。如果你想将 README 添加进来，让我们开始
追踪此文件。

### 追踪（tracking）新的文件

为了追踪一个新文件，你需要使用 git add 命令。添加一个 README 文件， 运行下面的
命令：

```
$ git add README
```

如果再次运行状态查看命令，你会看到 README 文件已经被追踪，并且处于暂存待提交的
状态：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <FILE>..." to unstage)

  new file: README
```

你可以看到文件被暂存到所有待提交的头部。如果在此时提交，最后一次运行 git add
命令的改动会进入到快照历史。你可能会想起不久前运行的 git init 命令，然后运行了
git add (files) —— 那就是追踪目录里的文件。git add 接收参数为文件路径名或目录，
如果为目录，git 会递归添加目录下的所有文件以及文件夹。

### 暂存修改的文件

让我们修改一个已经被暂存的文件。如果你改变一个之前已经被暂存的名为
benchmarks.rb 的文件，然后再次运行 git status, 你将会看到如下的结果：

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <FILE>..." to unstage)

  new file: README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified: benchmarks.rb
```

benchmarks.rb 文件显示在改变但是还没有暂存起来做commit的区域 —— 表明此文件是一
个已被暂存过的文件，但是被修改后没有再次暂存起来。使用 git add 命令可以再次暂
存。git add 命令是一个多用途的命令 —— 你可以用来追踪新的文件，可以暂存文件，也
可以做其他的事情，比如标记文件冲突已经被解决。把它看做“把这些内容放到下一个
commit” 可能会比“把这些文件添加到项目中”更容易帮助理解一些。然我们运行 git add
把 benchmarks.rb 暂存起来，然后再次运行 git status:

```
$ git add benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  new file: README
  modified: benchmarks.rb
```

两个文件都被暂存起来了，并且都会出现在下一个 commit 中。 在此时，我们假设你突然
想到要在提交之前对 benchmarks.rb 在做点小改动。你打开它，然后做修改。改完准备提
交。这个时候再运行 git status：

```
$ vim benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

  new file: README
  modified: benchmarks.rb

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

  modified: benchmarks.rb
```

发生了什么？现在 benchmarks.rb 即被暂存，又没有被暂存。怎么可能？最终的结论是
Git 确实会在运行 git add 命令时将文件暂存。如果此时 commit，最终进入commmit的
benchmarks.rb的版本是最近一次运行 git add 命令时的版本，而不是运行git commit 工
作目录下的版本。如果你在 git add 之后修改了文件，需要再次运行 git add 将最新版
本存入暂存区。

```
$ git benchmarks.rb
$ git status
On branch master
Changes to be committed:

  (use "git reset HEAD <file>..." to unstage)

  new file: README
  modified: benchmarks.rb
```

### git status 简写

尽管 git status 的输出非常详尽，但是也显得啰嗦。 Git 提供了一个输出简写状态的选
项，可以以简写的方式查看你的修改。运行 git status -s 或者 git status --short,
你会看到一个相对来说非常简洁的输出：

```
$ git status -s
 M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```

未追踪的新文件已 ?? 开头，新添加到暂存区的已 A 开头，修改的以M开头等等。命令输
出有两列，左列表示是否在缓存区，右列表示是否修改。以 README 文件举例，此文件不
在暂存区但是已经修改，而文件 lib/simpligit.rb 已修改并且在暂存区。Rakiefile在
暂存区，已被修改，并且被再次修改，所以存在即在暂存区又不在暂存区的修改。

### 忽略文件

通常你会有一类文件并不想 Git 自动添加，甚至不想看到出现在未追踪区域。这些通常是
自动申城的文件如日志文件或者被构建系统生成的文件。在这种情况下，你可以创建一个
文件.gitingore,列出所有的模式来使 Git 忽略相关文件。看一个例子：

```
$ cat .giignore
*.[oa]
*~
```

第一行告诉 Git 忽略所有以 .o 或者 .a 结尾的文件 —— 可能是由构建系统产生的对象或
者 archive 文件。 第二行告诉 Git 忽略所有一波浪线结尾的文件，Emacs等 文本编辑器
以~线作为临时文件。你也可以添加 log ，tmp, pi 目录，自动生成的文档目录等。在项
开始前目配置 .gitignore 是一个好的想法，这样就不容易提交一些你根本不想提交的文
件到 Git 仓库中。

.gitignore中的规则满足一下模式：

* 空行或者以#号开头的行会被忽略
* 可使用标准的 glob 模式
* 以 / 为结尾表示目录
* 以 ! 开头表示取反

Glob 模式是 shell 使用的 正则表达式的简化版。 一个 * 号表示匹配 0 或多个字符；
[abc] 匹配任何一个方括号内的字符（在这个例子a，b , 或者c）; 一个 ？号匹配一个字
符。[0-9]匹配 0 到 9 之间所有的数字。你可以使用两个 * 号来匹配嵌套路径 `a/**/z`
会匹配 a/z, a/b/z, a/b/c/z, 等等。

下面是另外一个.gitignore的例子：

```
# a comment - this is ignored
*.a # no .a files
!lib.a # but do track lib.a, even though you're ignoring .a files above
/TODO # only ignore the root TODO file, not subdir/TODO
build/ # ignore all files in the build/ directory
doc/*.txt # ignore doc/notes.txt, but not doc/server/arch.txt
```

> 提示: Github 维护了一个详尽的 .gitignore示例，示例来自于几十个项目或者语言，
可供你在初始化项目时参考。链接
[https://github.com/github/gitignore](https://github.com/github/gitignore)

### 查看你的暂存文件和未暂存文件
