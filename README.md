[TOC]

# 第一章 起步

## 一、关于版本控制

版本控制系统的演化：

1. 本地版本控制系统
2. 集中化版本控制系统
3. 分布式版本控制系统

## 二、Git简史

## 三、Git基础  

1. 直接记录快照，而非差异比较
2. 近乎所有操作都是本地执行
3. Git保证完整性
   `Git数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。`
4. Git一般只添加数据
5. 三种状态
   - Git有三种状态，已被Git跟踪的文件一定处于其中之一：
     - **已提交（committed）**：数据已经安全的保存在本地数据库中。
     - **已修改（modified）**：修改了文件，但还没保存到数据库中。
     - **已暂存（staged**）：对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。
   - 由此引入Git项目的三个工作区域的概念：
     - **Git仓库目录（Repository）**：即`.git`隐藏目录，是Git用来保存项目的元数据和对象数据库的地方。这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。
     - **工作目录（Working Directory）**：对项目的某个版本独立提取出来的内容。这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。
     - **暂存区域（Staging Area）**：是一个文件，保存了`下次`将提交的文件列表信息，一般在 Git 仓库目录（.git目录）中。
   - 基本的 Git 工作流程如下：
     1. 在工作目录中修改文件。
     2. 暂存文件，将文件的快照放入暂存区域。
     3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。
   - 三态转换：
     - 如果 Git 目录中保存着的特定版本文件，就属于已提交状态。 
     - 如果作了修改并已放入暂存区域，就属于已暂存状态。
     - 如果自上次取出后，作了修改但还没有放到暂存区域，就是已修改状态。 

## 四、命令行

## 五、安装Git

## 六、初次运行Git前的配置

1. Git的配置文件

   - Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：
     1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 `--system` 选项的 `git config` 时，它会从此文件读写配置变量。
     2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 可以传递 `--global`选项让 Git 读写此文件。
     3. 当前使用仓库的 Git 目录中的 `config` 文件（就是 `.git/config` ）：针对该仓库的配置。
   - 每一个级别覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。
   - 在 Windows 系统中，Git 会查找 \$HOME 目录下（一般情况下是 C:\Users\\$USER ）的.gitconfig 文件。 Git 同样也会寻找 /etc/gitconfig 文件，但只限于 MSys 的根目录下，即安装 Git 时所选的目标位置。

2. 用户信息
   当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：

   ```
   $ git config --global user.name "John Doe"
   $ git config --global user.email johndoe@example.com
   ```

   再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

3. 文本编辑器
   既然用户信息已经设置完毕，你可以配置默认文本编辑器了，当 Git 需要你输入信息时会调用它：

   ```
   $ git config --global core.editor emacs
   ```

4. 检查配置信息

   - 如果想要检查你的配置，可以使用 `git config --list` 命令来列出所有 Git 当时能找到的配置：

     ```
     $ git config --list
     user.name=John Doe
     user.email=johndoe@example.com
     color.status=auto
     color.branch=auto
     color.interactive=auto
     color.diff=auto
     ...
     ```

   - 你可以通过输入 `git config <key>` ，来检查 Git 的某一项配置：

     ```
     $ git config user.name
     John Doe
     ```


## 七、获取帮助

若你使用 Git 时需要获取帮助，有三种方法可以找到 Git 命令的使用手册：

```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```



# 第二章 Git基础

## 一、获取Git仓库

> 有两种取得 Git 项目仓库的方法。 第一种是在现有项目或目录下导入所有文件到 Git 中； 第二种是从一个服务器克隆一个现有的 Git 仓库。

1. 在现有目录中初始化仓库

   - 如果你打算使用 Git 来对现有的项目进行管理，你只需要进入该项目目录并输入：

     ```
     $ git init
     ```

     该命令将创建一个名为 `.git` 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。

   - 如果你是在一个已经存在文件的文件夹（而不是空文件夹）中初始化 Git 仓库来进行版本控制的话，你应该开始跟踪这些文件并提交。 你可通过 `git add` 命令来实现对指定文件的跟踪，然后执行 `git commit` 提交：

     ```
     $ git add *.c
     $ git add LICENSE
     $ git commit -m 'initial project version'
     ```

