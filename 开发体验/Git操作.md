# 本地切换分支
1. git fetch：拉取远程分支
2. git checkout xx：本地切换到xx分支
# push时，本地版本落后（有其他人提交代码）
1. git pull --rebase origin xx：直接将远程代码合并在本地
2. git commit -m "feat: xx"、git push