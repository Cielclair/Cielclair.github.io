##### Git Flow学习笔记



[GitFlow+Gitlab工作流及Git规范](https://www.jianshu.com/p/d46da933c180)

[另一个参考](https://zhuanlan.zhihu.com/p/66048537)

[yet another](https://www.cnblogs.com/cnblogsfans/p/5075073.html)



## GitFlow

![GitFlow模型图](https://upload-images.jianshu.io/upload_images/4822184-1b70ca8c0a9069d2.png)



### 版本号(tag)

- 版本号(tag)命名规则：`主版本号.次版本号.修订号`，如`2.1.13`。(遵循GitHub[语义化版本](https://links.jianshu.com/go?to=https%3A%2F%2Fsemver.org%2Flang%2Fzh-CN%2F)命名规范)
- 版本号仅标记于master分支，用于标识某个可发布/回滚的版本代码
- 对master标记tag意味着该tag能发布到生产环境
- 对master分支代码的每一次更新(合并)必须标记版本号
- 仅项目管理员有权限对master进行合并和标记版本号

### 项目权限

Git权限分管理员、开发者、浏览者三种类型

- 浏览者(Guest)只能浏览代码，无push、pull request等所有写权限

- 开发者(Developer)拥有浏览、push非主分支、提交pull request工单权限

- 管理员(Master)拥有建立和管理Git项目、合并分支和代码、给master打tag版本号等权限

### 分支使用

主分支**master**和**develop**是保护分支，只能进行合并请求，均不可直接提交代码。

- 功能需求或常规Bug修复，请从develop拉取 `feature` 分支；

- 线上紧急问题修复，请从master拉取 `hotfix` 分支。

### 代码合并

将开发完毕的分支，`拉取` develop最新代码，`merge` 并 `解决冲突` 后，之后在对应的feature分支创建并 `提交` 到develop分支，并自动触发merge request请求，然后进行code review，确认无误后再合并。

**注意：**

- 每个merge request不要包含不相关的功能
- merge request提交后需要及时跟踪动态，包括通过、打回等
- 该功能进入提测流程后，需删除之前的功能分支

### 注释格式

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。

其中，`Header` 是必需的，`Body` 和 `Footer` 可以省略。不管是哪一个部分，任何一行都不得超过72个字符。这是为了避免自动换行影响美观。

#### Header

Header部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）。

- `type` 用于说明 commit 的类别，只允许使用下面7个标识。

  > - feat：新功能（feature）
  > - fix：修补bug
  > - docs：文档（documentation）
  > - style： 格式（不影响代码运行的变动）
  > - refactor：重构（即不是新增功能，也不是修改bug的代码变动）
  > - test：增加测试
  > - chore：构建过程或辅助工具的变动

- `scope` 用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

- `subject` 是 commit 目的的简短描述，不超过50个字符。

  > - 以动词开头，使用第一人称现在时，比如change，而不是changed或changes
  > - 第一个字母小写
  > - 结尾不加句号（.）

#### Body

Body 部分是对本次 commit 的详细描述，可以分成多行, 有两个注意点:

- 使用第一人称现在时，比如使用change而不是changed或changes;
- 应该说明代码变动的动机，以及与以前行为的对比;

#### Footer

Footer 部分只用于两种情况:

- 如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法;
- 如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue (Closes #123, #245, #992), GitHub这个功能很好用；

Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以 `revert:` 开头，后面跟着被撤销 Commit 的 Header。

> 为什么要约定注释格式？
>
> 1. 加快 Reviewing Code 的过程
> 2. 帮助我们写好 release note
> 3. 5年后帮你快速想起来某个分支，tag 或者 commit 增加了什么功能，改变了哪些代码 
> 4. 让其他的开发者在运行 git blame 的时候想跪谢
> 5. 一个好的提交信息，会帮助你提高项目的整体质量

### 分支说明

| 名称    | 说明                                     | 命名规范              | 命名示例             | 合并目标       | 合并操作      |
| ------- | ---------------------------------------- | --------------------- | -------------------- | -------------- | ------------- |
| master  | 线上稳定版本                             | master                | master               | --             | --            |
| release | 待发布分支，下个版本需上线的版本         | release/xxx           | release/v1.0.0       | master         | merge request |
| develop | 当前正在开发的分支                       | develop               | develop              | master         | merge request |
| feature | 功能分支，每个功能需分别建立自己的子分支 | feature/版本号-功能名 | feature/v1.0.0-Login | develop        | merge request |
| hotfix  | 紧急修复分支                             | hotfix/xxx            | hotfix/v1.0.1        | master/develop | merge request |

### 分支约定

Git Flow有**主分支**和**辅助分支**两类分支。其中主分支用于组织与软件开发、部署相关的活动；辅助分支组织为了解决特定的问题而进行的各种开发活动。

#### 主分支

![img](https://upload-images.jianshu.io/upload_images/4822184-056668f897b82511.png)

主分支是所有开发活动的核心分支。所有的开发活动产生的输出物最终都会反映到主分支的代码中。**主分支分为 `master` 分支和 `develop` 分支**。

##### master 分支

- master分支存放的是随时可供在生产环境中部署的稳定版本代码
- master分支保存官方发布版本历史，release tag标识不同的发布版本
- 一个项目只能有一个master分支
- **仅在发布新的可供部署的代码时才更新master分支上的代码**
- 每次更新master，都需对master添加指定格式的tag，用于发布或回滚
- master分支是保护分支，不可直接push到远程仓master分支
- master分支代码只能被release分支或hotfix分支合并



##### develop 分支

- develop分支是保存当前最新开发成果的分支
- 一个项目只能有一个develop分支
- develop分支衍生出各个feature分支
- develop分支是保护分支，不可直接push到远程仓库develop分支
- develop分支不能与master分支直接交互

每次将develop分支上的代码合并回master分支时，我们都可以认为一个新的可供在生产环境中部署的版本就产生了。基于此，理论上说，每当有代码提交到master分支时，我们可以使用Git Hook触发软件自动测试以及生产环境代码的自动更新工作。这些自动化操作将有利于减少新代码发布之后的一些事务性工作。

只有`开发管理员`有合并的权限

### 辅助分支

![img](https://upload-images.jianshu.io/upload_images/4822184-024429e135c8d8d5.png)

辅助分支是用于组织解决特定问题的各种软件开发活动的分支。辅助分支主要用于组织软件新功能的并行开发、简化新功能开发代码的跟踪、辅助完成版本发布工作以及对生产代码的缺陷进行紧急修复工作。这些分支与主分支不同，通常只会在有限的时间范围内存在。跟“历史性”分支相反，这三类分支都是短期分支，针对他们的工作内容完成后，一般都要进行删除。

**工作内容完成的标识有两个：开发完成、合并完成，缺一不可。**

辅助分支包括：

- 用于开发新功能时所使用的`feature`分支
- 用于辅助版本发布的`release`分支
- 用于修正生产代码中的缺陷的`hotfix`分支

以上这些分支都有固定的使用目的和分支操作限制。从单纯技术的角度说，这些分支与Git其他分支并没有什么区别，但通过命名，我们定义了使用这些分支的方法。

##### feature 分支

使用规范：

- 分支的命名格式必须是版本号-功能名，例如`v1.0.0-login`
- develop分支的功能分支
- feature分支使用develop分支作为它们的父类分支
- 以功能为单位从develop拉一个feature分支
- 每个feature分支颗粒要尽量小，以利于快速迭代和避免冲突
- 当其中一个feature分支完成后，它会合并回develop分支
- 当一个功能因为各种原因不开发了或者放弃了，这个分支直接废弃，不影响develop分支
- feature分支代码可以保存在开发者自己的代码库中而不强制提交到主代码库里
- 由每组开发管理员负责把所有feature分支开发完成的代码合并到develop分支
- feature分支只与develop分支交互，不能与master分支直接交互

如有几个同事同时开发，需要分割成几个小功能，每个人都需要从develop中拉出一个feature分支，但是**每个feature颗粒要尽量小**，因为它需要我们能尽早merge回develop分支，否则冲突解决起来就没完没了。同时，当一个功能因为各种原因不开发了或者放弃了，这个分支直接废弃，不影响develop分支。

##### release 分支

使用规范：

- 命名规则：`release/*`，“*”以本次发布的版本号为标识
- release分支主要用来为发布新版的测试、修复做准备
- 当需要为发布新版做准备时，从develop衍生出一个release分支
- release分支可以从develop分支上指定commit派生出
- release分支测试通过后，合并到master分支并且给master标记一个版本号
- **release分支一旦建立就将独立，不可再从其他分支pull代码**
- 必须合并回develop分支和master分支

release分支是为发布新的产品版本而设计的。在这个分支上的代码允许做小的缺陷修正、准备发布版本所需的各项说明信息（版本号、发布时间、编译时间等）。通过在release分支上进行这些工作可以让develop分支空闲出来以接受新的feature分支上的代码提交，进入新的软件开发迭代周期。

当develop分支上的代码已经包含了所有即将发布的版本中所计划包含的软件功能，并且已通过所有测试时，我们就可以考虑准备创建release分支了。而所有在当前即将发布的版本之外的业务需求一定要确保不能混到release分支之内（避免由此引入一些不可控的系统缺陷）。

成功的派生了release分支，并被赋予版本号之后，develop分支就可以为“下一个版本”服务了。所谓的“下一个版本”是在当前即将发布的版本之后发布的版本。版本号的命名可以依据项目定义的版本号命名规则进行。

##### hotfix 分支

使用规范：

- 命名规则：`hotfix/*`，“*”以本次发布的版本号为标识
- hotfix分支用来快速给已发布产品修复bug或微调功能
- 只能从master分支指定tag版本衍生出来
- 一旦完成修复bug，必须合并回master分支和develop分支
- master被合并后，应该被标记一个新的版本号
- hotfix分支一旦建立就将独立，不可再从其他分支pull代码

除了是计划外创建的以外，hotfix分支与release分支十分相似：都可以产生一个新的可供在生产环境部署的软件版本。

当生产环境中的软件遇到了异常情况或者发现了严重到必须立即修复的软件缺陷的时候，就需要从master分支上指定的TAG版本派生hotfix分支来组织代码的紧急修复工作。

这样做的显而易见的好处是不会打断正在进行的develop分支的开发工作，能够让团队中负责新功能开发的人与负责代码紧急修复的人并行的开展工作。



![流程图](https://img-blog.csdn.net/20180803135443287?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbmdiYW96aGVuMTIxMA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

------

### 