2. 克隆现有的仓库
   如果你想获得一份已经存在了的 Git 仓库的拷贝，比如说，你想为某个开源项目贡献自己的一份力，这时就要用到 `git clone` 命令。Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。当执行 `git clone` 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。克隆仓库的命令格式是 `git clone [url]` ，例如：

   ```
   $ git clone https://github.com/libgit2/libgit2
   ```

## 二、记录每次更新到仓库

> 工作目录下的每一个文件都不外乎这两种状态：`已跟踪或未跟踪`。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于`未修改，已修改或已放入暂存区`。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。
>
> 编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。
>
> 因此，Git工作目录中的文件有且只有四种状态：
>
> - 未跟踪（Untracked）
> - 未修改（Unmodified）
> - 已修改（Modified）
> - 暂存（Staged）

1. 检查当前文件状态
   要查看哪些文件处于什么状态，可以用 `git status` 命令，例如：

   ```
   $ git status
   On branch master
   nothing to commit, working directory clean
   ```

2. 跟踪新文件
   使用命令 `git add` 开始跟踪一个文件，例如：

   ```
   $ git add README
   ```

    `git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

3. 暂存已修改文件
   同样需要使用命令`git add`。这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为`添加内容到下一次提交中`而不是将一个文件添加到项目中要更加合适。例如：

   ```
   $ git add CONTRIBUTING.md
   ```

4. 状态简览
   使用 `git status -s` 命令或`git status --short` 命令，你将得到一种更为紧凑的格式输出。 运行 `git status -s` ，状态报告输出如下：

   ```
   $ git status -s
    M README
   MM Rakefile
   A lib/git.rb
   M lib/simplegit.rb
   ?? LICENSE.txt
   ```

   新添加的未跟踪文件前面有 `??` 标记，新添加到暂存区中的文件前面有 `A` 标记，修改过的文件前面有 `M` 标记。 你可能注意到了 `M` 有两个可以出现的位置，出现在右边的 `M` 表示该文件被修改了但是还没放入暂存区，出现在靠左边的 `M` 表示该文件被修改了并放入了暂存区。 

   例如，上面的状态报告显示： `README` 文件在工作区被修改了但是还没有将修改后的文件放入暂存区, `lib/simplegit.rb` 文件被修改了并将修改后的文件放入了暂存区。 而`Rakefile` 在工作区被修改并提交到暂存区后又在工作区中被修改了，所以在暂存区和工作区都有该文件被修改了的记录。

5. 忽略文件
   一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件模式。其格式规范见`P36`。

6. 查看已暂存和未暂存的修改
   如果 `git status` 命令的输出对于你来说过于模糊，你想知道具体修改了什么地方，可以使用`git diff` 命令，它将通过文件补丁的格式显示具体哪些行发生了改变。

   - `git diff` ：比较的是**工作目录中当前文件和暂存区域快照之间的差异**；显示的是**尚未暂存的改动**，而不是自上次提交以来所做的所有改动。
   - `git diff --staged`：比较的是**暂存区域当前快照和上次快照之间的差异**；显示的是**已暂存的的内容**。

7. 提交更新

   - 每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit` ：

     ```
     $ git commit
     ```

     这种方式会启动文本编辑器以便输入本次提交的说明。

   - 可以在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行，如下所示：

     ```
     $ git commit -m "Story 182: Fix benchmarks for speed"
     [master 463dc4f] Story 182: Fix benchmarks for speed
     2 files changed, 2 insertions(+)
     create mode 100644 README
     ```

   - 请记住，提交时记录的是**放在暂存区域的快照**。 任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。 **每一次运行提交操作，都是对你项目作一次快照**，以后可以回到这个状态，或者进行比较。

8. 跳过使用暂存区域
   尽管使用暂存区域的方式可以精心准备要提交的细节，但有时候这么做略显繁琐。 Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就会自动把所有**已经跟踪过的**文件暂存起来一并提交，从而跳过 `git add` 步骤。

