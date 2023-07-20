# 本地切换分支
1. git fetch：拉取远程分支
2. git checkout xx：本地切换到xx分支
# push时，本地版本落后（有其他人提交代码）
1. git pull --rebase origin xx：直接将远程代码合并在本地
2. git commit -m "feat: xx"、git push
# 清空所有未push的工作区改动（包括commit信息）
git reset --hard origin。
# 解除项目原来远程仓库的关联
1. 切换到项目的根目录，查看项目原有的remote：git remote -v
2. 解除与原来远程仓库的关联：git remote rm origin
3. 取消git初始化：rm -rf .git