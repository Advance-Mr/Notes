#                                                   Git

##   一    介绍代码版本控制系统

###            1.1   主流的是 Git & Svn 

###            1.2   介绍 && 区别

- ​         Git 是分布式的，SVN 是集中式

​              这是 Git 和 SVN 最大的区别。因为 Git 是分布式的，所以 Git 支持离线工作，在本地可以进行很多操作，包括接下来将要重磅推出的分支功能。而 SVN 必须联网才能正常工作。Git 复杂概念多，SVN 简单易上手所有同时掌握 Git 和 SVN 的开发者都必须承认，Git 的命令实在太多了，日常工作需要掌握add, commit, status, fetch, push, rebase等，若要熟练掌握，还必须掌握rebase和merge的区别，fetch和pull的区别等，除此之外，还有cherry-pick，submodule，stash等功能，仅是这些名词听着都很绕。

- ​         有无分支

​              大团队开发过程中，常常存在创建分支，切换分支的需求。Git 分支是指针指向某次提交，而 SVN 分支是拷贝的目录。这个特性使 Git 的分支切换非常迅速，且创建成本非常低。而且 Git 有本地分支，SVN 无本地分支。在实际开发过程中，经常会遇到有些代码没写完，但是需紧急处理其他问题，若我们使用 Git，便可以创建本地分支存储没写完的代码，待问题处理完后，再回到本地分支继续完成代码。

##   二    Git的安装和下载

### 2.1 流程介绍

####     2.1.1  安装Ｇit  

​              网上这个可以参考，当然还有很多，大家可以自行搜索哦，视频也很详细 ！