9. 移除文件

   - 要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。

   - 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f` （译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。

   - 另外一种情况是，我们想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中。 换句话说，你想让文件保留在磁盘，但是并不想让 Git 继续跟踪。 当你忘记添加 `.gitignore` 文件，不小心把一个很大的日志文件或一堆 `.a` 这样的编译生成文件添加到暂存区时，这一做法尤其有用。 为达到这一目的，使用 `--cached` 选项：

     ```
     $ git rm --cached README
     ```

   - `git rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式。

10. 移动文件

  - 使用`git mv`命令可以在Git中对文件改名，可以这么做：

    ```
    $ git mv file_from file_to
    ```

  - 运行 `git mv` 就相当于运行了下面三条命令：

    ```
    $ mv README.md README
    $ git rm README.md
    $ git add README
    ```

## 三、查看提交历史

1. 在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。完成这个任务最简单而又有效的工具是 `git log` 命令。默认不用任何参数的话， `git log` 会按提交时间列出所有的更新，最近的更新排在最上面。正如你所看到的，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

2. `git log`命令的常用选项

   - 一个常用的选项是 `-p` ，用来显示每次提交的内容差异。 你也可以加上 `-2` 来仅显示最近两次提交：

     ```
     $ git log -p -2
     commit 9e3fe3bd62f82ca589639ed4f469fe4285d09b6a
     Author: sepiggy <jms1209@qq.com>
     Date:   Thu Mar 30 13:38:41 2017 +0800

         second commit

     diff --git a/test.txt b/test.txt
     index 3a14fb5..47b10e5 100644
     --- a/test.txt
     +++ b/test.txt
     @@ -1 +1,2 @@
      I am a test file
     +This is another line

     commit 2fe7188e2d260f27c7f7b7ac2bd60addf3dc3287
     Author: sepiggy <jms1209@qq.com>
     Date:   Thu Mar 30 13:37:30 2017 +0800

         initial commit

     diff --git a/test.txt b/test.txt
     new file mode 100644
     index 0000000..3a14fb5
     --- /dev/null
     +++ b/test.txt
     @@ -0,0 +1 @@
     +I am a test file

     ```

     该选项除了显示基本信息之外，还在附带了每次 commit 的变化。 **当进行代码审查，或者快速浏览某个搭档提交的 commit 所带来的变化的时候**，这个参数就非常有用了。

   - 如果你想看到每次提交的简略的统计信息，你可以使用 `--stat` 选项：

     ```
     $ git log --stat
     commit 9e3fe3bd62f82ca589639ed4f469fe4285d09b6a
     Author: sepiggy <jms1209@qq.com>
     Date:   Thu Mar 30 13:38:41 2017 +0800

         second commit

      test.txt | 1 +
      1 file changed, 1 insertion(+)

     commit 2fe7188e2d260f27c7f7b7ac2bd60addf3dc3287
     Author: sepiggy <jms1209@qq.com>
     Date:   Thu Mar 30 13:37:30 2017 +0800

         initial commit

      test.txt | 1 +
      1 file changed, 1 insertion(+)
     ```

     正如你所看到的， `--stat` 选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。

   - 另外一个常用的选项是 `--pretty` 。 这个选项可以指定使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如用 `oneline` 将每个提交放在一行显示，查看的提交数很大时非常有用。 另外还有 `short` ， `full` 和 `fuller` 可以用：

     ```
     $ git log --pretty=oneline
     ca82a6dff817ec66f44342007202690a93763949 changed the version number
     085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
     a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
     ```

     `git log --pretty=format` 常用的选项列出了常用的格式占位符写法及其代表的意义，见`p47`。

   -  `git log` 的常用选项列出了我们目前涉及到的和没涉及到的选项，以及它们是如何影响 log 命令的输出的，见`p48`。

3. 限制输出长度
   除了定制输出格式的选项之外， `git log` 还有许多非常实用的限制输出长度的选项，也就是只输出部分提交信息，见`p50`。

## 四、撤销操作

