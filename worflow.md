## 开始Coding与版本控制器（VC）集成

Coding粗犷de来说可以分成下面几个阶段：

1. 项目建立
2. 功能开发
3. 测试
4. 发布

这次的开发任务主要适用于独自solo的情况（大部分），当然，如果可以多人协作，也完全没有问题（maybe）。

### init-新建项目

#### 创建一个新的项目

新建项目就像字面意思来说，这里有两种情况，自己本地开发（或自己的远程服务器），使用代码托管平台（Github、Gitlab类似物）。

如果是本地项目，那么：

```sh
git init --bare /path/to/repo.git

# or

ssh username@host git init --bare /path/to/repo.git
```

使用平台的话就需要在平台上面新建项目。也可以使用对应的开放Api来写脚本。gitsome为例，可以简单创建一个新的项目：

```sh
gh create-repo repo "A new Repozzz.."
```

#### 初始化相关工作

创建好之后，把项目clone下来到本地：

```sh
git clone username@host:/path/to/repo.git
cd $_
```

master分支默认作为主要开发分支。

接下来是做一些项目的初始化工作。这其中包括一些项目必备的，README之类。其实在github上创建项目过程中，就会选择一些默认配置。

1. .gitignore - git提交时的忽略文件
2. README.md - 项目自述文件
3. LICENSE - 软件许可证

如果这里使用项目脚手架，就会简单很多。例如开发一个react项目，可以利用`create-react-app`来快速创建：

```sh
create-react-app foo
yarn test # may not need
cd foo
```

项目初始化完成之后，经行一次提交：

```sh
git add .
git commit -m "Initial repo commit."
git push origin master
```

总结一下流程：

1. 创建新项目
2. 将项目clone到本地并用项目脚手架创建项目
3. 提交并推送到远端一次

这里的2，3步骤可以互换，因为大部分工具零配置时需求一个空文件夹，所以我们可以先创建同名项目，再clone：

> `git clone .` 只能用于一个空文件夹

```sh
git init
git remote add -t origin path/to/repo.git
```

#### 集成项目脚手架与VC来创建项目

当然，这些繁琐工作应该用一条命令来实现。

```sh
create foo
```



### 需求驱动开发（FDD）任务发布与分支创建

程序开发是由需求提出来经行驱动，其实就像玩游戏一样，来完成一个个的任务来升级。具体提现在issues的创建，就是所谓的工单票。我们可以创建一个工单，工单具备标题（title）和描述（description）。如果使用像Github之类的平台，这个功能已经内置在里面。这里我们也可以用第三方工具，比如vekan。

有了新的需求，我们就可以根据需求来创建新的分支来经行开发。首先要做的就是要签出一个分支：

```sh
git checkout -b feature-4-add-dashboard-page
```

除了功能分支外，还有一种常见的分支就是补丁分支（hotfix）。

分支名称的描述规则是：

```
[feat|fix]-ID-<TITLE>
```

> 如果是团队协作，这里可以指定一个该功能负责人并签给他。更cool的是可以用机器人来自动签发任务到指定负责人身上


这样我们就可以根据具体的需求描述来实现逻辑。


#### 集成任务发布

TODO


### 进度控制与Project面板

TODO


#### 集成vekan

TODO



### Github 工作流


#### 创建一个功能分支

遇到新的需求，比如一个新的feature或是一个hotfix时，就要创建一个新的分支。新分支的名字需要具体一些，但不能是master这种特殊的功能分支。

#### 添加提交

这个阶段一般就是开始coding，完成分支的基本开发任务。提交粒度也尽量以一个功能作为块，经行一次提交。提交时，message需要写清楚。

#### 创建合并请求

认为当前的任务已经开发完成，就可以创建一个合并请求（pr）。合并时需要填写描述信息。创建成功后，开发组人员根据此次的提交来讨论并审查代码。当代码不合适或出现错误，别人会指出并大家一起商讨修改意见，这时会反复进行二三阶段，添加一些新的提交，并确保审查通过。审查之后，可以尝试部署在预发布环境中来查看软件的具体行为。出现错误或任何不满意的地方，可以回到上一阶段添加一些新的提交。当然，也可以废弃此次提交。

#### 合并请求

开发完成，合并当前分支。如果该分支是hotfix类型的，那这个分支也可能没什么用了，直接删掉的说；feature类型的分支，可以在基础上继续接着开发。




### Awesomes

- [Understanding the GitHub Flow](https://guides.github.com/introduction/flow/)
- [Gitlab workflow an overview](https://about.gitlab.com/2016/10/25/gitlab-workflow-an-overview/)
- [Gitlab Flow](https://about.gitlab.com/2014/09/29/gitlab-flow/)
- [Comparing workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [FDD wiki](https://en.wikipedia.org/wiki/Feature-driven_development)
- [Wekan](https://wekan.github.io/)