>  ( https://blog.csdn.net/mukes/article/details/115693833)   *<!--按住ctrl后单击鼠标左键即可访问链接-->**

​                                Git GUI: git的图形化界面                                               Git bash :   git的命令行

![image-20230930133154708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930133154708.png)

------

​                         ** 由于自带的图形化工具git GUI比较拉垮，我们安装一个Tortoise Git的工具**

------

####   2.1.2   安装Tortoise Git   

​           网上这个可以参考，当然还有很多，大家可以自行搜索哦，视频也很详细 ！

> (https://blog.csdn.net/dsh789/article/details/110057004 )**<!--按住ctrl后单击鼠标左键即可访问链接-->**

  重点是配置： 重点是这个配置---需要配置Git软件的安装目录

![image-20230930133813765](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930133813765.png)

windows寻找git位置

```
C:\Users\Administrator>where git
C:\Program Files\Git\cmd\git.exe
```

​       *！！！！！！！！(安装后操作文件的同时会有相应的图标，如加号、对勾etc)  ！！！！！！！！！！！！*

​         ***问题：***安装后文件上并没有相应操作的图案

​         ***解决：***由于安装后的展示图标的文件权限不够，在注册表中查看，一般都是由于其他文件对应注册表的空格太多，导致系统会优先给别的文件权限。比如你的电脑有one drive,它对应的注册表文件前面空格多，就会生效。为对应的Tortoise Git 文件在后面排列，没有显示出来。进入注册表修改即可。

####  2.1.3   Git介绍

  　　在Git中有四个概念：**「远程仓库、工作区、暂存区、版本库」**。远程仓库就是代码托管平台，用于存储已经管理团队的代码。本地仓库就是进行本地代码的修改，一般在我们自己的电脑上。

  　　**工作区、暂存区、版本库**是我们本地的，例如当我们初始化`git init`后，就会在当前的目录下出现`.git`目录，在.git目录下index文件(`.git/index`)，这就是**「暂存区」**。

![image-20231011123721752](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011123721752.png)

 我们需要将工作目录的代码添加到暂存区，然后在上传到本地历史仓库。如图所示：

![image-20230930140750859](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930140750859.png)

#### 2.1.4  命令化操作介绍

1. 现在windows的文件中创建一个文件夹，用来存放工作目录中的文件、代码或需要上传的东西。

2. 接着初始化这个文件夹，使其成文一个git控制管理的本地工作目录。

    执行命令 -  `git init`

3. 初始化为git管理的文件夹后，需要将所初始化后的下一个文件执行添加操作

    执行命令 - `git add 文件名字` （变为加号）

4. 命令 git status 可以查看当前文件的一个状态 。

5. 接下来就可以提交到远程仓库中去了，执行命令 - git commit '提交信息/commit first text.txt file '(变为对勾)

6. 当修改文件后，可以使用git log 进行日志查看。commit后面会显示每次提交后的唯一标识，以便进行版本切换

#### 2.1.5  图形化操作介绍

​          

1.添加git管理

![image-20230930143921262](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930143921262.png)

2.先添加，在提交

![image-20230930144040197](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930144040197.png)

![image-20230930144115316](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930144115316.png)

提交时，写上提交的日志信息，如：第一次提交

![image-20230930144141285](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230930144141285.png)

​       注意：当修改过文件时，不需要点添加，直接提交修改后的文件！！！

#### 2.1.6 命令删除操作

 工作区的文件 git add 后到暂存区，暂存区的文件 git [commit]后到版本库。

　

- *rm 命令删除**工作区**的文件*   

```
$ rm test.txt
```

- ​      查看状态（成功删除工作区文件）：*

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

* ​      删除版本库文件*

```
 git commit -m "delete test"
```

#### 2.1.7 版本切换

 在版本切换之前，我们提交的文件有唯一的字符串标识。我们需要通过日志首先查看唯一的标识。

```
git reflog:  //可以查看所有的分支标识
```

![image-20231011121546092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011121546092.png)

1. ​         ！！！切换的命令！！！

```
git reset  --hard （加上对应版本的唯一索引值）
```

分支：在原来基础上进行代码开发。当发现底层代码出现bug，即可切换到master下去修改而不需要全部重新搞

​         当不创建分支时，系统默认一个master分支，在一个分支上进行累计。

![image-20231011122126364](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011122126364.png)



-  创建分支                                          git branch  (name)

- 查看分支                                           git status 

- 切换分支                                           git checkout  (名字)

- 查看当前分支下的文件                  ls  

- 分支合并                                          git merge   (name)

- 分支删除                                          git  branch  -d   (name) 
### 2.2   远程仓库介绍   

​            Gitee（码云）：是开源中国社区推出的代码托管协作开发平台，支持Git和SVN，提供免费的私有仓库托管。Gitee专为开发者提供稳定、高效、安全的云端软件开发协作平台，无论是个人、团队、或是企业，都能够实现代码托管、项目管理、协作开发。

​            Gitee是开源中国（OSChina）推出的基于Git的代码托管服务。

####   2.2.1  准备工作

​              新建仓库![image-20231011124751828](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011124751828.png)

​                                                  <!--创建完之后做推送，But是得配置ssh！！！-->

​    配置流程：1.查看账户和邮箱  

​                                                          ![image-20231011125042398](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011125042398.png)

​                             2.设置全局

​                                                       ​![  ](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011125115816.png) 

![image-20231011125251225](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011125251225.png)

3.查看之前是否生成过ssh公钥。（图显示未生成）              命令：cd ~/.sh 

4.生成公钥                                                                                      命令：ssh-keygen  -t  rsa  -C   "自己的邮箱"             

 然后三次回车，如图：![image-20231011125618992](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011125618992.png)

5.查看生成的公钥               命令：cat  ~/.ssh/id_ras.pub

6.最后配置---将查看的公钥复制到如下图所示：

![image-20231011125843811](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011125843811.png)

​                                                                 结束！！！

7.验证       命令：ssh -T git@邮箱 

如图：![image-20231011130205440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011130205440.png)

#### 2.2.2 推送

1.  重新指定远程仓库url的名称： git remote add   [rename]  url       

​                                                        url:在自己创建的仓库中复制，https和ssh都可以

  ![image-20231011130624625](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011130624625.png)

2.  提交：   git push -u  [rename]    [那个分支]

​                总结：![image-20231011130832884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011130832884.png)

3. 克隆 git clone  [远程仓库的url] ![image-20231011131235240](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011131235240.png)

4. 拉取：当把远程仓库文件克隆本机后，对其添加文件并推送。但你最初创建用本推送的文件夹是没有显示，需要重新在远程仓库拉取下来。                  命令：      git pull [rename] [分支]

   

![image-20231011132756387](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011132756387.png)

## 三    git集成idea

###   3.1  流程

####   3.1.1  idea中添加git

 在idea中找到设置，版本控制，test是否添加到Git的安装目录。（需要追加到exe文件）

###   ![image-20231011201819302](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011201819302.png)

​                        

​                                                                                        集成完毕！！！

#### 3.1.2 创建本地仓库

将代码的这个文件夹创建为本地仓库。如图所示：

![image-20231011202614607](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011202614607.png)

点击create Git Repository进行相应文件设置为本地仓库。

![image-20231011202710514](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011202710514.png)

​    **<!--注意：蓝色的箭头是拉取 ，绿色的对号是提交！（拉取，pull，不是clone!!!）-->**

![image-20231011202940472](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011202940472.png)

​      **<!--查看不同的提交：点击底下的版本控制，点击日志即可！-->**

![image-20231011203129862](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011203129862.png)



#### 3.1.3 版本回退

- 点击 reset Current Branch to Here会回退之前的代码并且不会保存之后的记录。

     ![image-20231011203717940](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011203717940.png) 

- 点击revert  Commit会重新提交代码，并且进行一个代码的冲突处理，之后重新提交。

​                 Revet 操作会当成一个新的提交记录这种回退的好处在于，如果后悔了“回退”这个操作也可以回退到没有回退之前的版本因为历史记录还保留提交记录

  ![ ](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011203903977.png)           

#### 3.1.2 创建分支

-   VCS->Git->Branchs ->New Branch  创建好之后输入名称，会在右下角显示。

-  点击即可切换！！！

  ![image-20231011204246095](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011204246095.png)

- 分支删除 VCS->git->branch ，如图所示：

  ![image-20231011204548498](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011204548498.png)

合并过程中会遇到冲突！如图：

![image-20231011204712281](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011204712281.png)

注意：如果当前是master分支，点击合并后会提示你需要勾选合并那个分支。将勾选的分支合并到当前右下角显示的分支里。

#### 3.1.3 P/C远程仓库

- 切换到当前项目下，点击 VCS  ->Git-> Push，点击Dedine remote，对远程仓库起名：

​               ![image-20231011205355100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011205355100.png)    



- 将远程仓库代码克隆：如下图所示点击Git![image-20231011205541347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011205541347.png)

​                    输入url和paths:一路确定即可！！！

​                   ![image-20231011205705274](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231011205705274.png)  

