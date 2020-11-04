---
title:  Git 分支管理规范
toc: true
categories: [前端, 规范]
tags: [规范]
date: 2018-05-10 08:09:12
cover: /images/covers/23.webp
---
## 1.流程
![A8F24363B9FA1D4218AAC95A23A5BCB0.jpg](https://cdn.nlark.com/yuque/0/2020/jpeg/85733/1590135939279-e5cb9b27-4677-435b-a0dd-73f14ce45ea8.jpeg#align=left&display=inline&height=1660&margin=%5Bobject%20Object%5D&name=A8F24363B9FA1D4218AAC95A23A5BCB0.jpg&originHeight=1660&originWidth=2066&size=468446&status=done&style=none&width=2066)


Action 说明：

1. 从master分支拉取对应的feature和dev分支，分支名：feature_需求编号，版本号：master版本号_时间戳_SNAPSHOT
1. 当master代码发生变更后，需merge到仍在开发测试中的feature分支
1. 从master分支拉取对应的hotfix分支，分支名：hotfix_BUG_11【hotfix_XG_99】，版本号：master版本号中z+1【如：master分支版本号为1.1.5，则hotfix分支版本号为1.1.6】
1. feature分支部分功能或全部功能已开发联调完成提测
1. feature分支在测试过程中修复BUG的过程
1. 当dev或hotfix分支对应的需求测试完成后准备进入预发布阶段时，从此dev_XG_26或hotfix拉取对应的release_XG_26分支并修改版本号，发布到UAT环境进行回归测试
1. release分支在UAT回归测试过程中发现BUG进行修复，或者有已评估通过的紧急需求需要进行开发的过程
1. UAT回归验证通过后发布生产
1. 提交merge request并找对应的模块负责人Code Review
1. 已上线的分支合并到master分支并打Tag



待优化的点：

1. 当Jira 需求状态变更为_开始开发_时，就自动执行1⃣️操作。
1. 当Jira 上需要紧急发布的需求或紧急修复的BUG状态变更为_开始开发_时，就执行3⃣️操作。
1. 当feature分支对应的需求在Jira上状态变更为_提测_时，就执行9⃣️操作。
## 2.分支说明
### master 分支【线上分支】

- master 分支时常保持着软件可以正常运行的状态。由于要维护这一状态，所以不允许开发者直接对master 分支的代码进行修改和提交。
其他分支的开发工作进展到可以发布的程度后，将会与master分支进行合并，并且这一合并只在发布成品时进行。发布时将会附加版本编号的Git标签。

### feature分支【功能分支】

- feature 分支以master分支为起点，是开发者直接进行功能开发的分支。开发流程：
   1. 从master分支创建feature分支，分支名：feature_需求编号，版本号：master版本号_时间戳_SNAPSHOT
   1. 从feature分支中实现目标功能
   1. 通过GitLab 向dev分支发起merge request
   1. 接受其他开发者审核后，将merge request合并至dev分支
   1. 与dev分支合并后，开发人员发送提测邮件
   1. 测试人员将此dev分支发布至测试环境，开始测试
- 注意：每天早上将最新的master代码合并到feature分支时，记得一定要先git checkout master、git pull origin master，保证master分支处于最新状态。
### dev分支【提测分支】

- dev分支是提测分支，dev与对应的feature分支需同时从master中拉取出来，待feature分支功能开发完成后，将feature分支代码合并到dev分支，开始提测。
与master 分支一样，这个分支也不允许开发者直接进行修改和提交。
在多个版本需求并行测试的过程中，此分支会存在多个。

### release分支【发布分支】

- 创建 release分支 ，在这个分支，我们只处理与发布前准备相关的提交，比如变更版本号。如果已经部署到UAT后测试出bug，相关修正也要提交到这个分支。
注意：该分支绝对不能包含功能变更等重大修正。这一阶段的提交数应该限制到最低。

### hotfix分支【紧急修复分支】

- hotfix是从master或release分支上拉取出来的分支，主要用于线上紧急需求开发或紧急问题修复或预发环境的BUG修复。hotfix开发完成后，需要向release提交merge request。
下述情况需要创建 hotfix 分支：
1. 线上发生漏洞需要及早修复
2. 紧急需求无法等到下一个版本一起发布
3. 预发环境发现的BUG修复

## 3. 标签和发布
      release分支每次发布完成后将代码合并到master分支，再从master分支上创建当前版本的tag，填写此版本相关发布内容。
## 4. 版本号分配规则
     版本号的分配规则 x.y.z
     x: 在重大功能变更，或者版本不向下兼容+1，此时y、z归零
     y: 在添加新功能或者删除已有功能+1 此时z归零
     z: 紧急发布为奇数，日常周迭代为偶数