1. 有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交：

   ```
   $ git commit --amend
   ```

   这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。

   例如，你提交后发现忘记了暂存某些需要的修改，可以像下面这样操作：

   ```
   $ git commit -m 'initial commit'
   $ git add forgotten_file
   $ git commit --amend
   ```

   最终你只会有一个提交 - 第二次提交将代替第一次提交的结果。

2. 取消暂存的文件
   已经修改了两个文件并且想要将它们作为两次独立的修改提交，但是却意外地输入了 `git add *` 暂存了它们两个。 如何只取消暂存两个中的一个呢？ `git status` 命令提示了你：

   ```
   $ git add *
   $ git status
   On branch master
   Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)

   	renamed: README.md -> README
   	modified: CONTRIBUTING.md
   ```

   根据提示，可以使用`git reset HEAD <file>...`来取消暂存。 所以，我们可以这样来取消暂存 `CONTRIBUTING.md` 文件：

   ```
   $ git reset HEAD CONTRIBUTING.md
   Unstaged changes after reset:
   M CONTRIBUTING.md
   $ git status
   On branch master
   Changes to be committed:
   	(use "git reset HEAD <file>..." to unstage)
   	renamed: README.md -> README
   Changes not staged for commit:
   	(use "git add <file>..." to update what will be committed)
   	(use "git checkout -- <file>..." to discard changes in working directory)
   	modified: CONTRIBUTING.md
   ```

   `CONTRIBUTING.md` 文件已经是修改未暂存的状态了。

3. 撤消对文件的修改
   如果你并不想保留对 `CONTRIBUTING.md` 文件的修改怎么办？ 你该如何方便地撤消修改 - 将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）？ 幸运的是， `git status` 也告诉了你应该如何做。 在最后一个例子中，未暂存区域是这样：

   ```
   Changes not staged for commit:
   (use "git add <file>..." to update what will be committed)
   (use "git checkout -- <file>..." to discard changes in working directory)
   	modified: CONTRIBUTING.md
   ```

   根据提示，可以使用`git checkout -- <file>...`来撤消对文件的修改，让我们来按照提示执行：

   ```
   $ git checkout -- CONTRIBUTING.md
   $ git status
   On branch master
   Changes to be committed:
   (use "git reset HEAD <file>..." to unstage)
   	renamed: README.md -> README
   ```

   可以看到那些修改已经被撤消了。

4. 记住，在 Git 中任何已提交的东西几乎总是可以恢复的。甚至那些被删除的分支中的提交或使用 `--amend` 选项覆盖的提交也可以恢复。 然而，任何你未提交的东西丢失后很可能再也找不到了。

## 五、远程仓库的使用

1. 查看远程仓库
   如果想查看你已经配置的远程仓库服务器，可以运行 `git remote` 命令。 它会列出你指定的每一个远程服务器的简写。 如果你已经克隆了自己的仓库，那么至少应该能看到 `origin` - 这是Git 给你克隆的仓库服务器的默认名字。
   你也可以指定选项 `-v` ，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL：

   ```
   $ git remote -v
   origin https://github.com/schacon/ticgit (fetch)
   origin https://github.com/schacon/ticgit (push)
   ```

2. 添加远程仓库
   运行 `git remote add <shortname> <url>` 添加一个新的远程 Git 仓库，同时指定一个你可以轻松引用的简写：

   ```
   $ git remote
   origin
   $ git remote add pb https://github.com/paulboone/ticgit
   $ git remote -v
   origin https://github.com/schacon/ticgit (fetch)
   origin https://github.com/schacon/ticgit (push)
   pb https://github.com/paulboone/ticgit (fetch)
   pb https://github.com/paulboone/ticgit (push)
   ```

   现在你可以在命令行中使用字符串 `pb` 来代替整个 URL。 

3. 从远程仓库中抓取与拉取
   从远程仓库中获得数据，可以执行`git fetch [remote-name]`，这个命令会访问远程仓库，从中拉取所有你还没有的数据。执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

   ```
   $ git fetch [remote-name]
   ```

   ​

# 附录

|        命令         |  页码  |
| :---------------: | :--: |
|     git diff      | p37  |
| git diff --staged | p38  |
|                   |      |
|                   |      |