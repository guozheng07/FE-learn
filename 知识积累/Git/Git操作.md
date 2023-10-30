# 本地切换分支
1. git fetch：拉取远程分支
2. git checkout xx：本地切换到xx分支
# push时，本地版本落后（有其他人提交代码）
1. git pull --rebase origin xx：直接将远程代码合并在本地
2. git commit -m "feat: xx"、git push
# 清空所有未push的工作区改动（包括commit信息）
git reset --hard origin。
# 撤销最近的一次commit，同时保留这次commit中的所有更改
git reset --soft HEAD~1
# 撤销最近的一次commit，并且抛弃这次commit中的所有更改
git reset --hard HEAD~1
# 跳过pre-commit钩子验证（有一些自定义卡控规则，但是不太准确时可以申请跳过）
git commit --no-verify
# 解除项目原来远程仓库的关联
1. 切换到项目的根目录，查看项目原有的remote：git remote -v
2. 解除与原来远程仓库的关联：git remote rm origin
3. 取消git初始化：rm -rf .git
# 回滚
情景1：当前开发分支A已经合并在master中，但是后端那边有问题，一时解决不了进行了回滚，前端为了不影响后续开发分支的上线，也需要回滚。

方法：
1. 手动从master checkout一个新的分支，并push到远端
```
git checkout master
git pull
git checkout -b code-reroll // 本地创建一个回滚分支code-reroll
git push --set-upstream origin code-reroll // 将本地分支推送到远端，并进行跟踪
```
2. 通过git log或可视化的仓库管理平台（mt是code平台）找到上线的merge commit（假设该commit id为“41aaad8e9b3”），进行回滚
```
git revert 41aaad8e9b3 -m 1
```
1. 在回滚分支code-reroll上进行push，用该分支进行上线（merge master）
```
git push
```

情景2：后端问题已修复上线，前端也需要重新上线，但是之间过程中前端已经进行了多次其他需求的上线。

方法：
1. 切换到回滚分支code-reroll，通过git log或可视化的仓库管理平台找到上次回滚的merge commit（假设commit id为“769bbf8040b”），进行再次回滚
```
git revert 769bbf8040b
```
2. 解决冲突，push到远端，重新上线
```
git push
```

