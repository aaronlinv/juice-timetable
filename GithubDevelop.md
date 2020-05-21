1. 版本定义
版本号使用x.x.x进行定义.

第一个x代表大版本只有在项目有重大变更时更新；

第二个x代表常规版本有新求会更新；

第三个x代表紧急Bug修正；

一个常见的版本号类似于：0.10.11

2. 系统开发环境

|简称	                      |全称	                             |作用                                                           |
| :---------------------------| :------------------------------ | :----------------------------------------------------------- |
|DEV	|Devment environment	|用于开发者调试使用|
|FAT	|Feature Acceptance Test environment	|功能验收测试环境，用于测试环境下的软件测试者测试使用|
|UAT	|User Acceptance Test environment	|用户验收测试环境，用于生产环境下的软件测试者测试使用|
|PRO	|Production environment	|生产环境|
3. 分支定义

| 分支	             | 名称	                                            |作用                               |
| :---------------------------| :----------------------------------- | :----------------------------------------------------------- |
|master	|主分支	|用于生产部署，最新稳定版本，一般由 release 或 hotfix 分支合并，任何情况下不允许直接在 master 分支上修改代码。|
|release	|预上线分支	|预上线分支，是dev与master之间的一个缓冲，始终保持与 master 分支一致，一般由 dev 或 hotfix 分支合并，不建议直接在 release 分支上直接修改代码。（UAT）|
|hotfix	|紧急修复分支	|紧急分支，名规则为 hotfix- 开头，从master生成，bug修正后自动合并到master和dev并且生成tag；|
|dev	|测试分支	|功能验收测试环境，用于测试环境下的软件测试者测试使用，可根据需求大小程度确定是由 feature 分支合并，还是直接在上面开发。，FAT，如果开发工时 < 1d，直接在 dev 开发，如果开发工时 > 1d，那就需要创建分支，在分支上开发。|
|feature	|需求开发分支	|用于开发新需求和需要较长时间的BUG修改，(正式环境) 测试通过后，研发人员需要删除 feature- 分支。|
4. Commit 日志规范
提交信息一定要认真填写！
建议参考规范：(scope)：

        比如：fix(首页模块)：修复弹窗 JS Bug。

        type 表示 动作类型，可分为：

        fix：修复 xxx Bug

        feat：新增 xxx 功能

        test：调试 xxx 功能

        style：变更 xxx 代码格式或注释

        docs：变更 xxx 文档

        refactor：重构 xxx 功能或方法

        scope 表示 影响范围，可分为：模块、类库、方法等。

        subject 表示 简短描述，最好不要超过 60 个字，如果有相关 Bug 的 Jira 号，建议在描述中加上。

5. 开发工作流程：
git flow feature start xxxxx（开始新需求）

    在feature/xxxxx分支下进行开发

    git flow feature finish xxxxx（开发完成后等待研发经理确认可以完成时执行）

    git push origin dev（发布dev分支）

    每天工程师都需要git pull origin dev来更新dev分支，然后将dev分支合

    并到你正在开发得feature/xxxxx分支上来保持代码最新

    切记不能直接在dev上进行开发

    5.1 常规分支debug流程：

    由研发经理通知相关工程师release版本x.x
    
    git fetch
    
    git checkout -b release/x.x origin/release/x.x（拉回release版本）
    
    git pull release/x.x（更新该分支）
    
    修改测试中发现的BUG
    
    git push origin release/vx.x（修改完后提交分支）
    
    循环4-5
    
    5.2.紧急debug流程：
    
    由研发经理通知相关工程师hotfix分支名称x.x.x
    
    git fetch
    
    git checkout -b hotfix/x.x.x origin/hotfix/x.x.x（拉回hotfix分支）
    
    git pull hfx.x（更新hotfix分支）
    
    在热修复分支下修改bug
    
    git push origin hfx.x（修改完成，提交分支）
    
    在日常工作中不能修改master分支下得代码
    
    5.3.研发经理：
    
    开发和DEBUG流程同工程师流程

    5.3.1.常规分支debug流程：
    
    git pull origin dev（更新dev分支为最新）
    
    git checkout dev（切换到dev分支）
    
    git flow release start x.x（生成一个release分支）
    
    通知测试和相关得工程师分支名称
    
    git pull origin release/x.x（最终测试完成后拉回分支最新代码）
    
    git flow release finish x.x（最终修改和测试完成后，结束release版本以供发
    布）
    
    git push origin develo (发布最新的dev)
    
    git push origin master（发布最终得master分支）
    
    5.3.2紧急debug流程：
    
    git pull origin master（更新master分支为最新）
    
    git checkout master（切换到master分支）
    
    git flow hotfix start x.x.x（生成一个hotfix分支）
    
    通知相关得工程师和测试人员hotfix分支名称
    
    git pull origin hotfix/x.x.x（最终测试完成后拉回分支最新代码）
    
    git flow hot fix finish x.x.x（最终修改和测试完成后，结束hot fix以供发布）
    
    git push origin master（发布最终得master分支）
    
    在全部的流程中，工程师必须维护自己的feature分支保证代码最新，减少合并时的冲突。
    研发经理必须维护release分支，将最新的hotfix都合并进去，保证代码最新，减少合并时的冲突。

在提交代码时还要注意判断对代码的修改是否是自己的，多用diff工具，多查看log，防止代码回溯