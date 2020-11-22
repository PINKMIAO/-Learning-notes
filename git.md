常用：

- git log

- git log --online

- git status

- git add .

- git remote 

- git remote -v

- git remote add origin url

  作为网络传输都要以origin命名

  另外还需要是干净的，只有被同一个项目上传过

  git remote remove <name> 

  移除

- git push origin master

- git branch

  查看

- git branch training

  本地生成分支

- git checkout training

  切换

- git push origin training 

  在远程生成新的分支

- git pull origin master

  获取 / 更新

- git merge master/training

  合并 master 或 training

- git commit --amend 

  撤销上一次的提交，并将暂存区的文件重新提交，所以在此此之前还有一个 git add .

- git rest HEAD 

- git checkout -- filename / .

- git rm filename

  删除

- git rm --cached filename

  删除暂存区中的文件



github不允许创建空文件夹



## 一、电脑本地初始化一个仓库

### 1. git init: 初始化一个电脑上本地仓库

终端进入项目目录，输入：

```kotlin
$ git init  
```

该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。
 详细介绍：`https://git-scm.com/book/zh/v2/Git-基础-获取-Git-仓库`

![img](https:////upload-images.jianshu.io/upload_images/5875141-f62243ce073e3a68.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/823/format/webp)



### 2. 创建远程仓库： 存放代码

目前我知道的git网站仓库：

码云:
 https://gitee.com/
 公有仓库、私有仓库都免费使用， 国内访问速度快。

github:
 https://github.com/
 公有仓库免费使用， 私有仓库收费， 有时候访问速度慢。

Bitbucket:
 https://bitbucket.org/
 公有仓库、私有仓库都免费使用， 但是免费的最多只能有5个用户对仓库进行读写，超过的就需要付费。
 访问速度有时很慢。

gitlab:
 公司自建的git服务器，随意使用。

创建仓库都差不多，在网站中点击新建仓库，然后选择仓库的类型(公有、私有)，然后点击创建即可。



### 3. 本地Git添加远程仓库地址

#### git remote add <shortname> <url>： 添加仓库

本地git初始化后，此时还没有添加远程仓库地址，需要添加一个远程仓库地址才能上传代码到服务器。
 可在终端中运行`git remote add <shortname> <url>` 添加一个新的远程 Git 仓库。
 `shortname`是`url`的简写，当上传代码的时候，可用这个简写代替`url`地址。
 命令详细介绍：https://git-scm.com/book/zh/v2/Git-基础-远程仓库的使用

```csharp
$ git remote add pb https://github.com/paulboone/ticgit
```

#### `git remote -v`：查看本地git的远程仓库地址

当添加好远程仓库后，可以使用命令来查看添加的仓库是否正确。



```csharp
# 列出你指定的每一个远程服务器的简写
$ git remote
origin  

# 指定选项 -v，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。
$ git remote -v
origin  https://github.com/schacon/ticgit (fetch)
origin  https://github.com/schacon/ticgit (push)
```

#### git remote show [remote-name] ：查看远程仓库详细信息

如果想要查看某一个远程仓库的更多信息，可以使用 git remote show [remote-name] 命令。
 如果想以一个特定的缩写名运行这个命令，例如 origin，会得到像下面类似的信息：



```dart
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

#### 远程仓库的重命名、移除、修改地址

如果想要重命名引用的名字可以运行 git remote rename 去修改一个远程仓库的简写名。
 例如，想要将 pb 重命名为 paul，可以用 git remote rename 这样做：



```ruby
$ git remote rename pb paul
$ git remote
origin
paul
```

这同样也会修改你的远程分支名字。 那些过去引用 pb/master 的现在会引用 paul/master。

如果因为一些原因想要移除一个远程仓库  - 可以使用 git remote rm ：



```ruby
$ git remote rm paul
$ git remote
origin
```

修改地址：



```css
git remote set-url origin www.baidu.com
```

[***关于远程仓库详细介绍\***](https://git-scm.com/book/zh/v2/Git-基础-远程仓库的使用)

## 二、从远程仓库(服务器)获取代码：本地什么也没有，拉取代码

### 1. 获取默认分之代码：`git clone [URL]`

如果本地没有代码，远程仓库有代码，则需要从远程仓库克隆代码。
 克隆仓库的命令格式是 `git clone [url]` 。 比如，要克隆 Git 的可链接库 libgit2，可以用下面的命令：



```php
$ git clone https://github.com/libgit2/libgit2   
```

这会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 .git 文件夹，
 从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。
 如果你进入到这个新建的 libgit2 文件夹，你会发现所有的项目文件已经在里面了，
 准备就绪等待后续的开发和使用。

#### ① 拉取仓库时自定义本地仓库文件名： `git clone [URL] [新名字]`

如果你想在克隆远程仓库的时候，自定义本地仓库的名字，你可以使用如下命令：



```php
$ git clone https://github.com/libgit2/libgit2 mylibgit    
```

这将执行与上一个命令相同的操作，不过在本地创建的仓库名字变为 mylibgit。

关于克隆远程仓库命令详细介绍：https://git-scm.com/book/zh/v2/Git-基础-获取-Git-仓库



### 2. `git clone -b [分支名] [代码地址]`

直接拉取特定分之的代码

如果不想克隆默认分之的代码， 也可以克隆特定分之的代码：

```bash
$ git  clone http://192.168.102.9/jas-paas/cloudlink-front-framework.git
Cloning into 'cloudlink-front-framework'...
remote: Counting objects: 14436, done.
remote: Compressing objects: 100% (3072/3072), done.
remote: Total 14436 (delta 11224), reused 14226 (delta 11075)
Receiving objects: 100% (14436/14436), 17.93 MiB | 4.98 MiB/s, done.
Resolving deltas: 100% (11224/11224), done.
```



### 3. `git clone -b [标签名] [代码地址]`

直接拉取特定标签(tag)的代码

git允许直接克隆特定分之的代码，不过克隆好后，项目里没有分之，需要自己创建一个分支：



```csharp
$ git clone -b V2.2 http://192.168.102.9/jas-paas/cloudlink-front-framework.git
Cloning into 'cloudlink-front-framework'...
remote: Counting objects: 14436, done.
remote: Compressing objects: 100% (3072/3072), done.
remote: Total 14436 (delta 11224), reused 14226 (delta 11075)
Receiving objects: 100% (14436/14436), 17.93 MiB | 4.70 MiB/s, done.
Resolving deltas: 100% (11224/11224), done.
Note: checking out 'eeea534cdae1f82c48c7b0de8f9993b54ffa065d'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>
```

从信息中就可以看见，克隆特定标签的项目后，是没有分之的，需要进入项目目录后，使用命令创建一个分支：

```ruby
$ git checkout -b V2.2
```



### 4. `git checkout -b [分支名] [远程仓库名]/[分支名]`

如果已经拉取了代码，还需要拉取其他分支代码

![img](https:////upload-images.jianshu.io/upload_images/5875141-070369c90140fd49.png?imageMogr2/auto-orient/strip|imageView2/2/w/770/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/5875141-d883f6691ac25faa.png?imageMogr2/auto-orient/strip|imageView2/2/w/939/format/webp)

112289165-WX20170420-162329.png

例如，本地就一个master分支，远程有2个分支(master,develop)，把远程的develop拉取到本地：



```bash
# 本地的分支是干净的，也就是没有修改的文件
# 获取远程所有分支名字
~ git fetch
# 显示远程所有分支名字
~ git branch -a
# 提取远程新分支到本地
~ git checkout -b develop origin/develop       
```

------









## 三、记录每次更新到仓库

### 1. GIT中文件状态介绍(已跟踪/未跟踪)

工作目录下每个文件只有两种状态：

#### 已跟踪的两种状态

指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。
 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。
 git会自动管理`已跟踪`的文件，记录文件处于什么状态中。

##### 已暂存(Changes to be committed)

文件在这个状态下的，说明已经准备好把文件提交了，可以把这次代码变动记录保存在历史记录中。
 这个状态为GIT可以提交的内容。

##### 已修改(Changes not staged for commit)

这状态下的文件， git只是知道修改了那些内容，但是并不会在提交代码的时候把这部分内容提交上去。
 如果需要提交这部分代码需要使用命令`git add` 把文件添加到 已暂存中，然后提交代码。

#### 未跟踪(Untracked files)

工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。
 git不会去管理这些文件。

### 2. `git status`: 查看当前文件状态

使用`git status`命令可以查看文件处于什么状态，例如：



```rust
$ git status
On branch P02                                   // 告诉你当前是哪个分之下
Your branch is up-to-date with 'origin/P02'.    // 当前分之是从哪个仓库更新的
Changes to be committed:                        // 已暂存状态
  (use "git reset HEAD <file>..." to unstage)   // 使用这个命令可回退到Changes not staged for commit:

    modified:   fileName.ts

Changes not staged for commit:                  // 已跟踪文件的内容发生了变化
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   test.txt

Untracked files:                                // 未跟踪的文件
  (use "git add <file>..." to include in what will be committed)

    src/library/
```

在`git status`命令输出的信息中：

Changes to be committed:
 已暂存状态
 如果此时提交，那么该文件此时此刻的版本将被留存在历史记录中。

Changes not staged for commit:
 已跟踪文件的内容发生了变化，但还没有放到暂存区。

Untracked files:
 未跟踪的文件，意味着 Git 在之前的快照（提交）中没有这些文件；
 Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文件”，
 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。

### 3. `git status -s`紧凑格式输出

使用`git status -s`命令或`git status --short`命令，你将得到一种更为紧凑的格式输出。



```php
$ git status -s           
M  README              // 出现在右边的 M 表示该文件被修改了但是还没放入暂存区
MM Rakefile            // 文件已经放入暂存区，但是又修改过了，在已修改中也存在
A  lib/git.rb          // 新添加到暂存区中的文件前面有 A 标记
M  lib/simplegit.rb    // 出现在靠左边的 M 表示该文件被修改了并放入了暂存区
?? LICENSE.txt         // 新添加的未跟踪文件前面有 ?? 标记
```

### 4. GIT代码管理：改变文件的状态

#### git add

`git add`命令是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，
 还能用于合并时把有冲突的文件标记为已解决状态等。
 将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。
 例子：
 src/test.txt文件是一个未跟踪文件，可使用`git add`命令开始跟踪：



```csharp
$ git add src/test.txt    //指定单个文件添加
$ git add src/*           //指定src目录下所有文件都添加
```



#### `git checkout -- <file>`： 撤消对文件的修改

如果你并不想保留对 CONTRIBUTING.md 文件的修改,
 将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）
 可使用如下命令：

```css
git checkout -- CONTRIBUTING.md
```

注：你需要知道 git checkout -- [file] 是一个危险的命令，这很重要。 你对那个文件做的任何修改都会消失 - 你只是拷贝了另一个文件来覆盖它。除非你确实清楚不想要那个文件了，==否则不要使用这个命令==。



#### `git reset HEAD <file>`：取消暂存的文件

如果你在提交代码的时候，不想提交一些文件，可使用该命令把文件从暂存中回退到已修改的文件中。



```css
git reset HEAD CONTRIBUTING.md     
```

关于撤销操作的详细介绍： https://git-scm.com/book/zh/v2/Git-基础-撤消操作



### 5. .gitignore：忽略文件，不纳入GIT版本控制

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。
 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。
 在这种情况下，我们可以在项目根目录创建一个名为 .gitignore 的文件，列出要忽略的文件模式。

文件 .gitignore 的格式规范如下：

1. 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
2. 可以使用标准的 glob 模式匹配。
3. 匹配模式可以以（/）开头防止递归。
4. 匹配模式可以以（/）结尾指定目录。
5. 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

一个 .gitignore 文件的例子：



```bash
# 忽略所有后缀为 .a 的文件
*.a
# 但是要跟踪lib.a，即使你上面忽略了 .a文件
!lib.a
# 仅忽略当前目录的TODO文件夹， 不是subdir/TODO 
/TODO
# 忽略build /目录中的所有文件
build/
# 忽略doc / notes.txt，而不是doc / server / arch.txt
doc/*.txt
# 忽略doc 目录中的所有.pdf文件
doc/**/*.pdf
```

GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore 文件列表:
 https://github.com/github/gitignore

这部分详细介绍： https://git-scm.com/book/zh/v2/Git-基础-记录每次更新到仓库



### 6. git diff: 查看代码修改详细内容

如果你想知道文件具体修改了什么，可以用`git diff`命令。

#### Ⅰ. git diff： 查看尚未暂存的文件修改记录

`git diff`命令查看修改之后还没有暂存起来的变化内容。也就是`git status`命令输出信息中，
 `Changes not staged for commit:`下面的文件， 例如：



```cpp
$ git diff         //查看所有详细修改
$ git diff 1.txt   //只查看 1.txt文件的 详细修改   

//输出信息如下   
diff --git a/.gitignore b/.gitignore   //①
index e722882..f98470c 100644          //②
--- a/.gitignore    //③
+++ b/.gitignore    //③
@@ -3,8 +3,8 @@     //④

 *~
 *.sw[mnpcod]
-.DS_Store          //⑤
 *.log              //⑥
+text.t             //⑦
 *.tmp
 *.tmp.*
 log.txt
```

① git格式的diff,进行比较的是，a版本的.gitignore（即变动前）和b版本的.gitignore（即变动后）。
 ② 表示两个版本的git哈希值（index区域的e722882对象，与工作目录区域的f98470c对象进行比较），
 最后的六位数字是对象的模式（普通文件，644权限）。
 ③ 表示进行比较的两个文件。  "---"表示变动前的版本，"+++"表示变动后的版本。
 ④ 下面代码所在的行：
 -3,7： 修改前下面信息变化的行， 从第3行开始一直到第7行
 +3,6： 修改后下面信息变化的行， 从第3行开始一直到第6行
 ⑤ 以`-` 开头表示删除的代码
 ⑥ 以空格开头表示没有修改
 ⑦ 以`+`开头便是增加的代码

[git diff输出结果介绍](http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html)

#### Ⅱ. git diff --staged： 查看以暂存文件修改记录

查看已暂存的将要添加到下次提交里的内容可是使用命令`git diff --staged`(GIT版本1.6.1以上)。
 也可以使用`git diff --cached`命令。



```cpp
git diff --staged        // 查看所有已暂存文件的修改记录
git diff --staged t.txt  // 只查看1.txt文件 已暂存的修改记录
```

#### Ⅲ. git diff 的其他用法: --stat HEAD SHA1



```cpp
~ git diff --stat         // 查看简单的diff结果，只查看修改的文件名、修改了多少内容
~ git diff HEAD           // 查看所有修改记录(已暂存、已修改)：显示工作版本(Working tree)和HEAD的差别
~ git diff topic master   // 直接将两个分支上最新的提交做diff
~ git diff HEAD^ HEAD     // 比较上次提交commit和上上次提交
~ git diff SHA1 SHA2      // 比较两个历史版本之间的差异
```



### 7. 把代码纳入版本控制中： 提交更新 `git commit`

当所有需要提交的代码都放到暂存区后，就可以提交代码了：

```ruby
$ git commit
```

这种方式会启动文本编辑器以便输入本次提交的说明。
 一般都是 vim 或 emacs。使用 git config --global core.editor 命令设定你喜欢的编辑软件。
 编辑器会显示类似下面的文本信息（本例选用 Vim 的屏显方式展示）：



```bash
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#   new file:   README
#   modified:   CONTRIBUTING.md
#
~
~
".git/COMMIT_EDITMSG" 9L, 283C
```

可以在这段文本信息中看见， 所有以`#`符号开头的行，都会被忽略，也就是`#`是注释行，不会放到提交信息中去。
 接下来就可以输入一些提交信息了，关于写提交信息的建议：

信息应当以少于 50 个字符（25个汉字）的单行开始且简要地描述变更，接着是一个空白行，再接着是一个更详细的解释。
 Git 项目要求一个更详细的解释，包括做改动的动机和它的实现与之前行为的对比 - 这是一个值得遵循的好规则。

提交信息简单的模板：



```undefined
修改的摘要（50 个字符或更少）

如果必要的话，加入更详细的解释文字。在
大概 72 个字符的时候换行。在某些情形下，
第一行被当作一封电子邮件的标题，剩下的
文本作为正文。分隔摘要与正文的空行是
必须的（除非你完全省略正文）；如果你将
两者混在一起，那么类似变基等工具无法
正常工作。
```

关于VIM编辑器使用教程： [**vim编辑器常用命令与用法总结**](vim编辑器常用命令与用法总结)

#### commit命令的其他的用法： -m选项，提交信息简写

在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行，如下所示：



```csharp
$ git ci -m "测试提交"
[master f949399] 测试提交
 1 file changed, 1 insertion(+)
```

从输出的信息中可以看见：
 当前是在哪个分支（master）提交的，本次提交的完整 SHA-1 校验和是什么（f949399），
 以及在本次提交中，有多少文件修订过，多少行添加和删改过。

***注意：\***       
 提交时记录的是放在暂存区域的快照。 任何还未暂存的仍然保持已修改状态，可以在下次提交时纳入版本管理。
 每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。

#### commit命令的其他的用法： -a选项,提交所有修改过的文件（已暂存，已修改）

有时候把修改的提交到暂存区，然后在提交代码比较繁琐， GIT提供了一种`-a`选项，在提交代码的时候，
 跳过暂存区，把所有已经跟踪的文件暂存起来一并提交，从而跳过`git add`步骤，例如：



```rust
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md

no changes added to commit (use "git add" and/or "git commit -a")

$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```

从输出信息中可以看到：提交之前不再需要 git add 文件“CONTRIBUTING.md”了。

关于`git commit`命令详解：https://git-scm.com/book/zh/v2/Git-基础-记录每次更新到仓库



### 8. git push： 把本地的跟新推送到远程仓库服务器上

当在本地开发完毕，需要把代码提交到服务器时，可使用`it push [remote-name] [branch-name]`命令。
 你想要将 master 分支推送到 origin 服务器时：

```cpp
$ git push origin master     // 把更新上传到 origin服务器的 master分之上。
$ git push                   // 上传本地所有分支代码到远程对应的分支上
```

只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。
当你和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。
你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。





## 四、 查看提交历史: git log

在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。 完成这个任务最简单而又有效的工具是 git log 命令：

```tsx
$ git log
commit ca82a6dff817ec66f44342007202690a93763949    // 提交的 SHA-1 校验和
Author: Scott Chacon <schacon@gee-mail.com>        // 作者的名字和电子邮件地址
Date:   Mon Mar 17 21:52:11 2008 -0700             // 提交时间

    changed the version number                     // 提交说明

commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700

    removed unnecessary test
```

默认不用任何参数的话，git log 会按提交时间列出所有的更新，最近的更新排在最上面。

### git log 的常用选项: -stat, -p, --name-status, --pretty=oneline, --abbrev-commit

#### `git log --stat`:显示每次更新的文件修改统计信息(修改的文件名，每个文件添加的多少、删除了多少数字)。

#### `git log --name-only`: 只显示修改的文件名，没有其他信息

#### `git log --pretty=oneline`: 用一行显示信息



```cpp
$ git log 1.txt            //查看 1.txt文件的历史修改记录。
$ git log -n               //只查看前n次修改记录，例如查看前2次的记录： git log -2
$ git log -p               // 按补丁格式显示每个更新之间的差异。  
$ git log --shortstat      // 只显示 --stat 中最后的行数修改添加移除统计。
$ git log --name-status    // 显示新增、修改、删除的文件清单。     
$ git log --abbrev-commit  // 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。
$ git log --relative-date  // 使用较短的相对时间显示（比如，“2 weeks ago”）。
$ git log --graph          // 显示 分支合并历史。   
$ git log --pretty=oneline // 用一行显示信息
// 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。
```

[***关于git log命令详解\***](https://git-scm.com/book/zh/v2/Git-基础-查看提交历史)

## 五、 打标签(一个版本发布) git tag

像其他版本控制系统（VCS）一样，Git 可以给历史中的某一个提交打上标签，以示重要。
 比较有代表性的是人们会使用这个功能来标记发布版本节点（v1.0 等等）。

### 1. 什么是标签？

标签可以认为是版本管理工具的一个版本，也就是说像很多第三方开源软件安装包表示的版本号，
 比如说1.0,2.0等等，它代表修复了一个bug,或者重大的重构，git的标签也是这样的一个作用，
 可以对版本的不同阶段做一个标示。

### 2. 查看现有的标签

#### ① 查看所有标签：`git tag`

```ruby
$ git tag   
v0.1
v1.3
v1.4
```

这个命令以字母顺序列出标签；但是它们出现的顺序并不重要。

#### ② 查看符合条件的标签：`git tag -l 'v1.*'`

也可以使用特定的模式查找标签, 如果只对 1.0以上tag感兴趣，可以运行：

```ruby
$ git tag -l 'v1.*'
```

#### ③ 查看一个标签的详细信息：`git tag show V1.4`

该命令会输出打标签者的信息、打标签的日期时间、附注信息，然后显示具体的提交信息。



### 3. 创建标签：Git 使用两种主要类型的标签：

#### ① `git tag -a`:创建附注标签（annotated）

附注标签是存储在 Git 数据库中的一个完整对象。
 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；
 并且可以使用 GNU Privacy Guard （GPG）签名与验证。 通常建议创建附注标签，这样你可以拥有以上所有信息。

创建一个附注标签，使用`tag`命令时指定`-a`选项：

```ruby
$ git tag -a v1.4 -m 'my version 1.4'    
```

`-m`选项指定了一条将会存储在标签中的信息。 如果没有为附注标签指定一条信息，Git 会运行编辑器要求你输入信息。

如果你有自己的私钥，还可以用 GPG 来签署标签，只需要把之前的`-a`改为`-s`（取 signed 的首字母）即可：

```bash
git tag -s v1.5 -m 'my signed 1.5 tag'
```

再运行 git show 会看到对应的 GPG 签名也附在其内.

#### ② `git tag V1.4`: 创建轻量标签（lightweight）

如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，则可以选择使用轻量标签。
 轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。
 创建轻量标签，不需要使用 -a、-s 或 -m 选项，只需要提供标签名字：

```ruby
$ git tag v1.4-lw
```

#### ③ `git tag -a v1.2 9fceb02`：对某个历史提交打标签(后期打标签)

如果你在项目版本发布的时候忙忘记打标签，你可以在之后补上标签。
 要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）:

```css
git tag -a v1.2 9fceb02    
```



### 4. 把创建的标签推送到服务器上：`git push origin [tagname]`

默认情况下，`git push`命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。

```bash
$ git push origin v1.5

Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```

如果想要一次性推送很多标签，也可以使用带有`--tags`选项的`git push`命令。
这将会把所有不在远程仓库服务器上的标签全部传送到服务器。

```dart
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
 * [new tag]         v1.4-lw -> v1.4-lw
```

现在，当其他人从仓库中克隆或拉取，他们也能得到你的那些标签。



### 5. 检出标签: 代码变成标签的样子

在 Git 中你并不能真的检出一个标签，因为它们并不能像分支一样来回移动。
如果你想要工作目录与仓库中特定的标签版本完全一样，
可以使用`git checkout -b [branchname] [tagname]`在特定的标签上创建一个新分支：

```csharp
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```



### 6. 删除标签

#### ① `git tag -d v1.0`:删除本地标签

```ruby
$ git tag -d v1.0  
Deleted tag 'v1.0' (was 2ffea56)
```

#### ② `git push origin :refs/tags/v0.1`:删除服务器标签



```cpp
$ git push origin :refs/tags/v0.1
To http://192.168.95.95/user/test.git
 - [deleted]         v0.1
```

如果服务器的标签删除后，本地的也需要自己删除。

[***关于git tag详解\***](https://git-scm.com/book/zh/v2/Git-基础-打标签)







## 六、 GIT分支

### 1. 分支介绍作用

Git 的分支，其实本质上仅仅是指向提交对象的可变指针。 Git 的默认分支名字是 master。
 在多次提交操作之后，你其实已经有一个指向最后那个提交对象的 master 分支。 它会在每次的提交操作中自动向前移动。

分支作用：

1. 开发多个项目任务，比如说我有两个任务都比较紧急，任务1需要两天完成，任务2需要一天完成，而任务1是之前就已经开始进行的，任务二是中间加的新任务，所以需要第一天就完成任务2.
2. master分支始终要保证可发布的状态，用dev分支和bug分支进行开发和错误调试，这样能够保证主干代码的干净、可发布。
3. 自己开发测试或者修复BUG等等，可以避免代码的丢失。

### 2. 本地分支的创建、切换、删除：

#### ① 创建分支：`git branch testing`

比如，创建一个 testing 分支， 你需要使用 git branch 命令：

```ruby
$ git branch testing
```

这条命令会根据当前的分支来创建一个新分支，分支中内容与当前分支内容完全一样。
 `git branch`命令仅仅 创建 一个新分支，并不会自动切换到新分支中去。

#### ② 切换分支：`git checkout testing`

要切换到一个已存在的分支，你需要使用`git checkout`命令。 我们现在切换到新创建的 testing 分支去：

```ruby
$ git checkout testing 
```

#### ③ 创建并且换：`git checkout -b testing`

```cpp
$ git checkout -b testing
Switched to a new branch "testing"
```

这个命令会创建一个分支并切换到新分支中去， 实际上他是上面2条命令的简化版。

#### ④ 删除分支：`git branch -d testing`

*删除一个分支的时候，你不能在要删除的分支中，要切换到别的分支中，否则会报错。*      
 如果要删除 testing分支，可以使用带 -d 选项的 git branch 命令来删除分支：

```ruby
$ git br -d testing
Deleted branch testing (was 4baf2a3).
```



### 3. 远程仓库分支的新建与删除

#### ①远程仓库分支的新建： `git push origin testing`

如果想把本地的新分支推送到远程仓库，可使用 `git push [远程仓库名] [分支名]`：

```cpp
$ git br -a

  testing
* master
  remotes/origin/master

$ git push origin testing

Total 0 (delta 0), reused 0 (delta 0)
To http://192.168.132.55/user/test.git
 * [new branch]      testing -> testing
```

#### ② 删除远程仓库的无用分支：`git push origin --delete testing`

如果远程仓库某个分支无用了，想要删除，可以运行带有 --delete 选项的 git push 命令来删除一个远程分支：

```cpp
git push origin --delete testing
To http://192.168.132.55/user/test.git
 - [deleted]         testing
```



### 4. 分支合并：`git merge`

如果一个分支的功能开发完毕了，需要把开发的内容合并的其他分支中去，则需要使用 git的 merge命令。
 例如， 现在有2个分支(master, ningning), master分支的代码要合并到ningning,2个分支代码都已经git commit过了。
 如果没有commit，需要先commit，否则不能合并代码。

#### ① 合并命令：`git merge [分支名]`

```cpp
// 切换到需要添加新功能的分支上
$ git checkout ningning
// 把 master分支上的代码合并到ningning分支上
$ git merge master
```

#### ② 合并很完美，直接合并成功的提示

如果合并成功了则输出的信息类似于下面：

```csharp
$ git merge master 
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)   

// 或者下面的输出信息
Merge made by the 'recursive' strategy.
index.html |    1 +
1 file changed, 1 insertion(+)
```

Git 将合并的结果做了一个新的快照并且自动创建一个新的提交指向它。

#### ③ 合并过程中代码有冲突，解决冲突

![img](https:////upload-images.jianshu.io/upload_images/5875141-bf880c272552c95b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1158/format/webp)

3353411608-WX20170420-172823.png

错误信息类似于：



```css
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Auto-merging是指已经自动合并的文件，
 CONFLICT (content)是指有冲突的文件，需要解决冲突的文件。

打开有冲突的文件，冲突部分代码类似于下面：



```jsx
<<<<<<< HEAD:ningning
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> master  
```

<<<<< ======= 中的是 ningning分支的代码。
 ======= >>>>>> 中的是 master分支的代码。
 经过对比后删除冲突部分的代码， 并把 <<< ====  >>>> 所在行全部删除。
 保存后，使用`git add`添加修改的文件。
 使用`git commit`命令来完成合并提交：

![img](https:////upload-images.jianshu.io/upload_images/5875141-d22b1d42ef24cb49.png?imageMogr2/auto-orient/strip|imageView2/2/w/825/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/5875141-c251222140ad2e0e.png?imageMogr2/auto-orient/strip|imageView2/2/w/841/format/webp)

##### 解决冲突文件：`git checkout --ours/--theirs`放弃其中一个分支的冲突代码

如果想放弃一个分支文件的冲突代码，只保留一个分支的代码，可使用如下命令。

假如：冲突文件名为 1.txt，要放弃`master`分支的修改，可使用如下命令：

```ruby
$ git checkout --ours 1.txt
```

放弃`ningning`分支冲突代码，可使用如下命令：

```ruby
$ git checkout --theirs 1.txt 
```

#### ④ 取消合并

##### 冲突太多，解决冲突乱了， 可取消合并：`git reset --hard HEAD`

如果冲突代码太多了，解决冲突代码过程中产生了混乱，想要重新合并，可使用下面命令取消这次合并：

```csharp
git reset --hard HEAD
HEAD is now at 9e791f3 提交信息
```



##### 合并已经commit，还没上传远程仓库，取消合并：`git reset --hard HEAD^`

如果合并的代码产生了错误，或者合并有问题，但是已经commit了，但是还没有把合并提交到远程仓库，则可以使用如下命令取消这次合并：

```ruby
$ git reset --hard HEAD^ 
```

